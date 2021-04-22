Upgrade Drupal 8 > 9 in 20 Minutes
====

This repository contains an example Drupal 8 application to be used for the "Upgrade Drupal 8 > 9 in 20 Minutes" workshop.

To upgrade this site to Drupal 9, proceed to the following directions.

Prepare for the Workshop
===

1. When you receive an invite to an Acquia Cloud Platform application via email, accept the invite. This will create an Acquia Cloud Platform account for you and add you to the correct application.
1. On the Acquia Cloud Platform site on cloud.acquia.com, create a new Cloud IDE for yourself.
   ![image](https://user-images.githubusercontent.com/539205/115599148-407c6900-a2a9-11eb-97f4-8de9404fa4c8.png)
1. Login to the Cloud IDE and familiarize yourself with the user interface.
1. If you'd like, you can complete steps 1-3 in the "Workshop Walkthrough" below so that you join thi workshop with
   Drupal already installed.

Workshop Walkthrough
===

### Set up your Cloud IDE with a Drupal 8 site

1. Login to your Cloud IDE.
1. Open CLI pane in the IDE browser tab.
   ![image](https://user-images.githubusercontent.com/539205/115598056-fd6dc600-a2a7-11eb-9b07-f1e365898981.png)
1. In the CLI pane of the Cloud IDE tab, Run:
   ```
   git clone https://github.com/grasmash/drupal8to9-upgrade .
   composer install
   drush site-install -y
   ```
1. If you'd like, you may login to the new Drupal site and validate that it is working as expected. Just `drush user-login` and cmd+click the link.

### Add the "Upgrade Status" tool to your Drupal 8 site

Next, we will download the Upgrade Status module. This will assist us with identifying what changes will be necessary to make our Drupal 8 site Drupal 9 compatible.

1. Run:
   ```
   composer require drupal/upgrade_status
   composer require drupal/core-dev --dev -W
   drush pm-enable upgrade_status -y
   ```
1. If you're not already logged into your Drupal site, run `drush user-login` and cmd+click the link to login.
1. Visit `/admin/reports/upgrade-status` on the Drupal site. You can navigate there by first clicking "Reports" in the admin menu and then clikcing "Upgrade Status."
   ![image](https://user-images.githubusercontent.com/539205/115598426-6bb28880-a2a8-11eb-996e-f1e6ca758b79.png)

### Upgrade Contributed modules

1. Note on the "Upgrade Status" report that the Token module is listed as incompatible with Drupal 9. However, that notice also indicates under the "Plan" column that "8.x-1.7+ is Drupal 9 compatible.". We will therefore update the Token module to _at least_ 8.x-1.7.
  ![image](https://user-images.githubusercontent.com/539205/115734335-9e678a00-a357-11eb-8997-5163313a8f2b.png)
1. In Drupal's Semantic versioning for Composer, the canonical version 8.x-1.7 is transformed to `1.7.0`. We will therefore use a version constraint of `"drupal/token": "^1.7.0"` to instruct Composer to install a minimum version of Token 1.7.0. Upgrade the token module by opening `composer.json` in the IDE "files" pane and changing the following files content:
   ```
   "drupal/token": "1.5.0",
   ```
   To:
   ```
   "drupal/token": "^1.7.0",
   ```
1. Run `composer update` to update your dependencies (and `composer.lock`) to match your newly defined requirements.

Your contributed modules are now Drupal 9 compatible!

### Upgrade custom code for

1. Let's enable an example custom module called `my_special_module`. This modules in NOT Drupal 9 compatible. Run:
   ```
    drush pm-enable my_special_module -y
   ```
1. Visit `/admin/reports/upgrade-status` on the Drupal site again. Re-run the scan.
   ![image](https://user-images.githubusercontent.com/539205/115738525-20a57d80-a35b-11eb-9c6d-47dc1bd1f7ca.png)
1. Click on the "2 problems" link for the "My special module" module:
   ![image](https://user-images.githubusercontent.com/539205/115738683-4468c380-a35b-11eb-898c-ba040d91f753.png)
1. You'll see details on exactlly what needs to be changed in the module:
   ![image](https://user-images.githubusercontent.com/539205/115739800-38c9cc80-a35c-11eb-8505-b74258a5ced4.png)


### Upgrade Drupal core.

1. Open composer.json in the IDE.
   ![image](https://user-images.githubusercontent.com/539205/115598302-41f96180-a2a8-11eb-8896-bba917745afa.png)
1. Change:
   ```
   "drupal/core-composer-scaffold": "^8.9.0",
   "drupal/core-recommended": "^8.9.0",
   ```
   To:
   ```
   "drupal/core-composer-scaffold": "^9.2.0",
   "drupal/core-recommended": "^9.2.0",
   ```
1. Run:
   ```
   composer update
   drush cache-rebuild
   drush updatedb
   ```
1. You're done!
