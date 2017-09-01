# Generate Response Class

This package allows you to generate a class that implements Responsable Interface

## Installation

it can be used in Laravel 5.5 or higher

You can install the package via composer:

``` bash
composer require te7a-houdini/response-generate
```

In Laravel 5.5 the service provider will automatically get registered. so you don't have to register the provider in `config/app.php`

## Usage

`php artisan generate:response ExampleResponse` 

you will find a new class created under `App\Http\Responses` namespace which will look like

```php
namespace App\Http\Responses;
    
use Illuminate\Contracts\Support\Responsable;
    
class ExampleResponse implements Responsable
{   
    /**
     * Create an HTTP response that represents the object.
     *
     * @param  \Illuminate\Http\Request $request
     * @return \Illuminate\Http\Response
     */
    public function toResponse($request)
    {
        
    }
}
```
## Example

lets say in your controller you are doing something like that

```php
public function show($id)
{
    $post = Post::find($id);
    
    if (request()->ajax)
    {
        return response()->json(['data' => $post]);
    }
    
    else
    {
        return view('posts.show',compact('post'));
    }
}
```

by using the new generated response class we can do that

```php
public function show($id)
{
    $post = Post::find($id);
    
    return new PostResponse($post);
}
```
```php
class PostResponse implements Responsable
{   
    protected $post;
    
    public function __construct ($post)
    {
        $this->post = $post;
    }
    
    /**
     * Create an HTTP response that represents the object.
     *
     * @param  \Illuminate\Http\Request $request
     * @return \Illuminate\Http\Response
     */
    public function toResponse($request)
    {
        if ($request->ajax())
        {
            return response()->json(['data' => $this->post]);
        }
        else
        {
            return view('posts.show',$post);
        }
    }
}
```

## Credits

- [Te7a Houdini](https://github.com/Te7a-Houdini)
- [All Contributors](../../contributors)

## Resources

- [Adam Wathan Dedicated view objects](https://twitter.com/adamwathan/status/897836363885797376)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.