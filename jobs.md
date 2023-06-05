# Laravel Queues & Jobs
- queued jobs are processed in the background
- e.g. sending emails, parsing CSV files or creating reports, HTTP Client request
- `php artisan make:job TestJob`
- job can dispatch other jobs
- edge case and job fails -> fix bug -> re-run job

```php
// app/Http/Controllers/ConfirmEmailAddress.php
<?php
namespace App\Http\Controllers;

use App\Jobs\TestJob;

class ConfirmEmailAddress extends Controller
{
    public function __invoke(Request $request)
    {
        TestJob::dispatch($request->email);
    }
}
```

```php
// app/Jobs/TestJob.php
<?php

namespace App\Jobs;

use App\Jobs\SendWelcomeEmail;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;

class TestJob implements ShouldQueue
{
    use Dispatchable;

    /** @var string */
    public string $firstName;

    public function handle()
    {
        // ...
        SendWelcomeEmail::dispatch($this->firstName);
    }
}
```






[Next](https://github.com/jcergolj/my-laravel-adventure/blob/master/horizon.md)
