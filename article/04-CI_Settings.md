# [Day 4] CI基本設定

CI設定檔架構：
- `/index.php`
- `/application/config/*`

由於設定項繁多，這邊只說明重要設定。

- `/index.php`

    Line 56: 設定環境(修改最後方`development`)
    ```php
    define('ENVIRONMENT', isset($_SERVER['CI_ENV']) ? $_SERVER['CI_ENV'] : 'development');
    ```
    + development 開發模式，顯示所有錯誤
    + testing 測試模式，隱藏所有錯誤
    + production 生產模式，隱藏所有錯誤
- `/application/config/autoload.php` 設定自動載入

    如果要自動載入名為`url`的`helper`
    ```php
    $autoload['helper'] = array('url');
    ```
- `applcation/config/config.php`
    Line 26:
    ```php
    $config['base_url'] = 'http://example.com'; //網站網址
    ```
  
    Line 38:
    ```php
    $config['index_page'] = '';   //index.php名稱(已做URL rewrite所以留空)
    ```
- `application/config/database.php` 設定資料庫連線
    ```php
    $db['default'] = array(             //設定連線名稱(default)
    	'dsn'	=> '',              //設定連接字串(若設定此項則hostname, username, password, database無須設定)
    	'hostname' => 'localhost',  //資料庫主機
    	'username' => '',           //資料庫使用者名稱
    	'password' => '',           //資料庫使用者密碼
    	'database' => '',           //資料庫名稱
    	'dbdriver' => 'mysqli',     //連線用Driver(mysqli或pdo)
    	'dbprefix' => '',
    	'pconnect' => FALSE,
    	'db_debug' => (ENVIRONMENT !== 'production'),
    	'cache_on' => FALSE,
    	'cachedir' => '',
    	'char_set' => 'utf8',
    	'dbcollat' => 'utf8_general_ci',
    	'swap_pre' => '',
    	'encrypt' => FALSE,
    	'compress' => FALSE,
    	'stricton' => FALSE,
    	'failover' => array(),
    	'save_queries' => TRUE
    );
    ```
    連線時，使用$connection = $this->load->database('default', true);連線
- `application/config/routes` 設定路由
    
    保留路由
    ```php
    $route['default_controller'] = 'welcome';   //預設的Controller
    $route['404_override'] = '';                //404執行的Controller
    $route['translate_uri_dashes'] = FALSE;     //將網址中的-(連字號)轉為_(底線)去呼叫Controller
    ```
    一般路由
    ```php
    //規則
    $route['網址']['HTTP請求方式'] = '呼叫的Controller';
  
    //範例
    //當[GET] api/v1/user/(任意文字)時，呼叫api/v1/user/get/(任意文字)
    $route['api/v1/user/(:any)']['get'] = 'api/v1/user/get/$1';
  
    //當[GET] api/v1/data/(任意數字)時，呼叫api/v1/user/data/(任意數字)
    $route['api/v1/data/(:num)']['get'] = 'api/v1/data/get/$1';
  
    //當[POST] api/v1/user/時，呼叫api/v1/user/new
    $route['api/v1/user/']['post'] = 'api/v1/user/new';
    ```