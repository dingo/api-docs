Much of the package comes preconfigured so that you can start building your API as soon as possible. You can use your `.env` file to configure most of the package, however, some finer tuning of the package will require you to either publish the configuration file (Laravel) or configure it in `bootstrap/app.php` (Lumen).

If you're using Laravel you can publish the configuration file with the following Artisan command:

```
php artisan vendor:publish --provider="Dingo\Api\Provider\LaravelServiceProvider"
```

#### Vendor

Your vendor name is typically a short name of your application, all lowercase.

You can configure this in your `.env` file.

```
API_PREFIX=app
```

#### Prefixes and Subdomains

If you've ever worked with an API you'll know that most are served from either a subdomain or under a prefix. A prefix or subdomain *is* required, but only one. Avoid putting a version number as your prefix or subdomain as versioning is handled via the `Accept` header.

You can configure this in your `.env` file.

```
API_PREFIX=api
```

Or you can use a domain.

```
API_DOMAIN=api.myapp.com
```

#### Conditional Requests

By default conditional requests are enabled as it will utilize the clients caching capabilities when possible to cache API requests.

You can configure this in your `.env` file.

```
API_CONDITIONAL_REQUEST=false
```

#### Strict Mode

Strict mode will require clients to send the `Accept` header instead of defaulting to the version specified in the configuration file. This means you will not be able to browse the API through your web browser.

If strict mode is enabled and an invalid `Accept` header is used the API will throw an unhandled `Symfony\Component\HttpKernel\Exception\BadRequestHttpException` that should be you should handle appropriately.

You can configure this in your `.env` file.

```
API_STRICT=false
```

#### Authentication Providers

By default only `basic` authentication is enabled. Authentication is covered in more detail in a later chapter.

You must configure this in a published configuration file or in your bootstrap file.

```php
$app['Dingo\Api\Auth\Auth']->extend('oauth', function ($app) {
   return new Dingo\Api\Auth\Provider\JWT($app['Tymon\JWTAuth\JWTAuth']);
});
```

#### Throttling / Rate Limiting

By default rate limiting is disabled. You can register your custom throttles with the rate limiter or use the existing authenticated and unauthenticated throttles.

You must configure this in a published configuration file or in your bootstrap file.

```php
$app['Dingo\Api\Http\RateLimit\Handler']->extend(function ($app) {
    return new Dingo\Api\Http\RateLimit\Throttle\Authenticated;
});
```

#### Response Transformer

Fractal is the default response transformer.

You can configure this in your `.env`. file, however, advanced configuration will need to take place in a published configuration file or in your bootstrap file.

```php
$app['Dingo\Api\Transformer\Factory']->setTransformer(function ($app) {
    $transformer = new League\Fractal\Manager;

    $transformer->setSerializer(new League\Fractal\Serializer\JsonApiSerializer);

    return $transformer;
});

#### Response Formats

The default response format is JSON and a JSON response format is registered by default.

You can configure the default response format in your `.env` file. Further response format configuration will need to take place in a published configuration file or in your bootstrap file.

```
API_DEFAULT_FORMAT=json
```

```php
Dingo\Api\Http\Response::addFormatter('json', new Dingo\Api\Http\Response\Format\Jsonp);
```

#### Error Format

When the package encounters errors it will try to generate a generic error response instead of dumping exceptions to the user. The error format it uses can be configured to your liking.

You must configure this in a published configuration file or in your bootstrap file.

```php
$app['Dingo\Api\Exception\Handler']->setErrorFormat([
    'error' => [
        'message' => ':message',
        'errors' => ':errors',
        'code' => ':code',
        'status_code' => ':status_code',
        'debug' => ':debug'
    ]
]);
```

#### Debug Mode

Generic errors handled by the package include a `debug` key that will be populated with stack trace details when this is enabled.

You can configure this in your `.env` file.

```
API_DEBUG=true
```

[← Installation](https://github.com/dingo/api/wiki/Installation) | [Creating API Endpoints →](https://github.com/dingo/api/wiki/Creating-API-Endpoints)
