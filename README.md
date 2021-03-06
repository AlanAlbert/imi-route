# imi-route

中文 | [English](./README-en.md)

使用laravel的方式管理imi的路由。

## 使用

1、使用composer安装依赖：

```sh
composer require alanalbert/imi-route
```
2、将下列代码添加到项目根目录的`Main.php`文件中：
  
```php
<?php
namespace ImiApp;

use Imi\Main\AppBaseMain;
  
class Main extends AppBaseMain
{
  public function __init()
  {
      \Alan\ImiRoute\Route::init(); // Add this line
  }

}
```
3、在项目根目录下，创建`route/route.php`目录及文件，在`route.php`文件中，你就可以管理你的路由了：

```php
/**
 * @var $router Route
 */

use Alan\ImiRoute\Route;
use ImiApp\ApiServer\Controller\IndexController;
use ImiApp\ApiServer\Middleware\Test2Middleware;
use ImiApp\ApiServer\Middleware\TestMiddleware;

$router->group(['middleware' => TestMiddleware::class], function (Route $router) {

    $router->group(
        [
            'middleware' => Test2Middleware::class, 
            'ignoreCase' => true, 
            'prefix' => 'prefix'
        ], function (Route $router) {
        $router->get('hi', 'ImiApp\ApiServer\Controller\IndexController@index');
    });

    $router->any('/hi/api/abc', [IndexController::class, 'index']);

    $router->any('/hi/api/{time}', [IndexController::class, 'api']);

});

$router->group(['prefix' => 'prefix'], function (Route $router) {
    $router->get('/TEST/{time}', [IndexController::class, 'api']);
});
```
