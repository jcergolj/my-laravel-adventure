# Taking care of the code

## Laravel Pint
[Pint](https://laravel.com/docs/10.x/pint) is a package for formatting the code according to Laravel standards.
```bash
composer require laravel/pint --dev
```

```bash
./vendor/bin/pint
```

## PHP Insights
[PHP Insights](https://github.com/nunomaduro/phpinsights): Code analysis for PHP/Laravel
<img src="https://raw.githubusercontent.com/nunomaduro/phpinsights/master/art/preview.png" alt="PHP Insights Preview">

```bash
composer require nunomaduro/phpinsights --dev
```

```bash
php artisan vendor:publish --provider="NunoMaduro\PhpInsights\Application\Adapters\Laravel\InsightsServiceProvider"
```

```bash
php artisan insights
```
<br/>

## Larastan
[Larastan](https://github.com/nunomaduro/larastan) finds errors
<img src="https://raw.githubusercontent.com/nunomaduro/larastan/master/docs/example.png" alt="Larastan Preview">

```bash
composer require nunomaduro/larastan:^2.0 --dev
```

`phpstan.neon` or `phpstan.neon.dist` file in the root of your application

```
includes:
    - ./vendor/nunomaduro/larastan/extension.neon

parameters:

    paths:
        - app/

    # Level 9 is the highest level
    level: 5
```

```bash
./vendor/bin/phpstan analyse --memory-limit=2G
```

## Tighten Tlint
[Tlint](https://github.com/tighten/tlint) is an opinionated code linter.
```bash
composer global require tightenco/tlint
```

```bash
tlint format
```

```php
# option A
$value = 'Hello, World!';

return view('view', compact('value'));

# option B
return view('view', ['value' => 'Hello, World!']);

# option C
return view('view')
    ->with('value', 'Hello, World!');
```

## Bonus: composer command
```bash
 "scripts": {

    "phpstan": "./vendor/bin/phpstan analyse --memory-limit=2G",
    "insights": "@php artisan insights --no-interaction --fix -v",
    "pint": "vendor/bin/pint",
    "tlint": "tlint format"
    "analyse": [
        "@pint",
        "@insights",
        "@phpstan",
        "@tlint"
    ]
},
```

```bash
composer analyse
```

[Next](https://github.com/jcergolj/my-laravel-adventure/blob/master/final-thoughts.md)
