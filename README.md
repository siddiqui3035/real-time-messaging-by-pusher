

## About Realtime Messaging:
Realtime Message Sending by Laravel Pusher

## Install a freash Laravel project.
```php
composer create-project laravel/laravel:^8.0 realtime-messageing-by-pusher

```

## Now enter into project folder:
```php 
npm install

npm run dev
```

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

public function broadcastOn()
{
    return new Channel('chat');
}

&

public function broadcastAs(){
    return 'message';
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

## Now go to js folder;
 open bootstrup.js file and uncommend below part;
 ```php
import Echo from 'laravel-echo';

window.Pusher = require('pusher-js');

window.Echo = new Echo({
    broadcaster: 'pusher',
    key: process.env.MIX_PUSHER_APP_KEY,
    cluster: process.env.MIX_PUSHER_APP_CLUSTER,
    forceTLS: true
});

 ```
## Now install Laravel-echo and pusher.js
```php
npm install --save laravel-echo pusher-js
```

## Now Install pusher:
```php
composer require pusher/pusher-php-server

```
## Now run below command:
```php
npm run watch
```

## Go to app.js file:
```php
const messages_el = document.getElementById("messages");
const username_input = document.getElementById("username");
const message_input = document.getElementById("message_input");
const message_form = document.getElementById("message_form");

message_form.addEventListener('submit', function(e){
    e.preventDefault();
    let has_errors = false;

    if(username_input.value == ""){
        alert("Please enter your Username");
        has_errors = true;
    }

    if(message_input.value == ""){
        alert("Please enter your message");
        has_errors = true;
    }

    if(has_errors){
        return;
    }

    const options = {
        method: 'post', 
        url: '/send-message',
        data: {
            username: username_input.value,
            message: message_input.value,
        }

    }

    axios(options);
});

window.Echo.channel('chat')
 .listen('.message', (e) => {
    // console.log();
    messages_el.innerHTML += '<div class="message"><strong>' + e.username + ':</strong> ' + e.message + '</div>';  
 }) 

```

## Now go to config/app.php:
**uncommand 
```php
App\Providers\BroadcastServiceProvider::class,
```

## Now run below command;

```php 
php artisan serve
```
and test you project.


<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"></a></p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>