# PHP MongoDB Driver

PHP7 之前我们通过 pecl 扩展 [mongo](http://pecl.php.net/package/mongo) 访问MongDB，但这个库目前已经停止维护并且仅支持PHP5和MongoDB3.0。原作者开发了 [mongodb](https://github.com/mongodb/mongo-php-driver) 取而代之，同时支持PHP5.4+和MongoDB2.4+，考虑到向PHP7的迁移，我们必须使用新的类库。

⚠️ 注意！MongoDB Driver 使用的是长连接，因此，需要运维控制与MongoDB连接的PHP-FPM进程数量。

## 安装

```bash
$ versionPhpExt=1.1.9
$ wget http://pecl.php.net/get/mongodb-${versionPhpExt}.tgz
$ tar -zxf mongodb-${versionPhpExt}.tgz
$ rm -f mongodb-${versionPhpExt}.tgz
$ rm -f package*.xml
$ cd mongodb-${versionPhpExt}
$ phpize
$ ./configure
$ make -j 8
$ make install
$ cd ..
$ rm -Rf mongodb-${versionPhpExt}
```

将mongodb加入php.ini配置文件

```
extension=mongodb.so
```

## 使用

### 连接

```php
//配置
$host = 'localhost'; //Host
$port = '27017'; //Port
$uri = "mongodb://${host}:${port}?readPreference=secondaryPreferred";
// $uri = "mongodb://${username}:${password}@${host1}:${port1},${host2}:${port2}?replicaSet=${replicaSet}&readPreference=secondaryPreferred";

//连接
$manager = new MongoDB\Driver\Manager($uri);
```

### 创建

```php
//配置
$dbname = 'my_db'; //DB anme
$coll = $dbname . '.my_coll'; //Coll name
$w = 1; //Write concern 可选值 http://php.net/manual/en/mongodb-driver-writeconcern.construct.php

//创建
$writeConcern = new MongoDB\Driver\WriteConcern($w, 10000);
$bulk = new MongoDB\Driver\BulkWrite(['ordered' => true]);
$bulk->insert(['_id' => 1, 'title' => 'my title']);
$bulk->insert(['_id' => 2, 'title' => 'my title']);
$bulk->insert(['_id' => 3, 'title' => 'my title']);
try {
    $result = $manager->executeBulkWrite($coll, $bulk, $writeConcern);
} catch (Exception $e) {
    die('插入失败:' . $e->getMessage());
}

var_dump($result);
```

### 更新

```php
//更新
$bulk = new MongoDB\Driver\BulkWrite(['ordered' => true]);
$bulk->update(['_id' => 2], ['$set' => ['title' => 'my title update']]);
$bulk->update(['_id' => 3], ['$set' => ['title' => 'my title update']]);
try {
    $result = $manager->executeBulkWrite($coll, $bulk, $writeConcern);
} catch (Exception $e) {
    die('更新失败:' . $e->getMessage());
}

var_dump($result);
```

### 查询

查询一个和多个文档的方式相同（使用过滤器），Query类的参数请查看[官方文档](http://php.net/manual/zh/mongodb-driver-query.construct.php)

```php
//查询
$query = new MongoDB\Driver\Query(['_id' => ['$in' => [3, 2, 1]]]);
try {
    $rows = $manager->executeQuery($coll, $query);
} catch (Exception $e) {
    die('查询失败:' . $e->getMessage());
}
foreach ($rows as $row) {
    var_dump($row);
}
```

### 删除

删除文档同样可以使用过滤器，具体请查看[官方文档](http://php.net/manual/zh/mongodb-driver-bulkwrite.delete.php)

```php
//删除
$writeConcern = new MongoDB\Driver\WriteConcern($w, 10000);
$bulk = new MongoDB\Driver\BulkWrite;
$bulk->delete(['_id' => 1], ['limit' => 1]);
try {
    $result = $manager->executeBulkWrite($coll, $bulk, $writeConcern);
} catch (Exception $e) {
    die('删除失败:' . $e->getMessage());
}

var_dump($result);
```
