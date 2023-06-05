# Model Local Scopes and Query Builders

## Local Scopes
- define common sets of query constraints
- improves readability
- BUT models get bloated

```php
// app/Models/User.php

/** Scope a query to only include active users. */
public function scopeActive(Builder $query): void
{
    $query->where('active', 1);
}

// $user->where('active', 1)->first() === $user->active()->first();
```

 ## Query Builders
 - [Build Your Own Query Builders In Laravel](https://martinjoo.dev/build-your-own-laravel-query-builders)

```php
// app/Models/User.php

use Illuminate\Database\Eloquent\Builder;
use App/QueryBuilders/UserQueryBuilder;

/** @param  Builder  $query */
public function newEloquentBuilder($query): UserQueryBuilder
{
    return new UserQueryBuilder($query);
}
```

```php
// app/QueryBuilders/UserQueryBuilder.php

use Illuminate\Database\Eloquent\Builder;

class UserQueryBuilder extends Builder
{
    public function active()
    {
        return $this->where('active', 1);
    }

    public function deactivate()
    {
        $this->model->active = false;
        $this->model->save();

        return $this;
    }
}
```

[Next](https://github.com/jcergolj/my-laravel-adventure/blob/master/filesystem-disks.md)
