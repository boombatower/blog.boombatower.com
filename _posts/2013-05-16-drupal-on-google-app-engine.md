---
created: 1368677000
layout: post
redirect_from:
- article/94/
- node/94/
- drupal-google-app-engine/
title: Drupal on Google App Engine
---
## [For the latest information see newer post](/drupal-integration-module-google-app-engine)

Today Google [announced PHP support for Google App Engine](http://googlecloudplatform.blogspot.com/2013/05/ushering-in-next-generation-of.html)! I have been one of the lucky folks who had early access and so of course I worked on getting [Drupal](http://drupal.org/) up and running on GAE. There are a few things that still need to be worked out which I will continue to discuss with the app engine team, but I have a [working Drupal setup](https://boombatower-drupal.appspot.com/) which I will detail below. Note that much of this may also apply to other PHP frameworks.

# Getting up and running
I will cover the steps specific to getting Drupal 7 *(notes for Drupal 6 along with branches in repository)* up and running on App Engine and not how to use the SDK and development flow which is detailed in the documentation. For an example (minimal profile from core) of Drupal running on Google App Engine see [boombatower-drupal.appspot.com](https://boombatower-drupal.appspot.com/).

## Sign up to be whitelisted for PHP runtime

Currently, the PHP runtime requires you to [sign up specifically for access](https://gaeforphp.appspot.com/). Assuming you have access you should be able to follow along with the steps below. Otherwise, the following steps will give you a feel for what it takes to get Drupal running on GAE.

## Create an app
Create app by visiting [appengine.google.com](https://appengine.google.com/) and clicking *Create Application*, see the [documentation for more details](https://developers.google.com/appengine/docs/adminconsole/index).

![Create an Application](/files/appengine-create-app.png)

## Create a Cloud SQL Instance
Follow the [documentation for setting up a Cloud SQL Instance](https://developers.google.com/cloud-sql/docs/before_you_begin). Be sure to give your application access to the instance.

![Create a Cloud SQL Instance](/files/appengine-create-sql-instance.png)

Once the instance has been created select the *SQL Prompt* tab and create a database for your Drupal site as follows.
```sql
CREATE DATABASE drupal;
```

![Create a Cloud SQL Database](/files/appengine-create-sql-database.png)

## Download Drupal

There are a [few tweaks](https://github.com/boombatower/drupal-appengine/compare/7.x...7.x-appengine) that need to be made to get Drupal to run properly on GAE which are explained below, but for the purposes of this walk-through one can simply download my branch containing all the changes from [github](https://github.com/boombatower/drupal-appengine).

```bash
git clone --branch 7.x-appengine https://github.com/boombatower/drupal-appengine.git

# or for Drupal 6
git clone --branch 6.x-appengine https://github.com/boombatower/drupal-appengine.git
```

or [download as a zip](https://github.com/boombatower/drupal-appengine/archive/7.x-appengine.zip) or for Drupal 6 [download as a zip](https://github.com/boombatower/drupal-appengine/archive/6.x-appengine.zip).

## Configure Drupal database settings
Since GAE does not allow the filesystem to be writeable one must configure the database settings ahead of time.

Copy *default.settings.php* as *settings.php* and add the following bellow `$databases = array();` around line *213*.

```php
$databases = array();
$databases['default']['default'] = array(
  'driver' => 'mysql',
  'database' => 'drupal', // The database created above (example used 'drupal').
  'username' => 'root',
  'password' => '',
  // Setting the 'host' key will use a TCP connection which is not supported by GAE.
  // The name of the instance created above (ex. boombatower-drupal:drupal).
  'unix_socket' => '/cloudsql/[INSTANCE]',
//  'unix_socket' => '/cloudsql/boombatower-drupal:drupal',
  'prefix' => '',
);
```

For Drupal 6 around line 91.

```php
$db_url = 'mysql://root:@cloudsql__boombatower-drupal___drupal/drupal';
```

## Push to App Engine

Update the *application* name in the [app.yaml](https://github.com/boombatower/drupal-appengine/blob/7.x-appengine/app.yaml) file to the one you created above and upload by following the [documentation](https://developers.google.com/appengine/docs/php/gettingstarted/uploading).

```yaml
# See https://developers.google.com/appengine/docs/php/config/appconfig.

application: drupal # <-- change this to your application
version: 1
runtime: php
api_version: 1
threadsafe: true

handlers:
# Default handler for requests (wrapper which will forward to index.php).
- url: /
  script: wrapper.php

# Handle static requests.
- url: /(.*\.(ico$|jpg$|png$|gif$|htm$|html$|css$|js$))
  # Location from which to serve static files.
  static_files: \1
  # Upload static files for static serving.
  upload: (.*\.(ico$|jpg$|png$|gif$|htm$|html$|css$|js$))
  # Ensures that a copy of the static files is left for Drupal during runtime.
  application_readable: true

# Catch all unhandled requests and pass to wrapper.php which will simulate
# mod_rewrite by forwarding the requests to index.php?q=...
- url: /(.+)
  script: wrapper.php
```

```bash
appcfg.py update drupal/
```

## Install

Visit [your-app.appspot.com/install.php](https://your-app.appspot.com/install.php) and follow the installation steps just as you would normally except that the database information will already be filled in. Go ahead and ignore the mbstring warning and note that the GAE team is looking into supporting [mbstring](http://php.net/manual/book.mbstring.php).

# Explanation of changes

If you are interested in what changes/additions were made and the reasons for them continue reading, otherwise you should have a working Drupal install ready to explore! There are a few basic things that do not work perfectly out of the box on GAE. The changes can be seen by [diffing the 7.x-appengine branch against the 7.x branch](https://github.com/boombatower/drupal-appengine/compare/7.x...7.x-appengine) in my repository.

##File directory during installation

The Drupal installer requires that the files directory be writeable, but GAE does not allow for local write access thus the requirement must be bypassed in order for the installation to complete.

```patch
Author: boombatower <boombatower@google.com>
Date:   Wed May 15 15:49:03 2013 -0700

    Hack to trick Drupal into ignoring that file directory is not writable.

diff --git a/modules/system/system.install b/modules/system/system.install
index 1b037b8..9931aad 100644
--- a/modules/system/system.install
+++ b/modules/system/system.install
@@ -333,6 +333,8 @@ function system_requirements($phase) {
     }
     $is_writable = is_writable($directory);
     $is_directory = is_dir($directory);
+    // Force Drupal to think the directories are writable during installation.
+    $is_writable = $is_directory = TRUE;
     if (!$is_writable || !$is_directory) {
       $description = '';
       $requirements['file system']['value'] = $t('Not writable');
```

## Clean URLs

In order to take advantage of clean urls, of which most sites take advantage, [mod_rewrite](http://httpd.apache.org/docs/current/mod/mod_rewrite.html) is required for [Apache](http://httpd.apache.org/) environments. Since GAE does not use Apache it does not support mod_rewrite and thus another solution is needed. The app.yaml can configure handlers which allow for wildcard matching which means multiple paths can easily be routed to a single script. Taking that one step further we can alter the ```php $_GET['q']``` variable just as mod_rewrite would so that Drupal functions properly. Rather than modify core this can be done via a [wrapper](https://github.com/boombatower/drupal-appengine/blob/7.x-appengine/wrapper.php) script as show below (this should work well for other PHP applications).

```php
/**
 * @file
 * Provide mod_rewrite like functionality and correct $_SERVER['SCRIPT_NAME'].
 *
 * Pass through requests for root php files and forward all other requests to
 * index.php with $_GET['q'] equal to path. In terms of how the requests will
 * seem please see the following examples.
 *
 * - /install.php: install.php
 * - /update.php?op=info: update.php?op=info
 * - /foo/bar: index.php?q=/foo/bar
 * - /: index.php?q=/
 */

$path = parse_url($_SERVER['REQUEST_URI'], PHP_URL_PATH);

// Provide mod_rewrite like functionality. If a php file in the root directory
// is explicitely requested then load the file, otherwise load index.php and
// set get variable 'q' to $_SERVER['REQUEST_URI'].
if (dirname($path) == '/' && pathinfo($path, PATHINFO_EXTENSION) == 'php') {
  $file = pathinfo($path, PATHINFO_BASENAME);
}
else {
  $file = 'index.php';

  // Provide mod_rewrite like functionality by using the path which excludes
  // any other part of the request query (ie. ignores ?foo=bar).
  $_GET['q'] = $path;
}

// Override the script name to simulate the behavior without wrapper.php.
// Ensure that $_SERVER['SCRIPT_NAME'] always begins with a / to be consistent
// with HTTP request and the value that is normally provided (not what GAE
// currently provides).
$_SERVER['SCRIPT_NAME'] = '/' . $file;
require $file;
```

## PHP $_SERVER['SCRIPT_NAME'] variable

The `$_SERVER['SCRIPT_NAME']` implementation differs from Apache mod_php implementation which can cause issues with a variety of PHP applications. The variable matches the HTTP spec and not the filesystem when called through Apache.

For example a script named *foo.php* contains the following.
```php
var_dump($_SERVER['SCRIPT_NAME']);
```

When executed from command line here are the results.

```bash
$ php foo.php
string(7) "foo.php"

$ php ./foo.php
string(9) "./foo.php"
```

When invoked through Apache like *http://example.com/foo.php*.

```bash
string(8) "/foo.php"
```

The [documentation](http://php.net/manual/en/reserved.variables.server.php) does not talk about this behavior (although many comments demonstrated the expected Apache behavior), but it is definitely depended on.

The difference causes Drupal to format invalid URLs.

```
example.com.foo.css (instead of ...com/foo.css)
example.comsubdir/foo.css (instead of ...com/subdir/foo.css)
```

Drupal derives the URL from `dirname() ``` of ```php $_SERVER['SCRIPT_NAME']` which will return . if no slashes or just / for something like /index.php.

The wrapper script above solves this by ensuring that the *SCRIPT_NAME* variable alway starts with a leading slash.

## HTTP requests

GAE does not yet support support outbound sockets for PHP (although supported for Python and Java) and if/when it does the preferred way will continue to be [streams](https://developers.google.com/appengine/docs/php/urlfetch/overview) due to automatic caching of outbound requests using urlfetch. I have included a small change to provide basic HTTP requests through drupal_http_request(). A proper solution would be to override the drupal_http_request_function variable and provide a fully functional alternative using streams. Drupal 8 has [converted drupal_http_request()](http://drupal.org/node/1447736) to use [Guzzle](http://guzzlephp.org/) which supports streams. Making a similar conversion for Drupal 7 seems like the cleanest way forward rather than reinventing the change.

## php.ini

GAE [disables a number of functions for security reasons](https://developers.google.com/appengine/docs/php/runtime#Function-Support), but only *softly disables* some functions which may then be enabled. Drupal provides access to *phpinfo()* from *admin/reports/status* and uses output buffering, both of which are disabled by default. The included [php.ini](https://github.com/boombatower/drupal-appengine/blob/7.x-appengine/php.ini) enables both functions in addition to *getmypid* which is used by *drupal_random_bytes()*.

```ini
# See https://developers.google.com/appengine/docs/php/config/php_ini.

# Required for ob_*() calls which you can find by grepping.
# grep -nR '\sob_.*()' .
output_buffering = "1"

# See https://developers.google.com/appengine/docs/php/runtime#Functions-That-Must-Be-Manually-Enabled
# phpinfo: Provided on admin/reports/status under PHP -> "more information".
# getmypid: Used by drupal_random_bytes(), but not required.
google_app_engine.enable_functions = "getmypid, phpinfo"
```

# Future

I plan to continue working with the GAE team to ensure that support for Drupal can be provided in a clean and simple manner. Once current discussions have been resolved I hope to provide more formal documentation and support for Drupal.

## File handling

I worked on file support, but there were a number of upcoming changes that would make things much cleaner so I decided to wait. GAE provides a stream wrapper for [Google Cloud Storage](https://cloud.google.com/products/cloud-storage) which makes using the service very simple. Assuming you have [completed the prerequisites](https://developers.google.com/appengine/docs/php/runtime#Function-Support) files on GCS may be accessed using standard PHP file handling functions as shown in the [documentation](https://developers.google.com/appengine/docs/php/googlestorage/overview).

```php
$file = 'gs://my_bucket/hello.txt';
file_put_contents($file, 'hello world');

$contents = file_get_contents($file);
var_dump($contents); // prints: hello world
```

Unfortunately, the wrapper does not currently support directories nor does *file_exists()* work properly. Keep in mind that the filesystem is flat so a file may be written to any path without explicitly creating the directory. Meaning one can write to *gs://bucket/foo/bar.txt* without creating the directory *foo*. With that being the case it is possible to get some hacky support by simply disabling all the directory code in Drupal, but not really usable. It should be possible to hack support in through the stream wrapper since [directories are simply specially name files](https://developers.google.com/storage/docs/gsmanager#creatingfolders), but the app engine team has indicated they will look into the matter so hopefully this will be solved cleanly.

Assuming the stream wrappers are fixed up then support can be added in much the same way as that [Amazon S3 support is added](http://drupal.org/project/amazons3) except that no additional library will be needed.

Additionally, the documentation also notes the following.

> Direct file uploads to your POST handler, without using the App Engine upload agent, are not supported and will fail.

In order to support file uploads the form must be submitted to the url provided by *CloudStorageTools::createUploadUrl()* and the forwarded result handled by Drupal. A benefit of proxying requests through uploader service is that uploaded files may be up to *100TB* in size.

## Other

There are a number of additional services provided as part of GAE of which Drupal could take advantage.

- [Task Queue API](https://developers.google.com/appengine/docs/php/taskqueue/)
- [Google account authentication](https://developers.google.com/appengine/docs/php/users/)
- [Memcache](https://developers.google.com/appengine/docs/php/memcache/) seems to work out of the box with the [memcache module](http://drupal.org/project/memcache)
- `$_SERVER['HTTP_X_APPENGINE_*']` variables (like geo information)
Large number of services in [API Console](https://code.google.com/apis/console/)

# Closing

Hopefully this will be useful in getting folks up and running quickly on GAE with Drupal and understanding the caveats of the environment. Obviously there is a lot more to cover and I look forward to seeing what others publish on the matter.
