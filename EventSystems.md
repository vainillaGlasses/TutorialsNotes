

# Event Systems Overview & How To Subscribe To and Dispatch Events

From: https://www.drupal.org/docs/8/creating-custom-modules/event-systems-overview-how-to-subscribe-to-and-dispatch-events



[TOC]

## Event Systems Overview

Allow extensions to modify how the system works

* **Event Subscribers** (Listeners):

  * Callable methods or functions that react to an event
* **Event Registry**

  * Where event subscribers are collected and sorted
* **Event Dispatcher** (Trigger)
* **Event Context** 

  * Set of data that is important to the subscribers to an event. This can be a value passed to the Event Subscriber, or a specially created class with relevant data.


## Drupal Hooks



- **Event Subscribers** 

  - Drupal hooks are registered in the system by defining a function with a specific name. For example:
    - "*hook_my_event_name*" event --> `<moduleName>_my_event_name()`

- **Event Registry** 

  - Drupal hooks are stored in the "*cache_boostrap*" bin under the id "*module_implements*"; keyed by the name of the hook itself.

- **Event Dispatcher**

  - Drupal 7-: hooks are dispatched through use of the `module_invoke_all() `function

  - Drupal 8: hooks are dispatched through `\Drupal::moduleHandler()->invokeAll()` service method.

- **Event Context**

  - Context is passed into hooks by way of parameters to the subscriber

  - Drupal 7-: `module_invoke_all('my_event_name', $some_arbitrary_parameter);`
  - Drupal 8: `\Drupal::moduleHandler()->invokeAll('my_event_name', [$some_arbitrary_parameter]);`



Some drawbacks to the "*hooks*" approach to events are:

- **Only registers events during cache rebuilds.**
  Drupal only looks for new hooks when certain caches are built.You have to rebuild various caches depending on the hook you're implementing.
- **Can only react to each event once per module.**
  Since these events are implemented by defining very specific function names, there can only ever be one implementation of an event per module or theme. This is an arbitrary limitation when compared to other event systems.
- **Can not easily determine the order of events.**
  Drupal determines the order of event subscribers by the order modules are weighted within the greater system. This "weight" determines the order modules are loaded, and therefore the order events are dispatched to their subscribers.

With the foundation of Symfony in Drupal 8, while there are not a lot of events dispatched in Drupal 8 core, plenty of modules have started making use of this system.



## Drupal 8 Events

How this breaks down into our list of event system components?

- **Event Subscribers** - A class that implements the `\Symfony\Component\EventDispatcher\EventSubscriberInterface`.
- **Event Dispatcher** - A class that implements `\Symfony\Component\EventDispatcher\EventDispatcherInterface`. Generally at least one instance of the Event Dispatcher is provided as a service to the system, but other dispatchers can be created if desired.
- **Event Registry** - It's stored within the Event Dispatcher object as an array keyed by the event name and the event priority. When registering an event as a service, that event is registered within the globally available dispatcher.
- **Event Context** - Extends the `\Symfony\Component\EventDispatcher\Event` class. Generally each extension that dispatches its own event will create a new type of Event class that contains the relevant data event subscribers need.

## My First Drupal 8 Event Subscriber

An event subscriber that shows the user a message when a Config object is saved or deleted.

### Module

```php
name: Custom Events
type: module
description: Custom/Example event work.
core: 8.x
package: Custom
```

 **Humback**: ahoy site console generate:module



### Register a new event subscriber

1. Create custom_events.services.yml

   1. We define a new service named "*my_config_events_subscriber*"
   2. We set its "*class*" property to the the global name of a new PHP class that we will create.
   3. We define the "*tags*" property, and provide a tag named "event_subscriber". This is how the service is registered as an event subscriber within the syste

   ```php
   services:
     # Name of this service.
     my_config_events_subscriber:
       # Event subscriber class that will listen for the events.
       class: '\Drupal\custom_events\EventSubscriber\ConfigEventsSubscriber'
       # Tagged as an event_subscriber to register this subscriber with the event_dispatch service.
       tags:
         - { name: 'event_subscriber' }
   ```

2. The event subscriber class

   **Module Folder Tree**:

   > modules
   >
   > |--- custom
   >
   > ​	|--- custom_events
   >
   > ​		|--- src
   >
   > ​			|---EventSubscriber
   >
   > ​				|---ConfigEventsSubscriber.php
   >
   > ​		|---custom_events.info.yml
   >
   > ​		|---custom_events.module
   >
   > ​		|---custom_events.services.yml

   **Requirements**:

   - Extend the `EventSubscriber` class
   - Have a `getSubscribedEvents()` method that returns an array; where the keys of the array will be the event names you want to subscribe to, and the values of those keys are a method name on this event subscriber object

```php
<?php

namespace Drupal\custom_events\EventSubscriber;

use Drupal\Core\Config\ConfigCrudEvent;
use Drupal\Core\Config\ConfigEvents;
use Symfony\Component\EventDispatcher\EventSubscriberInterface;

/**
 * Class EntityTypeSubscriber.
 *
 * @package Drupal\custom_events\EventSubscriber
 */
class ConfigEventsSubscriber implements EventSubscriberInterface {

  /**
   * {@inheritdoc}
   *
   * @return array
   *   The event names to listen for, and the methods that should be executed.
   */
  public static function getSubscribedEvents() {
    return [
      ConfigEvents::SAVE => 'configSave',
      ConfigEvents::DELETE => 'configDelete',
    ];
  }

  /**
   * React to a config object being saved.
   *
   * @param \Drupal\Core\Config\ConfigCrudEvent $event
   *   Config crud event.
   */
  public function configSave(ConfigCrudEvent $event) {
    $config = $event->getConfig();
    drupal_set_message('Saved config: ' . $config->getName());
  }

  /**
   * React to a config object being saved.
   *
   * @param \Drupal\Core\Config\ConfigCrudEvent $event
   *   Config crud event.
   */
  public function configDelete(ConfigCrudEvent $event) {
    $config = $event->getConfig();
    drupal_set_message('Deleted config: ' . $config->getName());
  }

}
```



#### **Important notes:**

- Extend the `EventSubscriber` class.
- Implement the `getSubscribedEvents()` method; returns an array of event name => method name key/value pairs.
- In `configSave()` and `configDelete()` we expect an object of the `ConfigCrudEvent` type; has a the method `getConfig()` which returns the `Config` object for this event

#### **What is ConfigEvents::SAVE?**

You create a globally available constant whose value is the name of the event

####  **Where ConfigEvents::SAVE come from?**

``\Drupal\Core\Config\ConfigEvent`` has a constant `SAVE`and its value is ``'config.save'``

#### **Why did we expect a ConfigCrudEvent object, and how did we know that?**

- You create a new type of object that is special to your event; contains the data needed, and has a simple api for that data
- We're best able to determine the event object expected by exploring the code base and the public api documentation

We should see a message that contains the config object's name

## Steps

1. Install "*custom_events*" module on its own.

2. Install "*statistics*" module.

3. Uninstall "*statistics*" module.

   This time we see both the `SAVE` and `DELETE` events fired. We can see that the `statistics.settings` config object has been deleted, and the `core.extension`config object was saved.



