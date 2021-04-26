# Upgrade Drupal 8 > 9 in 20 Minutes

This repository contains an example Drupal 8 application to be used for the "Upgrade Drupal 8 > 9 in 20 Minutes" workshop.

To upgrade this site to Drupal 9, proceed to the following directions.

## Prepare for the Workshop

The "hands on" portion of the workshop will be performed in Acquia's recommended development environment, [Acquia Cloud IDE](https://www.acquia.com/products/drupal-cloud/cloud-ide). If you're already an Acquia customer you may use your existing [Cloud Platform application](https://cloud.acquia.com/a). In any case, you will be invited to join our "Acquia Learning Services" sandbox application where you are free to create a Cloud IDE for this workshop.

1. When you receive an invite to the Acquia Learning Services application from Acquia via email, accept the invite. You will be prompted to [login or create an Acquia Cloud Platform account](https://accounts.acquia.com/sign-in), and you will then be prompted to accept the invitation for that account.
1. On the ["Acquia Learning Services"](https://cloud.acquia.com/a/applications/11566968-0964-4659-9a21-6ef99adae3df) application on cloud.acquia.com, create a new Cloud IDE for yourself by clicking **Actions** > **Create Cloud IDE** and give it a **Label** such as **Drupal 9 Upgrade Workshop**. Please only create **one** IDE, as the number available are limited. This IDE will be available for 3 days following the workshop.
   ![image](https://user-images.githubusercontent.com/539205/115599148-407c6900-a2a9-11eb-97f4-8de9404fa4c8.png)
1. The IDE will take 10-15 minutes to provision. You will see it listed alongside your other environments such as Dev, Stage, and Prod. In the mean time, feel free to peruse the [Cloud IDE Product Documentation](https://docs.acquia.com/ide/) to familiarize yourself with the user interface.
1. Once provisioning is complete, click **Launch Cloud IDE** to login to the Cloud IDE. <img width="463" alt="Screen Shot 2021-04-22 at 12 28 59 PM" src="https://user-images.githubusercontent.com/332535/115774531-8fd49f00-a366-11eb-8319-5fe68d6ef6f0.png">
3. If you'd like, you can complete steps 1-3 in the "Workshop Walkthrough" below so that you join this workshop with Drupal already installed on your IDE.

## Workshop Walkthrough

### Set up your Cloud IDE with a Drupal 8 site

1. Login to your Cloud IDE.
1. Open a Terminal pane in the IDE browser tab by clicking **Terminal** > **New Terminal**. You will start off in the **~/project** directory, which is where your Drupal application will live.
   ![image](https://user-images.githubusercontent.com/539205/115598056-fd6dc600-a2a7-11eb-9b07-f1e365898981.png)

1. Clone the codebase. In the Terminal pane of the Cloud IDE, Run:
   ```bash
   git clone https://github.com/grasmash/drupal8to9-upgrade .
   ```
1. Install all the necessary modules and libraries using Composer. In the terminal run:
   ```bash
   composer install
   ```
1. Now, install the Drupal site automatically using Drush. In the terminal, run:
   ```bash
   drush site-install -y
   ```
1. Your codebase (files) is all ready to go! If you do not, then click the "Refresh" button on the explorer pane of your IDE.
   ![image](https://user-images.githubusercontent.com/673633/115936908-26cf5300-a45c-11eb-9c79-33710b6530fb.png)

1. Now, you may login to the new Drupal site and validate that it is working as expected. In the terminal, run:
   ```bash
    drush user-login
   ```
Hold the cmd key and click the link. This will open the your Drupal site in a new tab and log you in automatically. The site is called "Drush Site-Install" and has no content yet. You are now all set and ready to migrate your test site!

   ![image](https://user-images.githubusercontent.com/332535/115776003-76345700-a368-11eb-9236-6fb8059992d6.png)


### Add the "Upgrade Status" tool to your Drupal 8 site

Next, we will download the [Upgrade Status module](https://www.drupal.org/project/upgrade_status). This will assist us with identifying what changes will be necessary to make our Drupal 8 site Drupal 9 compatible.

1. First, we want to add Drupal core developer libraries and enable you to run tests. This is only necessary for migrating and doing development, so we will remove this later. In the terminal, run:
   ```bash
   composer require drupal/core-dev:8.9.0 --dev -W
   ```

1. Next, we will want to add the "Upgrade Status" module which will run tests and direct us on how to upgrade our Drupal 8 site and prepare it for Drupal 9! In the terminal, run:
   ```bash
   composer require drupal/upgrade_status
   ```
1. Now that we have the "Upgrade Status" module added to the site, lets turn it on. In the terminal, run:
   ```bash
   drush pm-enable upgrade_status -y
   ```
1. You should already be logged into your site from the previous section, but if you are not, run `drush user-login` and cmd+click the link to login.
1. Visit `/admin/reports/upgrade-status` on the Drupal site. You can navigate there by first clicking "Reports" in the admin menu and then clikcing "Upgrade Status."
   ![image](https://user-images.githubusercontent.com/539205/115598426-6bb28880-a2a8-11eb-996e-f1e6ca758b79.png)

### Upgrade Contributed modules

1. Note on the "Upgrade Status" report that the Token module is listed as incompatible with Drupal 9. However, that notice also indicates under the "Plan" column that "8.x-1.7+ is Drupal 9 compatible.". We will therefore update the Token module to _at least_ 8.x-1.7.
   ![image](https://user-images.githubusercontent.com/539205/115734335-9e678a00-a357-11eb-8997-5163313a8f2b.png)
1. Remember that in Drupal's Semantic versioning for Composer, the canonical version 8.x-1.7 is transformed to `1.7.0`. We will therefore use a version constraint of `"drupal/token": "^1.7.0"` to instruct Composer to install a *minimum* version of Token 1.7.0. 
1. We can do this by using the same command above to add a module, but this time we will specify the version. **Switch** back to your IDE. In the terminal, run:
   ```bash
   composer require drupal/token:^1.7.0
   ```
1. Your codebase has now been updated! You will want to clear the cache in order to let the system see the updated module version. In the terminal, run:
   ```bash
   drush cache-rebuild
   ```
2. **Switch** back to your Drupal site, and reload the Upgrade Status report. You should see that Token has now been moved under a "Drupal 9 Compatible" heading and the row is green.  ![image](https://user-images.githubusercontent.com/332535/115777051-b34d1900-a369-11eb-9870-c8145fdb03ff.png)

Your contributed modules are now Drupal 9 compatible!

### Upgrade custom code

1. The second module is an example custom module called `my_special_module`. This module is NOT Drupal 9 compatible. Lets enable the module so we can scan it. **Switch** back to your IDE. In the terminal, run:
   ```bash
    drush pm-enable my_special_module -y
   ```
1. Now we can check the status of the custom module. **Switch** back to your drupal site and refresh the Upgrade Status report. Check the row for **My special module**, and click **Scan selected**.
   ![image](https://user-images.githubusercontent.com/539205/115738525-20a57d80-a35b-11eb-9c6d-47dc1bd1f7ca.png)

1. The Upgrade Status tool has identified a few issues that we need to clear up. Click on the "2 problems" link for the "My special module" module:
   ![image](https://user-images.githubusercontent.com/539205/115738683-4468c380-a35b-11eb-898c-ba040d91f753.png)

1. You'll see some very helpful details on exactlly what needs to be changed in the module In this case, we need to replace a deprecated function and update the module's info.yml file to let Drupal know that it can be run in Drupal 8 OR 9:
   ![image](https://user-images.githubusercontent.com/539205/115739800-38c9cc80-a35c-11eb-8505-b74258a5ced4.png)

1. Lets make those changes! **Switch** back to your IDE. In the explorer pane, open the `docroot` folder, then the `modules` folder, and then the `custom` folder. You should now see the `my_special_module` folder. Open this one too.
1. Lets fix the easy one first. Open the `my_special_module.info.yml` file and lets tell Drupal that this module works for both Drupal 8 and 9. In the file, change:
   ```yml
   core: 8.x
   ```
   To:
   ```yml
   core_version_requirement: ^8 || ^9
   ```
Now you can save the file by going to `File -> Save` in your IDE menu.
1. Changing the other issue is a little trickier, but not bad. The Upgrade Status tool told us exactly what needed to be changed, and where! The `drupal_set_message` function has been deprecated, and we are using it on like 9. Lets go ahead and make that change. Open the `my_special_module.module` file and change:
   ```php
   drupal_set_message(t("Welcome @name! You are a special person.", ['@name' => $account->getAccountName()]));
   ```
   To:
   ```php
   \Drupal::messenger()->addMessage(t("Welcome @name! You are a special person.", ['@name' => $account->getAccountName()]));
   ```
Notice that we are just changing the function a little, but the inside of the function remains the same. This is exactly what the Upgrade Status tool told us to do. Now you can save the file by going to `File -> Save` in your IDE menu.
1. Here comes the fun part! **Switch** back to your Drupal site, and re-scan the custom module once more in the Upgrade Status report. This time, it also moves down to the "Drupal 9 Compatible" fieldset! Congratulations - you just migrated your custom module from Drupal 8 to Drupal 9!

### Remove our tools

Now that our entire site is Drupal 9 compatible, we can remove some of the tools that we installed. **Switch** to your IDE again.

1. Uninstall the "Upgrade Status" module. In the terminal, run:
   ```bash
   drush pm-uninstall upgrade_status
   ```
1. Now we can remove the "Upgrade Status" module completely. In the terminal, run:
   ```bash
   composer remove drupal/upgrade_status
   ```
1. We can also remove the core developer testing tools. In the terminal, run:
   ```bash
   composer remove drupal/core-dev --dev
   ```

After doing this, you'll see that the Upgrade Status report and module are both gone, and your site is back to the way it was.

### Upgrade Drupal core.

Now that all of our modules are Drupal 9 compatible, the last step is to upgrade Drupal core.

1. We could do this using the composer command again, but it is actually pretty easy to manually make the change. **Switch** to your IDE. In the root of your explorer pane, open the `composer.json` file.
   ![image](https://user-images.githubusercontent.com/539205/115598302-41f96180-a2a8-11eb-8896-bba917745afa.png)

1. Inside that file, lets change the three references to Drupal 8.9 to Drupal 9.1 (or higher). Change:
   ```json
   "drupal/core-composer-scaffold": "^8.9.0",
   "drupal/core-recommended": "^8.9.0",
   To:
   ```json
   "drupal/core-composer-scaffold": "^9.1.0",
   "drupal/core-recommended": "^9.1.0",
Now you can save the file by going to `File -> Save` in your IDE menu.
1. We are getting closer! The `composer.json` file will tell composer to replace Drupal 8 core with Drupal 9, so lets have it update. In the terminal, run:
   ```bash
   composer update
   ```
1. Next, we need to clear the caches to be sure that everything sees the updated code. In the terminal, run:
   ```bash
   drush cache-rebuild
   ```
1. Finally, we need to update the database so that it can make any necessary changes. In the terminal, run:
   ```
   drush updatedb -y
   ```
1. **You're done!** To verify, **Switch** to your Drupal site, and go to **Reports** > **Status report** and verify that your **Drupal version** reads 9.1.7 instead of 8.9.14!

![image](https://user-images.githubusercontent.com/539205/116137281-5622d180-a6a1-11eb-9b0b-c92a7aa8a938.png)

### Troubleshooting
#### I get an error "Your requirements could not be resolved to an installable set of packages."
This is caused by a typo in your composer.json, for example specifying `9.1.0` instead of `^9.1.0`. Check the results **carefully**... every character matters! :)

#### I see notices when running `composer update` about Package container-interop/container-interop being abandoned
Don't sweat it... this is coming from Drupal core because of one of its upstream dependencies; nothing you're doing wrong, and it's a harmless error.
