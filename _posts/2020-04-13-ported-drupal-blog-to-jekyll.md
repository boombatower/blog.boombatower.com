---
layout: post
title: Ported Drupal blog to Jekyll
---

Reworking my blog as a static site has been something I have been toying with for several years, but didn't commit to seeing it through. I imagine a large part of this was due to falling out of writing posts for a number of reasons. For several months I enjoyed writing posts on a [static blog covering my openSUSE work](http://release-tools.opensuse.org/), but that also made it easier to avoid updating my blog.

I have finally completed the process and pushed out a blog based on [Jekyll](https://jekyllrb.com/). The process certainly was not as straight forward as I expected and there are a decent number of gotchas. Since the conversion principally took place over the last year (no more than a few days work in total) I do not remember all the bits involved in converting it, but I'll document the ones that I do remember in the hopes of saving someone else a bit of time.

Previously, my blog was built using [Drupal](http://drupal.org/) 7, before that Drupal 6, and before that [Blogger](https://www.blogger.com/). As such the content is not designed for any of those systems, but rather a mix. I decided to try and avoid fully porting the content and instead only port the bits that necessitated it.

For posterity, here is the _Drupal 7_ and new _Jekyll_ blog at the time of this writing.

---

## Drupal 7

![Drupal 7 blog](/files/2020-04-13/blog-drupal.png)

---

## Jekyll

![Jekyll blog](/files/2020-04-13/blog-jekyll.png)

---

## Initial conversion

Some posts and tooling exist to convert a Drupal site to Jekyll. Using the [`jekyll-import`](https://github.com/jekyll/jekyll-import) gem as a basis I started with the following against a database dump of my Drupal 7 blog.

```bash
#!/bin/bash

if [ $# -ne 1 ] ; then
  echo "must specify a database dump to convert"
fi
echo "converting $1..."

docker run --detach \
  --name jekyll-mariadb \
  -e MYSQL_ROOT_PASSWORD=jekyll \
  -e MYSQL_DATABASE=jekyll \
  mariadb

sleep 15

docker cp "$1" jekyll-mariadb:/import.sql

docker exec \
  jekyll-mariadb \
  mysql -uroot -pjekyll jekyll \
  -e"source import.sql"

docker run --detach \
  --name jekyll-ruby \
  --link jekyll-mariadb:mariadb \
  ruby \
  /bin/bash -c "sleep 1000000"

docker exec \
  jekyll-ruby \
  /bin/bash -c "
gem install jekyll-import mysql2 sequel pg &&
mkdir -p /convert &&
cd /convert &&
ruby -rrubygems -e 'require \"jekyll-import\";
  JekyllImport::Importers::Drupal7.run({
    \"dbname\"   => \"jekyll\",
    \"user\"     => \"root\",
    \"password\" => \"jekyll\",
    \"host\"     => \"mariadb\",
    \"prefix\"   => \"\",
    \"types\"    => [\"blog\", \"story\", \"article\"]
  })'
"

docker cp jekyll-ruby:/convert convert

docker rm -f jekyll-mariadb
docker rm -f jekyll-ruby
```

That produced a rough Jekyll site containing all of my content which provided a solid basis from which to iterate.

## Site generation

I played around with a variety of the installed and dockerized Jekyll setups, but found them all rather lacking and cumbersome. I ended up [pushing everything to Github](https://github.com/boombatower/blog.boombatower.com) (`blog-test` subdomain prior to completion) to avoid the mess as that was my target destination anyway.

Using a `CNAME` file in the repository root and configured DNS to point to `boombatower.github.io` I was able to view the site.

## Content cleanup

Given the posts on my blog date back to _2007_ it, rather reasonably, turned out that a number of the posts embedded content that no longer existed. Additionally, some content had moved and while other worked it was deprecated or really should be self hosted.

First, I downloaded all the files still referencing `blogger.com` and replaced the references with self-hosted ones using the following script.

```bash
#!/bin/bash

PREFIX='files/blogger'

for url in $(grep -hRoP 'http://[^/]+blogger.com/[^"]+' _posts/) ; do
  filename="$(basename "$url")"
  if [[ $url == *"-h"* ]] ; then
    url="$(echo "$url" | sed 's|-h||')"
  else
    filename="$(echo "$filename" | sed 's|.png|.thumbnail.png|')"
  fi

  echo $filename $url
  wget -O "$PREFIX/$filename" "$url"
  find _posts/ -type f -exec sed -i "s|$url|$PREFIX/$filename|" {} \;
done
```

Reviewing posts I replaced dead content with short text which provided, for _posterity_, a reference to what was there.

Some embedded content that no longer worked (like [Youtube](https://youtube.com/) videos and a Picasa Web Album (now [Google Photos](http://photos.google.com/)) required an updated embedded format.

A few external images referencing other blogs had been updated and had the URLs changed. With a bit of searching I was able to find the images download them and replaced the references.

One image was utilizing the long deprecated _Google Charts API (image based)_, but Google apparently kept the service going so another download and replace.

## Redirects

One of the problems I ran into was broken links due to URLs either changing or being removed. I was determined not to contribute to broken links by ensuring all the existing URLs on my blog continued working regardless of the new canonical URLs. While the `jekyll-import` gem properly exported the various paths for a given post from Drupal it represented them as markdown files at the given path with instructions on where to redirect. For example, `acquia-internship.md`:

```markdown
---
layout: refresh
permalink: acquia-internship/
refresh_to_post_id: /2009/06/15/acquia-internship
---
```

Similarly, `node/61.md`:

```markdown
---
layout: refresh
permalink: article/61/
refresh_to_post_id: /2009/06/15/acquia-internship
---
```

Overall, this was a bunch of files and not easy to manage aliases. Rather I was looking for a list of aliases in the actual post file. Thankfully, I found the [`jekyll-redirect-from`](https://github.com/jekyll/jekyll-redirect-from) gem. After adding to list of gems in `_config.yml` the following worked from within a post:

```markdown
---
redirect_from:
  - /somethingrandom
  - /node/123456789
---
```

I then wrote the following script to automate the conversion.

```python
#!/usr/bin/env python3

import os
import yaml

directories = ['article', 'node', '.']
redirects = {}
for directory in directories:
    for entry in os.listdir(directory):
        if not entry.endswith('.md'):
            continue

        path = os.path.join(directory, entry)
        print('processing {}...'.format(path))
        with open(path) as f:
            parts = f.read().split('---')
            header = yaml.safe_load(parts[1])
            if not('permalink' in header and 'refresh_to_post_id' in header):
                continue

            post = os.path.join(
                '_posts', header['refresh_to_post_id'].lstrip('/').replace('/', '-') + '.md')

            redirects.setdefault(post, [])
            redirects[post].append(header['permalink'])

        os.remove(path)

print('writing {} redirects'.format(len(redirects)))

for post, paths in redirects.items():
    with open(post, 'r') as f:
        parts = f.read().split('---')

    with open(post, 'w') as f:
        parts[1] += 'redirect_from:\n- {}\n'.format('\n- '.join(paths))
        f.write('---'.join(parts))
```

## Categories

After a bunch of exploration I ended up avoiding the categories and tags. Both seem to require a fair bit of configuration and a theme that support them. My goal was to keep this blog simple and most people will find my post from a search engine instead of perusing my blog.

Using a derivation of the above script I removed all the category information (tags from Drupal).

```python
#!/usr/bin/env python3

import os
import yaml

directory = '_posts'
for entry in os.listdir(directory):
    if not entry.endswith('.md'):
        continue

    path = os.path.join(directory, entry)
    print('processing {}...'.format(path))
    with open(path) as f:
        parts = f.read().split('---')

    header = yaml.safe_load(parts[1])
    if 'categories' in header:
        del header['categories']

    # Force reformat all.
    parts[1] = '\n' + yaml.dump(header)

    with open(path, 'w') as f:
        f.write('---'.join(parts))
```

## Syntax

Since many of the posts were written in using _HTML_ instead of _Markdown_ they had unfamiliar syntax highlighting tags. After incrementally finding them all I ended up with the following script to make the conversion. Nothing pretty, but does the trick.

```python
#!/usr/bin/env python3

import os
import re

replacements = {
    '<?php': '```php',
    '?>': '```',
    '<bash>': '```bash',
    '</bash>': '```',
    '<mysql>': '```sql',
    '</mysql>': '```',
    '<code>': '```',
    '</code>': '```',
    '<diff>': '```diff',
    '</diff>': '```',
    '<yaml>': '```yaml',
    '</yaml>': '```',
    '<ini>': '```ini',
    '</ini>': '```',
    '<css>': '```css',
    '</css>': '```',
}

directory = '_posts'
for entry in os.listdir(directory):
    if not entry.endswith('.md'):
        continue

    path = os.path.join(directory, entry)
    print('processing {}...'.format(path))
    with open(path) as f:
        parts = f.read().split('---', 2)

    body = parts[2]

    # check for inline php first
    body = re.sub(r'<\?php (.*) \?>', r'`\1`', body)
    body = re.sub(r'<code>([^<\n]+)</code>', r'`\1`', body)
    body = re.sub(r'"`([^"]+)`"', r'`\1`', body)

    for old, new in replacements.items():
        body = body.replace(old, new)

    if body == parts[2]:
        continue

    parts[2] = body

    with open(path, 'w') as f:
        f.write('---'.join(parts))
```

## Custom formatting

Some of my posts contained special inline image formatting where the image was contained within a `<div>` containing a caption. The _div_ had `float: right` applied via CSS. Due to the image sizes and reduced width of the site I removed most of the formatting and some of the captions. Not ideal, but I expect little traffic on the old content and this leaves the most more than readable.

## Comment removal

A number of useful and positive comments had been left on my blog over the years, but a large number were ill conceived or blindly negative. Given I am the one maintaining providing things for free and gain next to nothing from comments I decided to drop them entirely. If folks want to contact me good 'ol email works just fine.

## Github pages minima version

It does not seem readily documented, if at all, but [Github pages](https://pages.github.com/) uses `minima` version `2.x` instead of the stable `3.x` which means one needs to read the documentation from the relevant branch instead of `master`. The `3.x` version changed a number of the configuration option keys along with the location for custom CSS which is `assets/main.scss` in the `2.x` version.

## Closing

Overall, I am satisfied with the result and look forward to writing up some technical posts like this one.
