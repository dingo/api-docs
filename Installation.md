To install this package you will need:

- Laravel 5.1+ or Lumen 5.1+
- PHP 5.5.9+

You must then modify your `composer.json` file and run `composer update` to include the latest version of the package in your project.

```json
"require": {
    "dingo/api": "0.10.*"
}
```

Or you can run the `composer require` command from your terminal.

```
composer require dingo/api:0.10.*
```

> At this time the package is still in a developmental stage and as such does not have a **stable** release.
> You may need to set your `minimum-stability` to `dev`.

Once the package is installed the next step is dependant on which framework you're using.

### Laravel

Open `config/app.php` and register the required service provider.

```php
'providers' => [
    Dingo\Api\Provider\LaravelServiceProvider::class
]
```

If you'd like to make configuration changes in the configuration file you can pubish it with the following Aritsan command:

```
php artisan vendor:publish --provider="Dingo\Api\Provider\LaravelServiceProvider"
```

### Lumen

Open `bootstrap/app.php` and register the required service provider.

```
$app->register(Dingo\Api\Provider\LumenServiceProvider::class);
```

[Configuration →](https://github.com/dingo/api/wiki/Configuration)
