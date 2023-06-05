# Macros

- an elegant way of extending "core" Laravel classes
- only classes `Macroable` trait can be exgtended
- macros could be put inside `boot` method of `AppServiceProvider` class

```php
<?php

namespace App\Providers;

use Illuminate\Support\Facades\Http;
use Illuminate\Support\Facades\Response;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    public function boot()
    {
        // config('khc.api_user_agent') -> 'domain.com | support - me@jcergolj.me.uk'

        Http::macro('trello', function () {
            return Http::withHeaders(['Authorization' => 'auth-header'])
                ->withUserAgent(config('project_name.api_user_agent'))
                ->baseUrl('https://api.trello.com/1/');
        });

        // ...
    }
}
```

- my preferred way is to create `macro` classes
```php
<?php

namespace App\Providers;

use App/Macros/HttpMacro;
use Illuminate\Support\Facades\Response;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    public function boot()
    {
        Http::mixin(HttpMacro);
    }
}
```

```php
<?php

namespace App\Macros;


class HttpMacro
{
    public function trello()
    {
        return function () {
            return Http::withHeaders(['Authorization' => 'auth-header])
                ->withUserAgent(config('project_name.api_user_agent'))
                ->baseUrl('https://api.trello.com/1/');
        };
    }

    public function github()
    {
        // ...
    }
}
```

## Macro example from additional-test-assertions-for-laravel package
```php
public function boot()
{
    TestResponse::mixin(new MiddlewareTestAssertions());
    TestResponse::mixin(new ComponentTestAssertions());
}
```

```php
<?php

class ComponentTestAssertions
{
    public function assertViewHasComponent()
    {
        return function ($componentName) {
            // ... actual code
            return $this;
        };
    }
}
```

```php
<?php

class MiddlewareTestAssertions
{
     public function assertMiddlewareIsApplied()
    {
        return function ($middleware) {
           // actual code

            return $this;
        };
    }
}
```

### Usage
```php
class UsersTest extends TestCase
{
    /** @test */
    public function assert_auth_middleware_is_applied()
    {
        $response = $this->delete(route('users.index'));
        $response->assertMiddlewareIsApplied('auth');
    }

     /** @test */
    public function assert_view_has_users_table_component()
    {
        $response = $this->get(route('users.index'));
        $response->assertViewHasComponent('components.users-table');
    }
}
```

[Next](https://github.com/jcergolj/my-laravel-adventure/blob/master/modal-scopes.md)
