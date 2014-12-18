To create an endpoint you must first declare an API route group, which looks something like this.

```php
Route::api('v1', function () {

});
```

This group informs the router that any routes registered within this group should be treated as API endpoints. It works just the same as a regular route group as well. Instead of a plain string as the first parameter you can pass in an array of options.

```php
Route::api(['version' => 'v1', 'before' => 'api.logs'], function () {

});
```

The `api.logs` before filter might be some user defined filter that logs all the API requests. You will notice though that the version is specified, as it is a required option. You can also set a prefix or the domain of a specific API route group in the array of options.

You can also nest regular route groups as well to handle things like namespacing or prefixes.

```php
Route::api('v1', function () {
    Route::group(['prefix' => 'users'], function () {
        // These routes will be prefixed with 'users'.
    });
});
```

An API route group may also indicate multiple API versions. This is useful when you have endpoints that did not change between your API versions.

```php
Route::api(['version' => ['v1', 'v2']], function () {

});
```

Inside your API route group you can now create your endpoints. To do so you just use the standard Laravel rouring methods, like `Route::get`, `Route::post`, `Route::resource`, etc.

```php
Route::api('v1', function () {
    Route::get('users/{id}', 'Api\UserController@show');
});
```

Because routes are grouped per version you can route to the exact same endpoint for a different version of the API.

```php
Route::api('v1', function () {
    Route::get('users/{id}', 'Api\V1\UserController@show');
});

Route::api('v2', function () {
    Route::get('users/{id}', 'Api\V2\UserController@show');
});
```

#### Viewing Routes In The Console

You can view your API routes in the console by using the `api:routes` command.

```
$ php artisan api:routes
```

This command behaves the same as the `routes` command that Laravel ships with.

[← Configuration](https://github.com/dingo/api/wiki/Configuration) | [Responses →](https://github.com/dingo/api/wiki/Responses)
