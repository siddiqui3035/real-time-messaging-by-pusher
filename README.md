

## About Laravel
Realtime Message Sending by Laravel Pusher

## Install a freash Laravel project.

## Register at pusher website for create pusher app;

```html
https://pusher.com/accounts/sign_in
```
## Go to .env file;

```php

PUSHER_APP_ID=********
PUSHER_APP_KEY=**********************
PUSHER_APP_SECRET=******************
PUSHER_APP_CLUSTER=***

```
&

```php

BROADCAST_DRIVER=pusher

```

## Now create a event:
```php

php artisan make:event eventName
```

## Now go to app/event/event file:
Implements shoundbroadcast on this class;

```php
class MessageRealTime implements ShouldBroadcast
```

&
create 2 public veriables;

```php
public $username;
public $message;

```
&

```php

public function __construct($username, $message)
{
    $this->username = $username;
    $this->message = $message;
}

&

public function broadcastAs(){
    return 'messageRealTime';
}

```

## Go to route/web.php:
```php
use App\Events\MessageRealTime;
use Illuminate\Http\Request;
use Illuminate\Http\Response;

Route::get('/', function () {
    return view('index');
});

```

## Now go to resource/views;
create a index.blade.php file.

## 

<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"></a></p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>