

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

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Chat Application</title>
    <link rel="stylesheet" href="./css/app.css"/>
</head>
<body>
    <div class="app">
        <header>
            <h1>Chat Application</h1>
            <input type="text" name="username" id="username" placeholder="Enter your user name....."/>
        </header>
        <div id="messages"></div>
        <form id="message_form">
            <input type="text" name="message" id="message_input" placeholder="Enter your message....."/>
            <button type="submit" id="message_send">Send</button>
        </form>
    </div>
    <script src="./js/app.js"></script>
</body>
</html>

```

## Now run below command;
```php
npm run dev

```

## Add some style on app.css file:
```css

*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: sans-serif;
}

body{
    background-color: #eee;
}

input, button{
    appearance: none;
    border: none;
    outline: none;
    background: none;
}

input{
    display: block;
    width: 100%;
    background-color: #eee;
    padding: 12px 16px;
    font-size: 18px;
    color: #888;
}
 
.app{
    display: flex;
    flex-direction: column;
    height: 100vh;
}

header{
    display: flex;
    padding-top: 128px;
    padding-bottom: 32px;
    background-color: rgb(23, 150, 182);
    background-image: linear-gradient(to bottom, #8C38FF );
    align-items: center;
    justify-content: flex-end;
    box-shadow: 0px 6px 12px rgba(0, 0, 0, 0.15);
    padding-left: 16px;
    padding-right: 16px;
}

h1{
    color: #fff;
    text-transform: uppercase;
    text-align: center;
    margin-bottom: 16px;
}

#username{
    border-radius: 8px;
    transition: 0.4s;
    text-align: center;
}

#username:focus{
    box-shadow: 0px 6px 12px rgba(0, 0, 0, 0.25);
}

#messages{
    flex: 1 1 0%;
    overflow: scroll;
    padding: 16px;
}

.message{
    display: block;
    width: 100%;
    border-radius: 99px;
    background-color: #fff;
    padding: 8px 16px;
    box-shadow: 0px 6px 12px rgba(0,0,0,0.15);
    font-weight: 400;
    margin-bottom: 16px;
}

.message strong{
    color: #8C38FF;
    font-weight: 600;
}

#message_form{
    display: flex;
}

#message_send{
    appearance: none;
    background-color: rgb(23, 150, 182);
    padding: 4px 8px;
    color: #fff;
    text-transform: uppercase;
}

```

## Go to web.php

```php
Route::post('/send-message', function(Request $request){
    event(new MessageRealTime($request->input('username'), $request->input('message')));
    return ["success" => true];
});

```

<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"></a></p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>