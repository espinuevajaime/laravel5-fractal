[![StyleCI](https://styleci.io/repos/32406904/shield)](https://styleci.io/repos/32406904)
[![Build Status](https://travis-ci.org/Cyvelnet/laravel5-fractal.svg?branch=2.0-dev)](https://travis-ci.org/Cyvelnet/laravel5-fractal)
[![Total Downloads](https://poser.pugx.org/cyvelnet/laravel5-fractal/downloads)](https://packagist.org/packages/cyvelnet/laravel5-fractal)
[![Latest Stable Version](https://poser.pugx.org/cyvelnet/laravel5-fractal/v/stable)](https://packagist.org/packages/cyvelnet/laravel5-fractal)
[![Latest Unstable Version](https://poser.pugx.org/cyvelnet/laravel5-fractal/v/unstable)](https://packagist.org/packages/cyvelnet/laravel5-fractal)
[![License](https://poser.pugx.org/cyvelnet/laravel5-fractal/license)](https://packagist.org/packages/cyvelnet/laravel5-fractal)

# laravel5-fractal
A simple fractal service provider and transformer generator for laravel 5 and lumen

<sup>welcome to my first laravel package, lets rock.<sup>

### New: Since version 2.0
* No longer require to call getArray(), getJson() or responseJson()
* `make:transformer UserTransformer -m User` Eloquent model attributes generation.
* `make:transformer Admin/UserTransformer` command now support child directory with namespace generation
* support excludes() child resources.

Require this package with composer using the following command:

````bash
composer require cyvelnet/laravel5-fractal
````

#### Laravel 5

After updating composer, add the ServiceProvider to the providers array in config/app.php for laravel>=5

````php
Cyvelnet\Laravel5Fractal\Laravel5FractalServiceProvider::class,
````

and register Facade

````php
'Fractal' => Cyvelnet\Laravel5Fractal\Facades\Fractal::class
````

#### Lumen

register service provider in /bootstrap/app.php for lumen
    
````php    
$app->register(Cyvelnet\Laravel5Fractal\Laravel5FractalServiceProvider::class);
````

and uncomment the line

````php
$app->withFacades();
````

and finally register Facade with

````php
class_alias(Cyvelnet\Laravel5Fractal\Facades\Fractal::class, 'Fractal');
````


Now you can start using this package with the following simple command

````php
$user = User::find(1);

return Fractal::item($user, new UserTransformer);
````

OR

````php
$users = User::get(); // $users = User::paginate();

return Fractal::collection($users, new UserTransformer);
````

You will automatically gain some extra attributes when you passing a laravel's paginator object.

In case you would like to get only the transformed array, you may do

````php
Fractal::collection($user, new UserTransformer)->getArray();
````

You may now generate transformer classes in artisan

````bash
php artisan make:transformer
````

in this case we are going to use artisan make:transformer UserTransformer, transformer file will automatically created in App\Transfomers\ directory

now you may open your generated transformer file and start formatting your data as you like

````php
public function transform($user)
{
    return [
           'id' => $user->user_id,
           'name' => "{$user->user_firstname} {$user->user_lastname}",
           ...
           ];
}
````

####Since version 2.0, you may enjoy attributes generation transformer with your Eloquent model

````bash
php artisan make:transformer UserTransformer -m User
````

You can also publish the config-file to change implementations to suits you.

````bash
php artisan vendor:publish --provider="Cyvelnet\Laravel5Fractal\Laravel5FractalServiceProvider"
````    
    
##### Auto embed sub resources.

Auto embed sub resouces are disabled by default, to enable this feature, edit ``config/fractal.php`` and set

``autoload => true``

or alternately you may add ``FRACTAL_AUTOLOAD=true`` to ``.env`` file.



###### TO DO
* add functionality to artisan command to generate sub transformer and includes function boilerplate
* allow mapping nested sub resouces to a much more meaningful and shorter identifier.

