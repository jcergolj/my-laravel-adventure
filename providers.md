# Service Providers

### Service providers are the central place of all Laravel application bootstrapping.

## Unguard
- built in feature that prevents saving fields which aren't specified in $fillable property

```php
<?php

namespace App\Models;

use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    /**
     * The attributes that are mass assignable.
     *
     * @var array<int, string>
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];
}
```

- downside: forget to add field to the property, field is not saved
- solution Model::unguard()
```php
// app/Providers/AppServiceProvider.php
public function boot()
{
    Model::unguard();

    // ...
}
```
- always use `$request->validated()` and not `$request->all()`

## shouldBeStrict
- `preventLazyLoading` -> Prevent model relationships from being lazy loaded.
- `preventSilentlyDiscardingAttributes` -> Prevent non-fillable attributes from being silently discarded.
- `preventAccessingMissingAttributes` -> Prevent accessing missing attributes on retrieved models.

```php
// app/Providers/AppServiceProvider.php
public function boot()
{
    Model::shouldBeStrict(! $this->app->isProduction());

    // ...
}
```

## AppServiceProvider.php
```php
<?php

namespace App\Providers;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Support\Facades\Http;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    /** @return void */
    public function boot()
    {
        Model::unguard();

        Model::shouldBeStrict(! $this->app->isProduction());
    }
}
```

[Next](https://github.com/jcergolj/my-laravel-adventure/blob/master/macros.md)
