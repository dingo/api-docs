A big part of this package is being able to perform requests on your API internally. This allows you to build your application on top of a consumable API. An internal request will also return the original data instead of a raw response object which means you get all the syntactical sugar that comes with it.

Using the `API` facade we can make an internal call to our API.

```php
Route::get('/', function () {
    $users = API::get('users');

    return View::make('index')->with('users', $users);
});
```

If your controllers use `Dingo\Api\Routing\ControllerTrait` you can use the `$api` property to fire internal requests.

```php
use Dingo\Api\Routing\ControllerTrait;

class HomeController extends Controller
{
    use ControllerTrait;

    public function index()
    {
        $users = $this->api->get('users');

        return View::make('index')->with('users', $users);
    }
}
```

#### Sending Along Data

```php
API::with(['name' => 'Jason', 'location' => 'Australia'])->post('users');
```

Or you can add the data as the second parameter to the `post` method (includes other HTTP verbs, not just `post`).

```php
API::post('users', ['name' => 'Jason', 'location' => 'Australia']);
```

#### Targeting Specific API Version

```php
API::version('v2')->get('users');
```

#### Targeting Specific Domain

```php
API::on('api.example.com')->get('users');
```

#### Attaching Uploaded Files

There's a number of ways you can attach files to be uploaded. You can pass an array of `Symfony\Component\HttpFoundation\File\UploadedFile` instances, which is handy when sending along files you've just uploaded to the API.

```php
API::attach(Input::files())->post('photos');
```

Or you can pass an array of file paths, the key of the array should be the file identifier.

```php
API::attach(['photo' => 'photos/me.jpg'])->post('photos');
```

Or you can pass an array of file paths and associated meta data. Depending on the circumstances this is vastly more accurate then the above method as the package does not need to figure out what the mime type and file size are.

```php
API::attach([
    'photo' => [
        'path' => 'photos/me.jpg',
        'mime' => 'image/jpeg',
        'size' => '49430'
    ]
])->post('photos');
```

You can also mix and match the above methods should you need to.

#### Sending JSON Data

```php
API::json($data)->post('users');
```

If `$data` is an array it will be automatically encoded. The `Content-Type` of this request will be set to `application/json`.

#### Pretend To Be Authenticated User

If some endpoints on your API require authentication you can internally pretend to be a given user. If, for example, a user is logged in to your application using Laravel's authentication you can retrieve the logged in user and pretend to be that user when performing internal requests.

```php
API::be(Auth::user())->get('posts');
```

Any subsequent requests will be authenticated as the same user. To authenticate as a given user for a single request you can use the `once` method.

```php
API::be(Auth::user())->once()->get('posts');
```

#### Retrieve Raw Response Object

All internal requests return the pre-transformed and pre-formatted data. If, for example, your API returns an Eloquent collection from an endpoint you will receive that Eloquent collection internally. Should you require the raw response object you just prefix all calls with `raw`.

```php
$response = API::raw()->get('users');
```

[← Authentication](https://github.com/dingo/api/wiki/Authentication) | [OAuth 2.0 →](https://github.com/dingo/api/wiki/OAuth-2.0)
