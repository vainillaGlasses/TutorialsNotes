# Using kint in twig and in preprocess

## Install Devel

```bash
composer require drupal/devel
```



## Enable Kint

In skeletor:

```bash
composer require drupal/devel

Composer install

Ahoy drush en kint
```

In settings.local.php, add: 

```php
$settings['twig_debug'] = TRUE;

$GLOBALS['_kint_settings']['maxLevels'] = 3;
```



## Preprocess Field

We are going to preprocess ``field_aic_link`` so that the link text **always** says Contact Us



In includes/field.inc.



Test Kint:  

```php
if ($field_name == 'field_aic_link') {

    ksm($variables['items']);

  }
```



Change the title: 

```php
if ($field_name == 'field_aic_link') {
    //ksm($variables['items']);
    foreach ($variables['items'] as &$item) {
        $item['content']['#title'] = 'Search the Google';
    }
}
```



Test Kint in Twig:

```twig
{#
/**
* @file
* Cards link field override.
*
*/
#}

{%
set classes = []
%}

<div{{ attributes.addClass(classes) }}>
    {% for item in items %}
        {{ kint(item) }}
        {{ item.content }}
    {% endfor %}
</div>
```



Drill down in Twig:

```twig
{#
/**
* @file
* Cards link field override.
*
\#}

{%
set classes = []
%}

<div{{ attributes.addClass(classes) }}>
    {% for item in items %}
        {{ item.content['#url'] }}
    {% endfor %}
</div>
```



## Preprocess Paragraph

We are going to preprocess ``component_accordion_item_cards``

So that if there is a link in the field, the text will use the URL and text that you entered for that paragraph.

If there is no link in the field, the title will say ‘Contact us’ and the URL will be ``<front>``



In includes/paragraph.inc, 

```php
function  [theme]_preprocess_paragraph(&$vars) {
ksm($vars);
}
```



**Remember ksm as “Kint set message”**



Open one of the bars at the top by clicking on the bar (not the plus sign!). We’re looking for the paragraph element. See that you can look at Available Methods. The method we want is getType, which will allow us to target a specific paragraph type.

Change your function to use ``ksm($vars[‘paragraph’]->getType());``

Create your preprocess paragraph function



In paragraph.inc

```php
function barebones_bootstrap_STARTERKIT_preprocess_paragraph(&$vars) {

  //ksm($vars);

  $paragraph = $vars['paragraph'];

  switch ($paragraph->getType()) {

    case 'component_accordion_item_cards':
    //ksm($paragraph->field_aic_link->getValue()[0]);
    if (!$paragraph->field_aic_link->isEmpty()) {
    $vars['link']['url'] = $paragraph->field_aic_link->getValue()[0]['uri'];
    $vars['link']['text'] = $paragraph->field_aic_link->getValue()[0]['title'];
	}
	else {
	$vars['link']['url'] = Url::fromRoute('<front>');
	$vars['link']['text'] = 'Contact us';
	}
    break;
  }
}
```

Change your twig template to use the variables you created in a link

In paragraph—component-accordion-item-cards.html.twig:

```twig
{{ link(link.text, link.url, {'class': ['btn', 'btn-primary']}) }} 
```



## Resources:

Drupal Twig Functions (for Link): 

<https://www.drupal.org/docs/8/theming/twig/functions-in-twig-templates>

Creating URLs in Drupal 8:

<https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Url.php/class/Url/8.2.x>

Debugging Twig using Kint and other methods:

<https://drupalize.me/blog/201405/lets-debug-twig-drupal-8>