# To build locally

## 1. Clone the repo:

- git clone some-link some-dir
- cd some-dir

## 2. Download Lando

- Follow instructions:[ https://docs.devwithlando.io/installation/installing.html](https://docs.devwithlando.io/installation/installing.html)

## 3. Initialize Lando

- lando init
- Drupal 8 recipe
- Root is web
- Call it whatever you like
- lando start

## 4. Local setup

- composer install
- cd to web/sites
- Rename example.settings.local.php to settings.local.php
- Settings.local.php goes in sites/default
- Add DB settings and disable cache settings to your settings.local.php:

/**

\* Lando DB settings

*/

$databases = array();

if (getenv('LANDO') === 'ON') {

  $lando_info = json_decode(getenv('LANDO_INFO'), TRUE);

  $settings['trusted_host_patterns'] = ['.*'];

  $settings['hash_salt'] = md5(getenv('LANDO_HOST_IP'));

  $databases['default']['default'] = [

​    'driver' => 'mysql',

​    'database' => $lando_info['database']['creds']['database'],

​    'username' => $lando_info['database']['creds']['user'],

​    'password' => $lando_info['database']['creds']['password'],

​    'host' => $lando_info['database']['internal_connection']['host'],

​    'port' => $lando_info['database']['internal_connection']['port'],

  ];

}

/**

\* Disable Drupal cache for development

*/

$settings['container_yamls'][] = DRUPAL_ROOT . '/sites/default/services.local.yml';

$config['system.performance']['css']['preprocess'] = FALSE;

$config['system.performance']['js']['preprocess'] = FALSE;

$settings['cache']['bins']['render'] = 'cache.backend.null';

$settings['cache']['bins']['dynamic_page_cache'] = 'cache.backend.null';

$settings['cache']['bins']['page'] = 'cache.backend.null';

$GLOBALS['_kint_settings']['maxLevels'] = 3;

/**
 \* Add Version Control Sync directory
 */

$config_directories['vcs'] = '../config/vcs';



## 5.Add a `settings.local.yml` file 

In your `sites/default` directory with the following code:

```php
parameters:

  http.response.debug_cacheability_headers: true

  twig.config:

   debug: true

    auto_reload: true

    cache: false

services:

  cache.backend.null:

    class: Drupal\Core\Cache\NullBackendFactory
```



## 6. Uncomment the following in your settings.php:

```php
if (file_exists($app_root . '/' . $site_path . '/settings.local.php')) {

  include $app_root . '/' . $site_path . '/settings.local.php';

}
```



## 7. Drush site-install

```
lando drush si fellowship
```



## 8. Set UUID

1. ```
    lando drush config-set "system.site" uuid 23bdf0ad-fc39-42a8-8b04-45bd280f1819
    ```


## 9. Delete shortcut links 

That cause import issues (there is a [Drupal issue](https://www.drupal.org/node/2583113) open for this)

```
lando drush ev '\Drupal::entityManager()->getStorage("shortcut_set")->load("default")->delete();'
```

**OR**

If this doesn’t work, you can go to the config/vcs directory and edit this file: **shortcut.set.default.yml**

Remove the uuid line at the top and save.

## 10. Import config from vcs config directory

```bash
lando drush cim vcs
```

 

##11. cd to doggos theme and run:

**cd web/profiles/custom/fellowship/themes/custom/doggos**

- yarn install
- yarn dev

 

## 12. Clear cache

1. ```
    lando drush cr
    ```

##13 .Log in

```
lando drush uli
```

Copy and paste link from user to the end. 



Example: user/reset/1/1542831139/PPmzZ3yXZJrEaIw6gyM_XUi-Y3j4jaU5WTPbVF1H_jg/login

## 14. Import DB

Copy that file to the root of your project and then the command to import would be:

1. ```
    lando db-import dogme-db.sql.gz
    ```

##15. Files

Uncompress that folder (in macs you can just click on it, let me know if you have any issues) - and replace your files directory in sites/default with this



## Notes

When beginning work, always fetch the newest code, rebase, and import config before you start. Note that develop is the default branch.

- git fetch origin
- git rebase origin/develop
- lando drush cim vcs

When creating a new branch, use the JIRA abbreviation as the start of your branch with a short description of the task:

- Example: FSHP-3-color-taxonomy 

When creating a pull request, add notes in the JIRA task on how to test your story:

- Example:

- - Import config
    - Verify that the color taxonomy exists

To peer review a story, fetch new branches and switch to the branch that needs to be reviewed:

1. git fetch origin

2. git checkout --track origin/FSHP-3-color-taxonomy

3. The above command creates a local version of the branch that you can test

4. Does everything look the way the testing notes say it should?

5. Did you have any issues importing config?

6. Did anything break?

7. If there are changes needed, make comments in the PR and request the changes from the person who created the PR.

8. - To leave a comment, use the comments box in the ‘Conversation’ tab.

9. If there are no changes needed, approve the PR:

10. - Go to the ‘Files Changed’ tab in the PR in Github and click ‘Review Changes’.

To code review a story (Laura will be doing this for now):

Important: There is one person who is the tech lead for any project.  Only the tech lead can add code to the main branch (develop), or merge code. When you put a task into code review, say on the chat (slack) that the task is in code review. If you need it merged urgently, let the tech lead know. The Tech Lead may say that you can merge the branch. But normally they will need to do a code review first. 

The Tech Lead on this project is Laura.

1. Look at the code changes in the pull request

2. - Do the changes make sense?
    - Was there anything removed that shouldn’t be?
    - Is there anything added that shouldn’t be?
    - Is all of the code necessary, or is there a cleaner solution?
    - Are there validations (if statements) around calls to functions and variables to ensure that they exist and there is no case where their absence will break the site?

3. Check out the branch and go through the changed files with PHP Codesniffer + Drupal Coder to ensure that the code conforms to Drupal code standards

4. If there are changes needed, make comments in the PR and request the changes from the person who created the PR.

5. - To leave a general comment, use the comments box in the ‘Conversation’ tab.
    - To leave a comment about a specific part of the code, leave a code comment. When you mouse over the code in the ‘Review Changes’ tab, you’ll see a blue ‘+’ square appear. Click on it to leave a comment at that line in the code.

6. If there are no changes needed, merge the code and delete the branch.

7. Let the team know that the branch is merged and ask them to rebase their code.