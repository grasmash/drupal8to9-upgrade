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

1. Run `composer require drupal/upgrade_status`. This will download the upgrade_status module to `docroot/modules/contrib/` via Composer.
1. Run `drush pm-enable upgrade_status -y`
1. If you're not already logged into your Drupal site, run `drush user-login` and cmd+click the link to login.
1. Visit `/admin/reports/upgrade-status` on the Drupal site. You can navigate there by first clicking "Reports" in the admin menu and then clikcing "Upgrade Status."
   ![image](https://user-images.githubusercontent.com/539205/115598426-6bb28880-a2a8-11eb-996e-f1e6ca758b79.png)

### Upgrade Contributed modules

1. Upgrade contributed modules.
   @todo

### Upgrade custom code for

@todo

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
