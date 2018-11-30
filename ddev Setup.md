# ddev Setup



From https://ddev.readthedocs.io/en/stable/



## Install ddv

```bash
brew tap drud/ddev && brew install ddev
```



##Database Import

```bash
ddev import-db --src=dumpfile.sql.gz
```

```php
In service.local.php:

$databases = array();

 $databases['default']['default'] = [
   'driver' => 'mysql',
   'database' => $ddev_db,
   'username' => $ddev_username,
   'password' => $ddev_pw,
   'host' => $ddev_host,
   'port' => $ddev_port,
 ];
```





In settings.php, uncomment:



```php
if (file_exist...)


```



## Drush Integration

In credential, download the folder, then in root run:

```bash
$ tar -C $HOME -xf $HOME/Downloads/acquia-cloud.drush-8-aliases.tar.gz
```



## Project Details

You can also see more detailed information about a project by running `ddev describe` from its working directory. You can also run `ddev describe [project-name]` from any location to see the detailed information for a running project.



   

## Drush

**Drush sa** allow to see all  alias of acquia









/**

 \#ddev-generated: Automatically generated Drupal settings file.

 ddev manages this file and may delete or overwrite the file unless this comment is removed.

 */