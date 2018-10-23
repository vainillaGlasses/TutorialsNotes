# Set up for Masters of the Universe React demo

## Create a content type: album

field_artist (text)

field_cover (image)

field_released (date)



Note** that the drupal console commands that are used in the video have to be made into ahoy commands in humpback.



## Create a custom module:

```bash
ahoy site console generate:module
```

Name:  “Masters of the Universe”

Machine name: masters_of_the_universe

Path: modules/custom

Description: React + Drupal!

Package name: Custom

Core: 8.x

Module file?  No

Module as feature? No

Add composer.json? No

Dependencies? No

Test unit class? No

Template? No

Proceed? Yes



**Enable the module**

On Video

```bash
ahoy site console module:install masters_of_the_universe
```

Module name: masters_of_the_universe



## Generate nodes

```bash
ahoy drush cr
ahoy site console create:nodes
```

type: album



## Install Module RESTful Web Services

Install Restful Web Service, it is on the Core, so just go to Extend and install it



## Generate an endpoint using a custom REST resource

```bash
ahoy site console generate:plugin:rest:resource
```

module name: masters_of_the_universe

Rest resource class name: AlbumRestResource

Rest resource id: albums

Rest resource label: Album rest resource

Rest resource url: api/albums

What REST states implemented by default: 0 (GET)

Confirm generation: yes



## Add Controller

```bash
ahoy site console generate:controller
```

Module: masters_of_the_universe

Controller class name: AlbumController

Controller method title: Albums

Action method name: app

Route path: /albums (masters_of_the_universe/albums)

Unit test class? no

Load services? no

Confirm? yes



**Now** look at the page (/albums).  Nothing there!  Because: no app.

ahoy drush cr



Now clear the cache

```bash
ahoy site console cr all
ahoy site console rest:enable
```



commands.rest.enable.arguments.methods: [0] GET

commands.rest.enable.arguments.formats: json

Available Authentication Providers: cookie



## Permissions

Go to People > Permissions







## After App Example (in ZIP)

```bash
npm install gulp-cli -g
npm install gulp -D
touch gulpfile.js
gulp --help


gulp build-dev
```