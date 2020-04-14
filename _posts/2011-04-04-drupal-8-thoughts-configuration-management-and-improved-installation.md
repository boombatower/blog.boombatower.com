---
created: 1301891689
layout: post
redirect_from:
- article/80/
- node/80/
- drupal-8-thoughts-configuration-management-improved-installation/
title: 'Drupal 8 thoughts: configuration management and improved installation'
---
I have been doing a lot of work related to easing the process of building a site from scratch on an individual machine and thus dabbling with configuration management and related topics. Since <a href="http://buytaert.net/configuration-management-in-drupal-8">configuration management</a> is one of the Drupal 8 key initiatives I figured I would share some thoughts I had on the outer fringes of the topic with more to come in the future.

<h3>Installation</h3>
One area of Drupal that has always seemed a bit odd to me has been the installation process/system. The process can play a key part in configuration management across machines and made it impossibly to implement a basic environment system during installation without hacking core. To remedy this issue I believe the installation system can be much improved, simplified, and made much more consistent with the rest of Drupal which will in tern make it easy to implement my environment system in contrib or core.

The installation system in Drupal 7 was re-factored/rewritten quite a bit and has thus been quite improved, but I think the direction of the installation system could be changed to make it much better. Currently the installer attempts to fake systems in core, like the cache, and actually duplicates a lot of code found elsewhere for module management and what not. Why not simply package a minimal database dump, similar to how the update tests used a Drupal 6 dump written in DBTNG, that can be installed to create an extremely minimal Drupal installation. At that point the installer can act like any other module and provide forms to complete the process. All modules in addition to the required modules can be installed through the standard process invoked from the modules page, but done in an automated fashion through the installer.

The reasons why this approach is beneficial are: 1) install.inc and install.core.inc could be virtually removed, 2) profiles would no longer be "hackish" during the install phase (solve the current issues related to dependency resolution and what not being the same for modules and profiles), 3) hooks like <a href="http://api.drupal.org/hook_system_info_alter">hook_system_info_alter()</a> would work properly for profiles and modules during early installation phases, and 4) remove race conditions in general caused by maintain two sets of the same code.

<h3>Environments</h3>
In addition, my environment.module would work without hacking core. This concept is nothing new in the development world, but is something I feel would be a great candidate for Drupal 8.
```php
/**
 * Implements hook_system_info_alter().
 */
function environment_system_info_alter(&$info, $file, $type) {
  if (!empty($info['dependencies'])) {
    $environment = environment_get();
    if (!empty($info['dependencies'][$environment])) {
      $info['dependencies'] = array_merge($info['dependencies'], $info['dependencies'][$environment]);
    }
    foreach ($info['dependencies'] as $key => $dependency) {
      if (!is_numeric($key)) {
        unset($info['dependencies'][$key]);
      }
    }
  }
}

/**
 * Get the current environment.
 *
 * @return
 *   The current environment: production, staging, or development.
 */
function environment_get() {
  return variable_get('environment', 'production');
}
```

The above code allows for the environment to be configured in the settings.php file for a site.
```php
$conf['environment'] = 'development';
```

During installation a profile (or module) can perform different tasks or conditional code based on the environment. For example, generated users can have a simple password on a development machine and complex ones on production or staging machines.
```php
function my_profile_install() {
  if (environment_get() == 'development') {
    // Do cool stuff that only devs get to see.
  }
}
```

Another cool feature that I have a use-case for on a site I am working on and seems to generally be useful is to enable modules based on the environment. Instead of having to do that in hook_install() or related it makes sense to have a way to specify that in a .info file. The above code allows for the following.
```
name = My profile
description = ....
version = 0.1
core = 7.x

dependencies[] = block
dependencies[] = dblog

dependencies[development][] = views_ui
dependencies[development][] = fields_ui
dependencies[development][] = devel

dependencies[production][] = integration_with_third_party
dependencies[staging][] = integration_with_third_party
```

Having this type of functionality in core would hopefully encourage better development practices and seems like a great feature to have. I have a number of scripts in combination with drush make, and the above environment utility that allow me to build out a fully functional site on a new box with a single command. I plan to cleanup the scripts, document them, and provide them in a followup post. As always I would love to hear your thoughts on this subject.
