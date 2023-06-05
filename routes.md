# Laravel Routes

## Resource routes
```php
Route::get('/posts', [PostController::class, 'index']);
Route::post('/posts', [PostController::class, 'create']);
Route::get('/posts/{post}', [PostController::class, 'show']);
Route::get('/posts/{post}/edit', [PostController::class, 'edit']);

// client sends data that updates the entire resource e.g. updating all the fields in Blog Post
Route::put('/posts/{post}', [PostController::class, 'update']);

// partial update to the resource e.g. updating only user's email or password
Route::patch('/posts/{post}', [PostController::class, 'update']);

Route::delete('/posts/{post}', [PostController::class, 'destroy']);

Route::options($uri, $callback); // never used it

// short version
Route::resource('posts', PostController::class);
```

## Singleton Resource Routes
```php
Route::get('/profile', [ProfileController::class, 'show']);
Route::get('/profile/edit', [ProfileController::class, 'edit']);
Route::put('/profile', [ProfileController::class, 'update']);
Route::patch('/profile', [ProfileController::class, 'update']);


// short version
Route::singleton('profile', ProfileController::class);
```

## Singleton Action Routes

```php
Route::get('/confirm-email-address', ConfirmEmailAddress::class);
```

### Blade view file
```php

<form method="POST" action="/profile">
    @csrf
    @method('PUT|PATCH|DELETE')
    ...
</form>
```

## Split routes definitions in multiple files
```php
// app/Providers/RouteServiceProvider.php

<?php

namespace App\Providers;

use Illuminate\Cache\RateLimiting\Limit;
use Illuminate\Foundation\Support\Providers\RouteServiceProvider as ServiceProvider;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\RateLimiter;
use Illuminate\Support\Facades\Route;

class RouteServiceProvider extends ServiceProvider
{
    public function boot(): void
    {
        $this->routes(function () {
            Route::middleware('api')
                ->prefix('api')
                ->group(base_path('routes/api.php'));

            Route::middleware('web')
                ->group(base_path('routes/web.php'));

            Route::prefix('webhooks')
                ->middleware('api')
                ->namespace($this->namespace)
                ->group(base_path('routes/webhooks.php'));

            // e.g. laravel breeze uses require __DIR__.'/auth.php';
            // my preferred way is to define auth routes in
            Route::middleware('web')
                ->namespace($this->namespace)
                ->group(base_path('routes/auth.php'));
        });
    }
}
```

[Next](https://github.com/jcergolj/my-laravel-adventure/blob/master/controllers.md)
