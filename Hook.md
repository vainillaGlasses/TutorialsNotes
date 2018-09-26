# Hook

Create a new module

```ahoy site console generate:module```

## Theme Hook

Create function 

```php
function hook_theme(){}
```

### Parameters

**array $existing**: An array for override purposes. This is primarily useful for themes that may wish to examine existing implementations to extract data (such as arguments) so that it may properly register its own, higher priority implementations.

**$type**: Whether a theme, module, etc. is being processed.May be one of:

- **'module'**: A module is being checked for theme implementations.
- **'base_theme_engine'**: A theme engine is being checked for a theme that is a parent of the actual theme being used.
- **'theme_engine'**: A theme engine is being checked for the actual theme being used.
- **'base_theme'**: A base theme is being checked for theme implementations.
- **'theme'**: The actual theme in use is being checked.

**$theme**: The actual name of theme, module, etc. that is being being processed.

**$path**: The directory path of the theme or module, so that it doesn't need to be looked up.