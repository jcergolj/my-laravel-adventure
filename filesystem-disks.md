# Multiple storage disk usage

one neat solution for organising files is usage of multiple disks

```php
// config/filesystem.php
<?php

return [
    // ...

    'disks' => [

        'local' => [
            'driver' => 'local',
            'root' => storage_path('app'),
        ],

        'public' => [
            'driver' => 'local',
            'root' => storage_path('app/public'),
            'url' => env('APP_URL').'/storage',
            'visibility' => 'public',
        ],

        'confirmed-addresses' => [
            'driver' => 'local',
            'root' => storage_path('app/confirmed-addresses'),
        ],

        'tmp' => [
            'driver' => 'local',
            'root' => storage_path('app/files/tmp'),
        ],

        // ...
    ],
];
```

### Usage
```php
// app/Http/Controllers/ConfirmEmailAddress.php
<?php

namespace App\Http\Controllers;

use Illuminate\Support\Facades\Storage;

class ConfirmEmailAddress extends Controller
{
    public function __invoke(Request $request)
    {
        Storage::disk('confirmed-addresses')->put('emails.txt', $request->email);

        Storage::disk('tmp')->get('tmp.txt');
    }
}
```



[Next](https://github.com/jcergolj/my-laravel-adventure/blob/master/jobs.md)
