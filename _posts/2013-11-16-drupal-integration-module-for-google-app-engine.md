---
created: 1384560142
layout: post
redirect_from:
- article/95/
- node/95/
- drupal-integration-module-google-app-engine/
title: Drupal integration module for Google App Engine
---
Since posting [Drupal on Google App Engine](/drupal-google-app-engine) the App Engine team has been working hard to identify and improve many of the troublesome areas identified in the original post, other external sources, and further internal discussions. In addition, I have developed a [Drupal integration module for Google App Engine](https://drupal.org/project/google_appengine). The combination of the integration module and the work done internally provides a more compelling Drupal experience on App Engine.

For those of you who are not sure [what features App Engine provides](https://developers.google.com/appengine/features) or [why to consider App Engine](https://developers.google.com/appengine/whyappengine) have a look at the reference material. Getting started is also easier than ever as the whitelist has been removed and the SDK comes bundled with PHP. The rest of this post will focus on what improvements have been made, what the integration provides, and how to make use of it.

To see a functioning Drupal site making use of the App Engine module and the Memcache module see my [demo site](https://boombatower-drupal.appspot.com/). The demo site includes an example of a file field served out of Google Cloud Storage.

*If you are just interested in making use of the integration visit the [Google App Engine Drupal project](https://drupal.org/project/google_appengine), have a look at the included [README](http://drupalcode.org/project/google_appengine.git/blob_plain/refs/heads/7.x-1.x:/README.txt) and/or the Getting started section of this post. The second half covers some technical details on the implementation for those who are interested.*

# Getting started

The preferred method of developing for App Engine is to work locally [using the SDK](https://developers.google.com/appengine/docs/php/gettingstarted/installing) and the included development server. Once the site is ready it can be [uploaded to App Engine](https://developers.google.com/appengine/docs/php/gettingstarted/uploading).

You can choose to skip the local development setup and work directly on App Engine, but you will still need the SDK setup for uploading the app.
Regardless, there are a number of ways to get a hold of Drupal and the integration module.

- all-in-one download
- drush make
- manual

## All-in-one download

Simply download the [full release](https://github.com/boombatower/drupal-appengine/releases/tag/7.x-appengine-full-1.0) containing a patched Drupal core, Google App Engine module, and [Memcache](http://drupal.org/project/memcache) module.

Extract the files and enjoy.

## Drush make

The App Engine module includes [Drush](https://github.com/drush-ops/drush) make scripts. There are two profiles: minimal using drupal.make; and full (to include more going forward) using drupal-full.make. Only the latter contains the memcache module.

Depending on which profile is desired, invoke the appropriate command.

```bash
$ drush make http://drupalcode.org/project/google_appengine.git/blob_plain/refs/heads/7.x-1.x:/root/drupal-full.make
$ drush make http://drupalcode.org/project/google_appengine.git/blob_plain/refs/heads/7.x-1.x:/root/drupal.make
```

## Manual

Obviously, the components can be downloaded and manually assembled as well.

# Drupal installation

Follow the [normal process for installing Drupal](https://drupal.org/documentation/install). Keep in mind that the Drupal files will not be writable on App Engine so any changes to settings.php or any other modules configuration will need to be made prior to upload. See the *SETTINGS.PHP* section of the [README](http://drupalcode.org/project/google_appengine.git/blob_plain/refs/heads/7.x-1.x:/README.txt) for details on setting up the settings.php file for development against the local server and production App Engine.

The gist of the comments is to use the following for database credentials, filling in the {} sections.

```php
if(strpos($_SERVER['SERVER_SOFTWARE'], 'Google App Engine') !== false) {
  // Cloud SQL database credentials.
  $databases['default']['default'] = array(
    'database' => '{DATABASE}',
    'username' => 'root',
    'password' => '',
    'unix_socket' => '/cloudsql/{SOME_PROJECT}:{DATABASE}',
    'port' => '',
    'driver' => 'mysql',
    'prefix' => '',
  );
}
else {
  // Local database credentials.
  $databases['default']['default'] = array(
    'database' => '{DATABASE}',
    'username' => '{USERNAME}',
    'password' => '{PASSWORD}',
    'host' => 'localhost',
    'port' => '',
    'driver' => 'mysql',
    'prefix' => '',
  );
}
```

## Memcache module

If you choose to make use of the Memcache module be sure to follow the [setup instructions](https://drupal.org/node/1131468). For the default setup simply add the following lines to the bottom of settings.php.

```php
$conf['cache_backends'][] = 'sites/all/modules/memcache/memcache.inc';
// The 'cache_form' bin must be assigned no non-volatile storage.
$conf['cache_class_cache_form'] = 'DrupalDatabaseCache';
$conf['cache_default_class'] = 'MemCacheDrupal';
$conf['memcache_key_prefix'] = 'something_unique';
```

Additionally, a [patch](https://gist.github.com/boombatower/7480017/raw/565456241f5c771d4784a9ef630d79f84c53fc8e/memcache-compressed.patch) should be applied to define `MEMCACHE_COMPRESSED` which is missing from the App Engine implementation (fix upcoming).

## App Engine module

To make use of the integration enable the App Engine module. In order to use Google Cloud Storage be sure to configure the default storage bucket by visiting *admin/config/media/file-system* in your Drupal site.

![GCS settings](/files/gcs-settings.png)

If you choose to enable CSS/JS aggregation be sure to read through the serving options on *admin/config/development/performance* and choose the one that best suites your workflow.

![GCS settings](/files/gcs-aggregate.png)

# Importing

If you are looking to import an existing site into Google App engine take a look at the following documentation links.

- [Cloud SQL - Importing and Exporting Data](https://developers.google.com/cloud-sql/docs/import-export): for import existing Drupal database
- [gsutil](https://developers.google.com/storage/docs/gsutil) [copy command](https://developers.google.com/storage/docs/gsutil/commands/cp): for importing files directory to GCS

Be sure to add and enable the App Engine module to the existing code base.

# Support

If you encounter any difficulties please let us know via the appropriate channel.

- For general App Engine (PHP) support please visit [Stackoverflow](http://stackoverflow.com/questions/tagged/google-app-engine+php)
- For issues specific to this Drupal module please visit the [issue queue](https://drupal.org/project/issues/google_appengine)

# Integration details

The rest of the post will discuss the implementation details behind the integration. The features provided by the [1.0 release of the App Engine module](https://drupal.org/node/2131979) are as follows. See [previous blog post](/drupal-google-app-engine) for details on what led up to this work.

- App Engine mail service
- Cloud Storage
- Drupal core patch

## App Engine mail service

Implements Drupal [MailSystemInterface](http://api.drupal.org/MailSystemInterface) to make use of the App Engine mail service. The system email address will be used as the default from address and must be authorized to send mail. To configure the address, visit admin/config/system/site-information. For details on App Engine mail service, read [this document](https://developers.google.com/appengine/docs/php/mail/).

For further details see the [mail integration code](http://drupalcode.org/project/google_appengine.git/blob/HEAD:/google_appengine.mail.inc).

## Cloud Storage

The GAE team has provided a PHP [stream wrapper](http://php.net/streamwrapper) which allows the use of standard PHP [file handling functions](http://php.net/file) for interacting with GCS. The current implementation requires a storage bucket in the file path which means applications must be altered to not only make use of the stream wrapper, but include a bucket in all file paths. Additionally, Drupal requires the implementation of an additional set of methods ([DrupalStreamWrapperInterface](http://api.drupal.org/DrupalStreamWrapperInterface)) on top of the default set required for all PHP stream wrappers.

Instead of attempting to provide a format that allows the bucket to be optionally included, the best route forward is to provide a bucketless stream wrapper that always assumes no bucket is included and instead uses a default bucket. The new stream wrapper would sit atop the default stream wrapper and add the default bucket to all paths before handing off to the parent implementation. For lack of anything more descriptive the letter *b* (for bucketless) was appended to the *gs* stream wrapper. The examples below demonstrate the usage.

- gs://defaultbucket/dir1/dir2/file
- gsb://dir1/dir2/file (assumed defaultbucket and thus equivalent to first example)

The additional stream wrapper solution means that paths can be identical to those used with a local file-system, but applications wishing to utilize more than one bucket can still do so.

An additional implication of removing the bucket is that it allows for staging sites in different environments with the same set of files and corresponding data since a different bucket may be configured for the entire site instead of duplicated in each path and requiring changing. Obviously, those applications that choose to use more than one bucket will need to handle the cases themselves. This also aids in site migration as file paths stored in database do not need to be changed.

In lieu of an upstream GAE PHP runtime user-space setting the Drupal module will provide a typical Drupal setting and make use of it in the bucketless stream wrapper. In the future, it would make sense for the Drupal setting to merely set the upstream user-space setting.

### Implementation

The following is the class hierarchy used for implementing the bucketless and Drupal specific stream wrappers discussed in details below.

![GCS hierachy](/files/gcs-hierarchy.png)

In order to facilitate a clean implementation and the possibility for moving upstream, while working within the restriction that the current stream wrapper is a final class, a rough facsimile, that acts as a proxy, of the base implementation is provided in order to allow for extension. The bucketless stream wrapper is built on top of the facsimile. This provides two basic stream wrapper implementations without any Drupal specific additions. The facsimile is implemented as a PHP [Trait](http://php.net/trait) in order to allow for multiple inheritance as needed later for the Drupal wrappers. 

There are two levels of integration with Drupal that make sense to allow GCS to be used as comprehensively and easily as possible. The first is providing stream wrappers that implement the additional functionality defined by the DrupalStreamWrapperInterface and the second is overriding the default provided local file-system stream wrappers to use GCS. The first is required for the second, but also allows for the use of GCS in a specific manner vs catch-all local file-system.

The three core stream wrappers (private://, public://, temporary://) are overridden via [hook_stream_wrappers_alter()](http://api.drupal.org/hook_stream_wrappers_alter) to use the GCS wrappers. The storage bucket must be configured in order for the GCS integration to function properly. The standard mechanisms for controlling the file system setup (admin/config/media/file-system) can be used and file fields can be stored within one of the default stream wrappers.

File MIME types are determined by [DrupalLocalStreamWrapper::getMimeType()](http://api.drupal.org/DrupalLocalStreamWrapper::getMimeType) which consults [file_mimetype_mapping()](http://api.drupal.org/file_mimetype_mapping) for a mapping of extensions to MIME types. The type is included in the stream context when writing files to GCS and as such the file will be served with the assigned MIME type.

## Drupal core patch

In order to have Drupal run properly on Google App Engine a few changes need to be made to Drupal core. Those changes can be found in [root/core.patch](http://drupalcode.org/project/google_appengine.git/blob_plain/refs/heads/7.x-1.x:/root/core.patch) which is managed in the [7.x-appengine branch](https://github.com/boombatower/drupal-appengine) and rebased on top of Drupal core updates. The patch creates three other files within the appengine root directory that need to be placed in the Drupal root.

- Alters [drupal_http_request()](http://api.drupal.org/drupal_http_request) in common.inc to work without requiring socket support.
- Alters [drupal_move_uploaded_file()](http://api.drupal.org/drupal_move_uploaded_file) in includes/file.inc to support newly uploaded files from the $_FILES array being referenced via a stream wrapper. In the case of App Engine all uploaded files are uploaded through the GCS proxy, hosted on GCS, and thus start with gs://. The change should be generally useful and has been rolled as a [core patch](https://drupal.org/node/2114885).
- Alters [file_upload_max_size()](http://api.drupal.org/file_upload_max_size) in includes/file.inc to only check PHP ini setting 'upload_max_filesize' instead of also checking 'post_max_size' which is normally relevant, but in the case of App Engine is not since all uploads are sent through GCS proxy and are thus not affected by app instance post limits.
- Alters [drupal_tempnam()](http://api.drupal.org/drupal_tempnam) in includes/file.inc to manually simulate [tempnam()](http://php.net/tempnam) since it is currently not supported by App Engine.
- Alters [system_file_system_settings()](http://api.drupal.org/system_file_system_settings) in modules/system/system.admin.inc to include #wrapper_scheme property to be picked up by [system_check_directory()](http://api.drupal.org/system_check_directory) in modules/system/system.module. Given that the current code voids using the stream wrappers this is technically a bug and is a candidate for being fixed in Drupal core as well.
- Alters [system_requirements()](http://api.drupal.org/system_requirements) in modules/system/system.install to skip the directory check since the GCS integration will not be loaded until the App Engine module is enabled.

A number of the changes included in the patch are being looked at and will hopefully becoming unnecessary in the future. The following are also included in the patch as a convenience, but they do not alter Drupal core.

- Add [app.yaml](http://drupalcode.org/project/google_appengine.git/blob_plain/refs/heads/7.x-1.x:/root/app.yaml) to root which provides basic information about the app to Google App Engine so that it can invoke Drupal properly.
- Add [php.ini](http://drupalcode.org/project/google_appengine.git/blob_plain/refs/heads/7.x-1.x:/root/php.ini) to root which enables some php functions used by Drupal and turns on output buffering.
- Adds [wrapper.php](http://drupalcode.org/project/google_appengine.git/blob_plain/refs/heads/7.x-1.x:/root/wrapper.php) to root which simulates [Apache mod_rewrite](http://httpd.apache.org/docs/current/mod/mod_rewrite.html) like behavior.

# Aggregate CSS/JS

Since a local writable file-system is not available on Google App Engine for various reasons, the ability for Drupal to aggregate CSS and JS into combined files is restricted. There are three choices.

- Directly from static files (recommended, but requires proper setup)

    Serving from static files requires that the aggregate files be uploaded with the app. There are a couple of ways to achieve this some of which are better than others.

    - Build site locally using the development server and generate the files locally. During upload the aggregate files will be present and included with app.
    - Upload app and generate the files while running on App Engine and written to GCS. Download the files locally into the app and re-upload. This method means that your app may serve with out-of-date CSS or JS until you re-upload which can cause all sort of issues.

      [gsutil](https://developers.google.com/storage/docs/gsutil) makes it easy to download the css and js files from GCS.

      Run the following with the relevant values filled in.
```bash
./gsutil cp -R gs://{BUCKET}/sites/default/files/css {~/path/to/drupal}/sites/default/files/
./gsutil cp -R gs://{BUCKET}/sites/default/files/js {~/path/to/drupal}/sites/default/files/
```

- From GCS using Drupal router as proxy (default)

    By default, aggregate files are served via a Drupal router which acts as a GCS proxy. The proxy should always work without any additional configuration, but this will consume instance hours for serving static aggregate resources.

- Directly from GCS

    Serving directly from GCS does not require uploading static files with the app, but can cause difficulties since resources referenced from CSS will need to be uploaded to GCS as well (or referenced using an absolute URL). Also note that that the CSS and JS files will be served from a different domain which may cause complications.

# Closing

There are areas that could be improved and I plan to continue working so stay tuned. If you have any ideas or want to pitch in you may do so in the [Google App Engine issue queue](https://drupal.org/project/issues/google_appengine). As always I look forward to your feedback.
