[![Build Status](https://img.shields.io/travis/petecoop/laravel-inky.svg)](https://travis-ci.org/petecoop/laravel-inky)

Allows you to use Foundation for Email(https://foundation.zurb.com/emails.html) email templates nicely in Laravel 5.

Any views with a `.inky.php` extension will be compiled with both Inky and Blade, allowing you to use both templating engines seamlessly together. CSS is automatically inlined so styles work in email clients that don't support external stylesheets.

## Installation

Require with composer
```
composer require petecoop/laravel-inky
```

Once installed, you'll need to register the service provider. Open `config/app.php` and add to the `providers` key:

```
Petecoop\LaravelInky\InkyServiceProvider::class
```

Move the assets to your application

```
php artisan vendor:publish --provider="Petecoop\LaravelInky\InkyServiceProvider"
```

## Usage

Check the [Foundation for Emails docs](http://foundation.zurb.com/emails/docs/index.html) for full usage on how to use Inky and Foundation for Emails CSS.

Create an Inky view e.g. `emails/welcome.inky.php`

```blade
<container>
  <row>
    <columns>Welcome, {{ $name }}</columns>
  </row>
</container>
```

Use `Mail` as usual in Laravel

```php
Mail::send('emails.welcome', ['name' => $user->name], function ($m) use ($user) {
  $m->from('hello@app.com', 'Your Application');

  $m->to($user->email, $user->name)->subject('Welcome!');
});
```

You can create a Blade layout to inherit from e.g. `emails/layout.inky.php`

```blade
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
  <meta name="viewport" content="width=device-width"/>
  <link rel="stylesheet" href="{{ asset('css/foundation-emails.css') }}">
</head>
<body>
  @yield('content')
</body>
</html>
```

then

```blade
@extends('emails.layout')

@section('content')
  <container>
    <row>
      <columns>Welcome, {{ $name }}</columns>
    </row>
  </container>
@stop
```

### CSS Inlining

`<style>` and `<link rel="stylesheet">` are automatically inlined.

Here's a handy reference for CSS in emails: [CSS Support Guide for Email Clients](https://www.campaignmonitor.com/css/)

## Licence

[MIT Licence](LICENCE)