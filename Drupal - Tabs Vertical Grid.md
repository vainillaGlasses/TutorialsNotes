# Drupal - Tabs Vertical Grid

## Install Skeletor

Prepare project with skeletor

- Generate a new project using humpback
- Delete custom install profile (under profiles folder)
- Delete default config (all files under config/sync)
- ahoy site local-settings
- Composer install
- Require skeletor: composer require myplanet/skeletor:8.2.x-dev
- Edit .ahoy/site.yml file and change install profile in install command to "skeletor" (around line 57)
- Edit settings.php (under settings folder) and change install_profile to skeletor (around line 40)
- Initialize git
- Start containers (ahoy up)
- Install site (ahoy site install)
- You should get a site with some common contrib modules installed and configured



## Install Skeletor Tabs Component

* 

