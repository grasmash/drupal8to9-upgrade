Acquia Drupal Minimal Project
====

This project template provides a minimal Drupal 9 application to be hosted on Acquia. It is based on the [Drupal Recommended Project](https://github.com/drupal/recommended-project/tree/9.0.x), with the principal difference being a relocated docroot for compatibility with Acquia hosting. It includes only essential dependencies and configuration to install and host Drupal 9 on Acquia.

This project targets experienced developers who prefer to build a new application from the ground up. If you prefer a more complete out-of-the-box Drupal 9 experience on Acquia Cloud, consider trying the [Acquia Drupal Recommended Project](https://github.com/acquia/drupal-recommended-project) instead.

## Installation and usage

Create a new project using Composer:
```
composer create-project --no-interaction acquia/drupal-minimal-project
```

Once you create the project, you can and should customize `composer.json` and the rest of the project to suit your needs. You will receive updates from any dependent packages, but not from the project template itself. It's yours to keep!

For instance, you can add a new package by running the following command and committing the changed `composer.json` and `composer.lock` to Git:
```
composer require acquia/blt
```

# License

Copyright (C) 2020 Acquia, Inc.

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License version 2 as published by the Free Software Foundation.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details.
