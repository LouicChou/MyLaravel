# Build Multi Vendor Ecommerce Website (2023)

Use Laragon be host. 

Laragon是整合Apache跟mysql的服務程式，方便開發PHP程式

## Change PHP Version
PHP版本下載: https://windows.php.net/downloads/releases/

將要換的PHP版本程式放在C:\laragon\www\之下，
在Laragon程式中點右鍵>PHP，即可切換版本


## VS Code useful extension
- Laravel Extension pack(整合包)

    Uninstall - EditConfig for vscode, Laravel Create view, Laravel Blade wrapper

- One Dark Pro

## Install Laravel
https://laravel.com/docs/10.x/installation

Laragon點擊下方Terminal

install Laravel tool : Enter "composer global require laravel/installer"

create new Project: Enter "laravel new [Project_Name]"

## open project
Laragon中點右鍵>PHP>www，可看到所有建立的專案

## 切換預設網站
Laragon中點右鍵>第二個項目>Switch Document Root>Select another，選到index網頁的那個路徑，像是Laravel專案就是選[Public]資料夾
## folder structure
- [app]
    - [http]    
        - [Controllers]
        - [Middleware]
        - kernal.php - Middleware中的東西都會註冊在此檔案
    - [Models] - 檢索資料與資料庫通訊
    - [Provider] - 提供者的服務，Router, Event之類的
- [bootstrap] - 基本上不用動
    - [cache] - 系統會放一些暫存文件用於優化
- [config] - 常用
- [database]
    - [seeder] - 假數據庫
    - [migrations] - 重要! 編修資料表
- [public] - 開放檔案，如首頁index.php
- [resource]
    - [css]
    - [js]
    - [views] - 所有HTML程式都在放在這
- [routes] - 所有路由都放在這
- [storage] - 舊版本檔案?
- [test] - 測試檔案可放在這
- [vendor] - 所有laravel依賴庫(連結外部工具庫Libraries)
- .env - 環境變數，放資料庫連接字串或密碼之類的東西
- artisan - 
- composer.json - 所有庫之間關係的內容
- package.json - 有安裝的NPM會在這

## artisan
查看artisan命令 - 在Cmder(terminal)輸入"php artisan"

常用命令:
- "php artisan serve" - 查看laragon伺服器的狀態與ip跟port，8000是laragon的預設PORT
- 所有命令後面加上"-h"就可以查看系統功能指令有哪些，譬如輸入"php artisan serve -h"
- "php artisan route:list" - 顯示所有應用程式的路由列表
- "php artisan tinker" - 進入php的即時運算功能，像是輸入"7 == "7"",會得到Ture的結果，輸入"strlen("ABCDE")"會得到5，可用來在網站運行中創建假數據測試之類的

## VS code的PHP錯誤
- 錯誤訊息: Cannot validate since a PHP installation could not be found. Use the setting 'php.validate.executablePath' to configure the PHP executable.

將C:\Users\user\AppData\Roaming\Code\User\settings.json中

"php.validate.executablePath"的值設定為使用中的PHP版本執行檔路徑

ex: "C:\\laragon\\bin\\php\\php-8.1.10-Win32-vs16-x64\\php.exe"

## Routes

在[routes]>web.php中定義

### 基本語法為`Route::get(URL,方法);`
ex: 

    Route::get('about',function(){
        return "<h1>About Page</h1>;
    })

### 加上參數寫法
    Route::get('about/{id}',function($id){
       return $id;
    });

進入此網頁URL: "Localhost/about/12"

### 使用別名
避免URL太長，可使用此方式

先定義有別名的路由

    Route::get('about', function () {
        return "<h1>About Page</h1>";
    })->name('hello');

以別名方式製作的路由

    Route::get('home',function(){
    return "<a href='".route('hello')."'>home</a>";
    });

進入此網頁URL: "Localhost/home"，進入之後按下連結就會進入about網頁

### 使用別名套用參數
定義使用帶參數的路由別名

    Route::get('contant/{id}',function($id){
    return $id;
    })->name('edit-contant');

寫死帶入1

    Route::get('contant',function(){
        return "<a href='".route('edit-contant',1)."'>about contant</a>";
    });

進入此網頁URL: "Localhost/contant"

### 路由群組
可以在程式碼中省略相同的根目錄customer
或是有其他設計需求，相同的子網頁可以直接複製，只更換根目錄名稱，類似Model的概念

    route::group(['prefix' => 'customer'],function(){
        Route::get('/', function () {
            return "<h1>customer list</h1>";
        });
        
        Route::get('/create', function () {
            return "<h1>customer create</h1>";
        });
        
        Route::get('/show', function () {
            return "<h1>customer show</h1>";
        });
    });
進入此網頁URL: "Localhost/customer"
"Localhost/customer/create"
"Localhost/customer/show"

### Fallback Route
若輸入錯的URL不希望出現404網頁而是指定別的網頁

    route::fallback(function (){
        return "Route not exist";
    });
此段一定要放在web.php的最下面才會生效

## View
給使用者看的網頁都會放在[resouces]>[views]裡

routes預設語法

    Route::get('/', function () {
    return view('welcome');
    });

指的是連接到[resouces]>[views]裡的welcome.blade.php，這就是View()的功能

所以可以在[views]裡建一個名為about.blade.php的網頁，然後在路由的web.php中加入以下語法

    Route::get('/about', function () {
        return view('about');;
    });

URL輸入"http://localhost/about"即可進入

檔案名稱的blade是讓laravel可以在HTML中使用Laravel功能，純PHP若要在PHP中使用PHP語法要加上<?php>，這個Blade可以用"@"開頭來表示此段是寫laravel語法，像是"@if".

### 網頁放在資料夾中
假設在[Views]中新增一個[contant]資料夾，裡面有一個index.blade.php網頁的呼叫方式如下

    Route::get('/contant', function () {
        return view('contant.index');;
    });

