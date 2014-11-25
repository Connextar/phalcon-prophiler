#Prophiler - A Phalcon Profiler & Dev Toolbar

[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/fabfuel/prophiler/badges/quality-score.png?b=develop)](https://scrutinizer-ci.com/g/fabfuel/prophiler/?branch=develop)
[![Code Coverage](https://scrutinizer-ci.com/g/fabfuel/prophiler/badges/coverage.png?b=develop)](https://scrutinizer-ci.com/g/fabfuel/prophiler/?branch=develop)
[![Build Status](https://scrutinizer-ci.com/g/fabfuel/prophiler/badges/build.png?b=develop)](https://scrutinizer-ci.com/g/fabfuel/prophiler/build-status/develop)


## Installation
You can use composer to install the Prophiler. Just add it as dependency:

    "require": {
       	"fabfuel/prophiler": "~0.1",
    }

## Setup
Setting up the Prophiler and the developer toolbar can be done by these simple steps. It could all be done in your front controller (e.g. public/index.php) 

#####1. Initialize the Profiler (as soon as possible)
Generally it makes sense to initialize the profiler as soon as possible, to measure as much execution time as you can. You should initialize the profiler in your frontcontroller or a separat bootstrap file right after requiring the Composer autoloader.

```php
$profiler = new \Fabfuel\Prophiler\Profiler();
```

#####2. Add the profiler to the dependency injection container
Add the profiler instance to the DI container, that other plugins and adapters can use it accros the application. This should be done in or after your general DI setup.
	
```php
$di->setShared('profiler', $profiler);
```

#####3. Initialize the plugin manager
The plugin manager registers all included framework plugins automatically and attaches them to the events manager.  

```php
$pluginManager = new \Fabfuel\Prophiler\Plugin\Manager\Phalcon($profiler);
$pluginManager->register();
```

#####4. Initialize and register the Toolbar

To visualize the profiling results, you have to initialize and render the Prophiler Toolbar. This component will take care for rendering all results of the Profiler benchmarks and other data collectors. Put that at the end of the frontcontroller.

```php
$toolbar = new \Fabfuel\Prophiler\Toolbar($profiler);
echo $toolbar->render();
```

To render the toolbar as very last action, you can also register it as shutdown function:

```php
register_shutdown_function([$toolbar, 'render']);
```

## Tips

To record session writing, you can commit or write & close the session before rendering the toolbar
    
```php
session_commit();

session_write_close() // same behavior
```