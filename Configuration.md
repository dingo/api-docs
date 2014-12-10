Much of the package comes preconfigured so that you can start building your API as soon as possible. If you haven't already publish the packages configuration file.

```
php artisan config:publish dingo/api
```

Open `app/config/packages/dingo/api/config.php` in your editor. The *very* first thing you need to change is the API vendor. This is typically the name of your application or an abbreviated name that is lowercase, although it doesn't have to be. Try to pick something unique though.

#### Prefixes and Subdomains

If you've ever worked with an API you'll know that most are served from either a subdomain or under a prefix. You can set the prefix and/or the subdomain on a global level for your API in the configuration file. Avoid putting a version in the prefix as versioning is done via the `Accept` header, this will be covered later.

#### Conditional Requests

By default `conditional_request` is enabled as it will utilize the clients caching capabilities when possible to cache API requests. If you prefer you can disable this and enable it on a per-route basis.

#### Strict Mode

Strict mode will require clients to send the `Accept` header instead of defaulting to the version specified in the configuration file. This means you will not be able to browse the API through your web browser.

If strict mode is enabled and an invalid `Accept` header is used the API will throw an unhandled `Dingo\Api\Exception\InvalidAcceptHeaderException` that should be you should handle appropriately. You can catch this exception by utilizing Laravel's exception handling component.

#### Authentication Providers

By default only `basic` authentication is enabled. Authentication is covered in more detail in a later chapter.

#### Throttling / Rate Limiting

By default rate limiting is enabled by using two throttles, an authenticated and an unauthenticed throttle. You can adjust the limits and expiration times or remove both throttles to disable rate limiting. Throttling and rate limiting is covered in more detail in a later chapter.

#### Response Transformer

Fractal is the default response transformer. Transformers are covered in more detail in a later chapter.

#### Response Formats

By default the only response format registered is JSON. Response formats are covered in more detail in a later chapter.

[← Installation](https://github.com/dingo/api/wiki/Installation) | [Creating API Endpoints →](https://github.com/dingo/api/wiki/Creating-API-Endpoints)
