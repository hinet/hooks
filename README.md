![Hooks](https://raw.githubusercontent.com/larapack/hooks/master/resources/logo.png)

<p align="center">
<a href="https://travis-ci.org/larapack/hooks"><img src="https://travis-ci.org/larapack/hooks.svg?branch=master" alt="Build Status"></a>
<a href="https://styleci.io/repos/76883435/shield?style=flat"><img src="https://styleci.io/repos/76883435/shield?style=flat" alt="Build Status"></a>
<a href="https://packagist.org/packages/larapack/hooks"><img src="https://poser.pugx.org/larapack/hooks/downloads.svg?format=flat" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/larapack/hooks"><img src="https://poser.pugx.org/larapack/hooks/v/stable.svg?format=flat" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/larapack/hooks"><img src="https://poser.pugx.org/larapack/hooks/license.svg?format=flat" alt="License"></a>
</p>

Made with ❤️ by [Mark Topper](https://marktopper.com)

# Hooks

Hooks is a extension system for your [Laravel](https://laravel.com) application.

# Installation

Install using composer:

```
composer require larapack/hooks
```

Then add the service provider to the configuration:
```php
'providers' => [
    Larapack\Hooks\HooksServiceProvider::class,
],
```

# Packages

Packages can be found on [larapack.io](https://larapack.io).

# Integrations

- [Voyager Hooks](https://github.com/larapack/voyager-hooks) - Hooks supported directly in the Voyager admin panel.


# Getting Started

A hook is basicly a Laravel service provider that is loaded automatically when that hook is enabled.  
A hook can have 3 states:
* **Not-installed** - This hook is not installed, but exists in the application for some reason, this is mostly only the case if you are building your own hook. In this case you have the option to install or delete the hook.
* **Disabled** - This hook is installed, means that it have been installed but its service providers are not loaded yet. In this case you have the option to uninstall or enable the hook.
* **Enabled** - This hook is installed and enabled, means that it's service providers will now be loaded. In this case you have the option to disable or uninstall the hook.

When working with hook there are 4 actions:
* **Install** - This will download the hook if not already downloaded and then run the installation of that hook.
* **Enable** - After installing a hook, you will have to enable it in order to load it service providers.
* **Disable** - If you ever need to disable the functionality that the hook came along with, you can always disable it to keep its configuration.
* **Uninstall** - This will disable a hook if not already disabled and then remove all the configurations and undo the whole installation of a hook along with deleting it from the system.

The hook system comes with multiple Artisan commands:
* *hook:check* - Check for updates and show hooks that can be updated
* *hook:disable* - Disable a hook
* *hook:enable* - Enable a hook
* *hook:info* - Get information on a hook
* *hook:install* - Download and install a hook from remote https://larapack.io
* *hook:list* - List installed hooks
* *hook:make* - Make a hook
* *hook:setup* - Prepare Composer for using Hooks
* *hook:uninstall* - Uninstall a hook
* *hook:update* - Update one or more hooks

# Creating a hook

When creating a hook, the important thing to remember is that you are basicly creating a service provider.
There are a few minor things to it, but that is basicly it.

## The composer.json file

The `composer.json` file is one important part of hooks, since this is where you choose your hook's name, dependencies, autoload methods and most important its service providers.
Lets take a example:

> This example is from [`voyager-polls`](https://github.com/thedevdojo/voyager-polls)
```
{
  "name": "voyager-polls",
  "description": "This is my first hook.",
  "tags": ["voyager"],
  "require": {
    "larapack/hooks": "~1.0"
  },
  "autoload": {
    "psr-4": {
      "VoyagerPolls\\": "src/"
    }
  },
  "extra": {
    "hook": {
      "providers": [
        "VoyagerPolls\\PollsServiceProvider"
      ]
    }
  }
}
```

Lets take those properties piece by peice.

### name
First the `name` proberty, this is a required property which is also the most important property.
This is the name of your hook. And unless any other Composer package, this may **not** include any slashes `/`.
In this case the name of the hook is `voyager-polls`, this means that in order to install it you will need to run:
```
php artisan hook:install voyager-polls
```

### description
This is an optional field where you can describe your hook in a few lines.

### tags
You should tag things to make them easier to find, for example I would tag everything related to Voyager with the `voyager` tag.
You can have multiple tags like this: `"tags": ["foo", "bar", "bas"]`

### require
This is where you can define your package requirements.
We recommend that you add the lastest version of `larapack/hooks` that you intend to support.
Along with the latest version of Laravel that you support.
> You can also use `require-dev` for requiring dependencies only on development mode.

### autoload
Here you can define which part of your files should be autoloaded how.
You may read more on this on Composer's website.

### providers
As an extra thing, you can define the service providers that you wish to load once the hook have been enabled.

## Installation

Once you have created your local hook, you can install your it by running `php artisan hook:install NAME` (replace `NAME` with the name of your hook).

