Dealing with errors when building an API can be a pain. Instead of manually building error responses you can simply throw an exception that extends from `Symfony\Component\HttpKernel\Exception\HttpException` and the API will automatically handle the response for you.

Here is a list of built-in Symfony exceptions.

```php
Symfony\Component\HttpKernel\Exception\AccessDeniedHttpException
Symfony\Component\HttpKernel\Exception\BadRequestHttpException
Symfony\Component\HttpKernel\Exception\ConflictHttpException
Symfony\Component\HttpKernel\Exception\GoneHttpException
Symfony\Component\HttpKernel\Exception\HttpException
Symfony\Component\HttpKernel\Exception\LengthRequiredHttpException
Symfony\Component\HttpKernel\Exception\MethodNotAllowedHttpException
Symfony\Component\HttpKernel\Exception\NotAcceptableHttpException
Symfony\Component\HttpKernel\Exception\NotFoundHttpException
Symfony\Component\HttpKernel\Exception\PreconditionFailedHttpException
Symfony\Component\HttpKernel\Exception\PreconditionRequiredHttpException
Symfony\Component\HttpKernel\Exception\ServiceUnavailableHttpException
Symfony\Component\HttpKernel\Exception\TooManyRequestsHttpException
Symfony\Component\HttpKernel\Exception\UnauthorizedHttpException
Symfony\Component\HttpKernel\Exception\UnsupportedMediaTypeHttpException
```

As an example you might throw a `ConflictHttpException` when you attempt to update a record that has been updated by another user prior to this update request.

```php
$api->version('v1', function ($api) {
    $api->put('user/{id}', function ($id) {
        $user = User::find($id);

        if ($user->updated_at > app('request')->get('last_updated')) {
            throw new Symfony\Component\HttpKernel\Exception\ConflictHttpException('User was updated prior to your request.');
        }

        // No error, we can continue to update the user as per usual.
    });
});
```

The package automatically catches the thrown exception and will convert it into its JSON representation. The responses HTTP status code is also changed to match that of the exception. A `ConflictHttpException` would result in an HTTP 409 status code and the following JSON representation assuming you haven't changed the default error format.

```json
{
	"message": "User was updated prior to your request.",
	"status_code": 409
}
```

### Resource Exceptions

The following is a list of generic resource exceptions.

```php
Dingo\Api\Exception\DeleteResourceFailedException
Dingo\Api\Exception\ResourceException
Dingo\Api\Exception\StoreResourceFailedException
Dingo\Api\Exception\UpdateResourceFailedException
```

These exceptions are special in that they allow you to pass along any validation errors that occurred when trying to create, update, or delete resources.

As an example you might throw a `StoreResourceFailedException` when you encounter errors when trying to validate the creation of a new user.

```php
$api->version('v1', function ($api) {
    $api->post('users', function () {
        $rules = [
            'username' => ['required', 'alpha'],
            'password' => ['required', 'min:7']
        ];

        $payload = app('request')->only('username', 'password');

        $validator = app('validator')->make($payload, $rules);

        if ($validator->fails()) {
            throw new Dingo\Api\Exception\StoreResourceFailedException('Could not create new user.', $validator->errors());
        }

        // Create user as per usual.
    });
});
```

The package automatically catches the thrown exception and will convert it into its JSON representation. The responses HTTP status code is also changed to match that of the exception. Resources exceptions result in an HTTP 422 status code and the following JSON representation.

```json
{
    "message": "Could not create new user.",
    "status_code": 422,
    "errors": {
        "username": [
            "The username field is required."
        ],
        "password": [
            "The password field is required."
        ]
    }
}
```

### Custom HTTP Exceptions

You can create your own custom HTTP exceptions so long as they extend from `Symfony\Component\HttpKernel\Exception\HttpException` or implement `Symfony\Component\HttpKernel\Exception\HttpExceptionInterface`.

### Custom Exception Responses

If you need to customize the response that exceptions return you can register a custom error handler.

```php
$app['Dingo\Api\Exception\Handler']->register(function (Symfony\Component\HttpKernel\Exception\UnauthorizedHttpException $exception) {
    return Response::make(['error' => 'Hey, what do you think you are doing!?'], 401);
});
```

Now if authentication fails we'll be presented with the following JSON representation.

```json
{
    "error": "Hey, what do you think you are doing!?"
}
```

[← Responses](https://github.com/dingo/api/wiki/Responses) | [Transformers →](https://github.com/dingo/api/wiki/Transformers)
