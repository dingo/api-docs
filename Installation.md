To install this package you will need:

- Laravel 4.1+
- PHP 5.4

> At this time **Laravel 5** is not supported. Work will be done to make it compatible in the near future.

You must then modify your `composer.json` file and run `composer update` to include the latest version of the package in your project.

```json
"require": {
    "dingo/api": "0.8.*"
}
```

Or you can run the `composer require` command from your terminal.

```
composer require dingo/api:0.8.*
```

> At this time the package is still in a developmental stage and as such does not have a **stable** release.

Once the package is installed you need to open `app/config/app.php` and register the required service provider.

```php
'providers' => [
    'Dingo\Api\Provider\ApiServiceProvider'
]
```

While you're there you can optionally register the `API` alias.

```php
'aliases' => [
    'API' => 'Dingo\Api\Facade\API'
]
```

Lastly you will need to publish the configuration of the package and modify it to suit your needs. This is explained in more detail in the next chapter.

```
php artisan config:publish dingo/api
```

[Configuration →](https://github.com/dingo/api/wiki/Configuration)
