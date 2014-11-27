See the [Authentication](https://github.com/dingo/api/wiki/Authentication) chapter for a guide on how to configure an OAuth 2.0 provider.

### Defining Route Scopes

By using scopes you'll have more control over who can access your protected endpoints. You can define an array of pipe separated string of scopes on a specific route or route group.

#### Route Group Scopes

```php
Route::api(['version' => 'v1', 'protected' => true, 'scopes' => 'read_user_data'], function () {
    // Only access tokens with the "read_user_data" scope will be given access.
});
```

#### Specific Route Scopes

```php
Route::api(['version' => 'v1', 'protected' => true], function () {
    Route::get('user', ['scopes' => 'read_user_data', function () {
        // Only access tokens with the "read_user_data" scope will be given access.
    }]);
});
```

#### Controller Scopes

If your controllers use `Dingo\Api\Routing\ControllerTrait` you can use the `scopes` method.

```php
use Dingo\Api\Routing\ControllerTrait;

class HomeController extends Controller
{
    use ControllerTrait;

    public function __construct()
    {
        $this->scopes('read_user_data');
    }
}
```

You can define the methods you want the scopes to apply to via the second parameter, either as a pipe separated string or as an array. If you do not supply the methods then the scopes will apply to all methods.

```php
use Dingo\Api\Routing\ControllerTrait;

class HomeController extends Controller
{
    use ControllerTrait;

    public function __construct()
    {
        $this->scopes('read_user_data', 'index');
    }

    public function index()
    {
        //
    }
}
```

[← Internal Requests](https://github.com/dingo/api/wiki/Internal-Requests) | [Making Requests To Your API →](https://github.com/dingo/api/wiki/Making-Requests-To-Your-API)
