Transformers allow you to easily and consistently transform objects into an array. By using a transformer you can type-cast integers and booleans, include pagination results, and nest relationships.

### Terminology

The word "transformer" is used quite a bit in this section. It's worth noting what the following terms mean when used throughout this section.

- The **transformation layer** is usually a library that prepares and handles transformers.
- A **transformer** is a class that will takes raw data and returns it as a presentable array ready for formatting. The way a transformer is handled is dependant upon the transformation layer.

### Using Transformers

There are three ways to make use of transformers.

#### Bind A Class To A Transformer

```php
API::transform('User', 'UserTransformer');
```
#### Implement An Interface On The Class

```php
class User extends Eloquent implements Dingo\Api\Transformer\TransformableInterface
{
    public function getTransformer()
    {
        return new UserTransformer;
    }
}
```

#### Use The Response Builder

Refer to the [Response Builder](https://github.com/dingo/api/wiki/Responses#response-builder) chapter for help on how to do this.

### Fractal

[Fractal](http://fractal.thephpleague.com) is the default transformation layer used by the package. It boasts a number of useful features to help you keep your data consistent.

To use Fractal it's recommend you read through the documentation found on their website.

#### Automatic Relationship Eager-loading

When using Fractal's includes feature to embed relationships you should ensure you name them the same as the relationships on your models. This package will automatically eager-load those relationships for you to save on query counts.

#### Advanced Configuration

Fractal is registered as the default transformation layer and with the default options. To configure the include key and separator used when defining relationships to embed you must manually instantiate the `FractalTransformer` instance in your configuration file.

```php
'transformer' => new Dingo\Api\Transformer\FractalTransformer(new League\Fractal\Manager, 'include', ',')
```

#### Advanced Usage With Response Builder

Using Fractal in conjunction with the response builder is typically the best way to return your data from controllers.

The `item`, `collection`, and `paginator` methods on the response builder all receive a few extra parameters that can be used to further customize Fractal.

##### Resource Key

```php
return $this->item($user, new UserTransformer, ['key' => 'user']);
```

##### Utilizing The Callback

The Fractal transformation layer allows you to register a callback that is fired after the creation of the resource. This callback receives either an instance of `League\Fractal\Resource\Item` or `League\Fractal\Resource\Collection`as its only parameter. You can then use this to interact with the resource at a much more complex level.

The most obvious use case is to set the cursor implementation when wanting to paginate data.

```php
return $this->collection($users, new UserTransformer, [], function ($resource) {
    $resource->setCursor($cursor);
});
```

### Custom Transformation Layer

Should you need a more custom approach to how your data is transformed then you can easily implement your transformation layer with this package. You'll need to create a  class that implements `Dingo\Api\Transformer\TransformerInterface` and has the required `transform` method.

```php
use Illuminate\Http\Request;
use Dingo\Api\Transformer\Binding;
use Dingo\Api\Transformer\TransformerInterface;

class MyCustomTransformer implements TransformerInterface
{
    public function transform($response, $transformer, Binding $binding, Request $request)
    {
        // Make a call to your transformation layer to transformer the given response.
    }
}
```

This `transform` method is the only required method, you're free to add as many other methods as you like. The purpose of the `transform` method is to take the `$response`, and hand it off to your transformation layer along with the `$transformer`. Your transformation layer should then return an array which is then returned by the `transform` method.

The `$binding` parameter may be useful should your transformation layer include more advanced features such as the ability to add meta data or allow other developers to interact with your layer via the callback.

The `$request` parameter is the current HTTP request being executed and is useful when your transformation layer requires query string values or other related data.

[← Errors And Error Responses](https://github.com/dingo/api/wiki/Errors-And-Error-Responses) | [Authentication →](https://github.com/dingo/api/wiki/Authentication)
