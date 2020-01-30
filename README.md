# BEAR.Sample02
BEAR.Skeleton + BEAR.Middleware 

## 手順

### BEAR.Sunday の雛形をインストール

```bash
composer create-project -n bear/skeleton BEAR.Sample02
```

### 動作確認1

```bash
php ./bin/page.php get /
```

出力結果（正常）
```
200 OK
Content-Type: application/hal+json

{
    "greeting": "Hello BEAR.Sunday",
    "_links": {
        "self": {
            "href": "/index"
        }
    }
}
```

### BEAR.Middleware をインストール

http://bearsunday.github.io/manuals/1.0/ja/psr7.html

```bash
cd BEAR.Sample02
composer require bear/middleware
```

```bash
mkdir bootstrap
cp vendor/bear/middleware/bootstrap/bootstrap.php bootstrap/bootstrap.php
```

`source/BEAR.Sample02/bin/page.php` を以下のように変更

変更前
```php
require dirname(__DIR__) . '/autoload.php';
exit((require dirname(__DIR__) . '/bootstrap.php')(PHP_SAPI === 'cli' ? 'cli-hal-app' : 'hal-app'));
```

変更後
```php
$context = 'hal-app';
require dirname(__DIR__) . '/bootstrap/bootstrap.php';
```

### 動作確認2

```bash
php ./bin/page.php get /
```

出力結果（エラー）
```
PHP Fatal error:  Uncaught TypeError: Argument 1 passed to BEAR\Package\AbstractAppModule::__construct() must be an instance of BEAR\AppMeta\AbstractAppMeta, null given, called in /var/www/vhosts/bear/BEAR.Sample02/vendor/bear/middleware/src/Boot.php on line 57 and defined in /var/www/vhosts/bear/BEAR.Sample02/vendor/bear/package/src/AbstractAppModule.php:17
Stack trace:
#0 /var/www/vhosts/bear/BEAR.Sample02/vendor/bear/middleware/src/Boot.php(57): BEAR\Package\AbstractAppModule->__construct(NULL)
#1 /var/www/vhosts/bear/BEAR.Sample02/vendor/bear/middleware/src/Boot.php(27): BEAR\Middleware\Boot->getContxtualModule(Object(BEAR\AppMeta\AppMeta), 'app')
#2 /var/www/vhosts/bear/BEAR.Sample02/bootstrap/bootstrap.php(25): BEAR\Middleware\Boot->getInjector(Object(BEAR\AppMeta\AppMeta), 'app')
#3 /var/www/vhosts/bear/BEAR.Sample02/bin/page.php(7): require('/var/www/vhosts...')
#4 {main}
  thrown in /var/www/vhosts/bear/BEAR.Sample02/vendor/bear/package/src/AbstractAppModule.php on line 17

Fatal error: Uncaught TypeError: Argument 1 passed to BEAR\Package\AbstractAppModule::__construct() must be an instance of BEAR\AppMeta\AbstractAppMeta, null given, called in /var/www/vhosts/bear/BEAR.Sample02/vendor/bear/middleware/src/Boot.php on line 57 and defined in /var/www/vhosts/bear/BEAR.Sample02/vendor/bear/package/src/AbstractAppModule.php:17
Stack trace:
#0 /var/www/vhosts/bear/BEAR.Sample02/vendor/bear/middleware/src/Boot.php(57): BEAR\Package\AbstractAppModule->__construct(NULL)
#1 /var/www/vhosts/bear/BEAR.Sample02/vendor/bear/middleware/src/Boot.php(27): BEAR\Middleware\Boot->getContxtualModule(Object(BEAR\AppMeta\AppMeta), 'app')
#2 /var/www/vhosts/bear/BEAR.Sample02/bootstrap/bootstrap.php(25): BEAR\Middleware\Boot->getInjector(Object(BEAR\AppMeta\AppMeta), 'app')
#3 /var/www/vhosts/bear/BEAR.Sample02/bin/page.php(7): require('/var/www/vhosts...')
#4 {main}
  thrown in /var/www/vhosts/bear/BEAR.Sample02/vendor/bear/package/src/AbstractAppModule.php on line 17
```

↓「app」コンテクストだけのディレクトリができる
```
sh-4.2# ls -l var/tmp/
合計 4
drwxr-xr-x 2 root root 4096  1月 30 15:59 app
sh-4.2#
```