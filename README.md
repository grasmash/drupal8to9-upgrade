Upgrade Drupal 8 > 9 in 20 Minutes
====

This repository contains an example Drupal 8 application to be used for the "Upgrade Drupal 8 > 9 in 20 Minutes" workshop.

To upgrade this site to Drupal 9, proceed to the following directions.

Workshop Walkthrough
===

1. Open CLI in IDE browser tab.
1. ```
   git clone https://github.com/grasmash/drupal8to9-upgrade .
   composer install
   drush site-install -y
   ```
1. Run `composer require drupal/upgrade_status`
1. Run `drush pm-enable upgrade_status -y`
1. Run `drush user-login` and cmd+click the link.
1. Visit `/admin/reports/upgrade-status` on the Drupal site. Reports => Upgrade Status.
1. Open composer.json in the IDE.
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
