Making a request to your API is quite simple. The best way to do this is using a tool such as [Postman](http://www.getpostman.com/).

Because we aren't versioning the API in the URI we need to define an `Accept` header to request a specific version. The header is formatted like so.

```
Accept: application/vnd.YOUR_VENDOR.v1+json
```

In the above example you would replace `YOUR_VENDOR` with the vendor name you defined in your configuration. Again, this is usually something unique to your application, such as its name or identifier, and is usually all lowercase.

Following the vendor name we have the version we want. In the above example we're requesting `v1` of our API. This is then followed by a plus sign and the desired format. If the format is invalid it the package will use the default format you defined in your configuration.

If you don't want to use Postman you can use a command line tool such as cURL.

```
$ curl -v -H "Accept: application/vnd.YOUR_VENDOR.v1+json" http://example.app/users
```

If you have strict mode enabled and you pass an invalid `Accept` header an unhandled `Dingo\Api\Exception\InvalidAcceptHeaderException` will be thrown. You are required to handle this exception appropriately by utilizing Laravel's exception handling component.

```php
$app['exception']->error(function (\Dingo\Api\Exception\InvalidAcceptHeaderException $exception) {
   // Handle the error here, perhaps return a meaningful response for clients.
});
```

Because this exception is thrown earlier in the API request cycle it needs to be handled manually and the response needs to constructed manually.

[← OAuth 2.0](https://github.com/dingo/api/wiki/OAuth-2.0)
