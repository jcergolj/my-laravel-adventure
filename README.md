# Laravel Chronicles: Tales from a 10-Year Journey

### 1. Laravel Routes (ruts)
How we define them:
```php
// available methods
Route::get('/user', $callback);
Route::post('/user', $callback);

// client sends data that updates the entire resource e.g. updating all the fields in Blog Post
Route::put($uri, $callback);

// partial update to the resource e.g. updating only user's email or password
Route::patch($uri, $callback);

Route::delete($uri, [UserController::class, 'destroy']);
Route::options($uri, $callback); // I've never used it
```

#### Blade view file
```php

<form method="POST" action="/profile">
    @csrf
    @method('PUT|PATCH|DELETE')
    ...
</form>
```

#### Split routes definitions in multiple files
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
        });
    }
}
```

### Controllers
- 6 methods index, create, show, edit, update, destroy
- must watch: [Cruddy By Design By Adam Wathan](https://www.youtube.com/watch?v=MF0jFKvS4SI)

### Validation
- use dedicated FormRequest classes
```bash
php artisan make:request StoreUserRequest
```


```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;
use Illuminate\Support\Facades\Validator;
use Illuminate\Support\Str;

class StoreAllocatedTokenRequest extends FormRequest
{
    public function authorize(): bool
    {
        // default is false, I always change it to true
        return true;
    }

    public function rules(): array
    {
        return [
            'email' => ['required', 'email'],
        ];
    }

    public function messages(): array
    {
        return [
            'email.required' => 'The :attribute field is required.'
        ];
    }

    public function prepareForValidation(): void
    {
        $this->merge([
            'email' => Str::of($this->email)->trim(' ')->lower(),
        ]);
    }

    protected function passedValidation()
    {
        // e.g. re-arrange params or add new one
    }

    protected function getRedirectUrl(): string
    {
        return back()->withFragment('#step-2')->getTargetUrl();
    }
}
```

### Models

#### Scopes
Scopes improve query readability, however they might bloat the model class.

To prevent Models to be too bloated move
#### Casts
```php
<?php

namespace App\Models;

use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    protected $casts = [
        'email_verified_at' => 'datetime',
        'status' => ServerStatus::class, // server status is enum class
        'stats' => StatsCast
    ];
}
```

```php
<?php

namespace App\Casts;


```

```php
<?php

namespace App\Casts;


```

### Jobs

### Horizon

### Macros

### Testing

### Additional Resources

### Events

### Middleware

### My AppServiceProvider

### Enums





### General suggestions
- embrace and browse through laravel source code


### twitter, github: @jcergolj
### web:jcergolj.me.uk
