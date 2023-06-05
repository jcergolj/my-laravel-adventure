# Form Request Classes

- use for preventing bloating controllers
- use dedicated FormRequest classes
- `php artisan make:request StoreUserRequest`
- I don't use authorise method

```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;
use Illuminate\Support\Str;

class StorePostRequest extends FormRequest
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

    public function attributes(): array
    {
        return [
            'email.*' => 'The :attribute on row :position is required.'
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

[Next](https://github.com/jcergolj/my-laravel-adventure/blob/master/testing.md)
