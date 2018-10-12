# Drupal - Scroll reveal

1.  install scrollreveal using yarn in the theme.

   ```bash
   cd themes/custom/<theme name>
   yarn add scrollreveal
   ```

   This adds the library to package.json

2. Create component-reveal.js in scr/js

   ```js
   import ScrollReveal from 'scrollreveal';
   
   /**
    * @file
    * Global JS.
    */
   (function componentReveal($, Drupal) {
     Drupal.behaviors.componentReveal = {
       attach: (context) => {
         if ($('.component--reveal', context).length) {
           ScrollReveal().reveal('.component--reveal');
         }
       }
     };
   }(jQuery, Drupal));
   ```


From [https://scrollrevealjs.org/guide/hello-world.html](https://scrollrevealjs.org/guide/hello-world.html)



3. in barebones_bootstrap_STARTERKIT_libraries.yml, declare library

   ```yml
   bootstrap:
     version: 1.x
     css:
       base:
         dist/app.css: {}
     js:
       dist/app.js: {}
     dependencies:
       - core/jquery.once
       - core/drupal
   componentReveal:
     js:
       dist/component-reveal.js: {}
     dependencies:
       - core/jquery
       - core/drupal
   ```


4. In templates for components, attach the library and add the class component—reveal

   In paragraph—component-accordion.html.twig

   ```twig
   {{ attach_library('barebones_bootstrap_STARTERKIT/componentReveal') }}
   ```

   Add component—reveal to the list of classes

   ```twig
   {%
     set classes = [
       'paragraph',
       'paragraph--type--' ~ paragraph.bundle|clean_class,
       view_mode ? 'paragraph--view-mode--' ~ view_mode|clean_class,
       not paragraph.isPublished() ? 'paragraph--unpublished',
       'component',
       'component--accordion',
       'full-width',
       'component--reveal'
     ]
   %}
   {{ attach_library('barebones_bootstrap_STARTERKIT/componentReveal') }}
   {% block paragraph %}
     <section{{ attributes.addClass(classes) }}>
       {% block content %}
         {{ content }}
       {% endblock %}
     </section>
   {% endblock paragraph %}
   ```


Modify webpack.config.js to include the new JS file

```js
// will create dist/app.js and dist/app.css
.addEntry('app', './src/js/app.js')

// will create dist/component-reveal.js
.addEntry('component-reveal', './src/js/component-reveal.js')
```



Then in the theme

```bash
yarn watch
yarn build
```



Finally in the project

```bash
ahoy drush cr
```

