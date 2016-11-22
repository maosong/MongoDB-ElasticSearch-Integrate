# PHP MongoDB Library

官方推荐使用 [PHP MongoDB Library](https://github.com/mongodb/mongo-php-library) 类库，它是对 [PHP MongoDB Driver](https://github.com/mongodb/mongo-php-driver) 的高级抽象，能够大幅简化 CRUD 操作代码量。

```text
We recommend using this extension in conjunction with our userland library, which is distributed as mongodb/mongodb for Composer.
```

## 安装

```bash
$ composer require "mongodb/mongodb=^1.0.0"
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
$client = (new MongoDB\Client($uri));
```

### 创建

```php
//配置
$dbname = 'my_db';
$coll = 'my_coll';
$w = 1; //Write concern 可选值 http://php.net/manual/en/mongodb-driver-writeconcern.construct.php

//创建
$db = $client->selectDatabase($dbname, ['writeConcern' => new MongoDB\Driver\WriteConcern($w, 10000)]);
$collection = $db->selectCollection($coll);
try {
    $result = $collection->insertOne(['_id' => 1, 'title' => 'my title']);
} catch (Exception $e) {
    die('创建失败:' . $e->getMessage());
}

var_dump($result);
```

### 批量创建

```php
//批量创建
try {
    $result = $collection->insertMany([
        [
            '_id' => 2,
            'title' => 'my title 2'
        ],
        [
            '_id' => 3,
            'title' => 'my title 3'
        ]
    ]);
} catch (Exception $e) {
    die('创建失败:' . $e->getMessage());
}

var_dump($result);
```

### 更新

```php
//更新
try {
    $result = $collection->updateOne(['_id' => 1], ['$set' => ['title' => 'my title is update-one!']]);
} catch (Exception $e) {
    die('更新失败:' . $e->getMessage());
}

var_dump($result);
```

### 批量更新

```php
//批量更新
try {
    $result = $collection->updateMany(['_id' => ['$gte' => 2]], ['$set' => ['title' => 'my title is update-many!']]);
} catch (Exception $e) {
    die('更新失败:' . $e->getMessage());
}

var_dump($result);
```

### 查询

```php
//查询
try {
    $row = $collection->findOne(['_id' => 1]);
} catch (Exception $e) {
    die('查询失败:' . $e->getMessage());
}

var_dump($row);
```

### 批量查询

```php
//批量查询
try {
    $rows = $collection->find(['_id' => ['$in' => [2, 3]]], ['sort' => ['_id' => -1]]);
} catch (Exception $e) {
    die('查询失败:' . $e->getMessage());
}

foreach ($rows as $row) {
    var_dump($row);
}
```

### 删除

```php
//删除
try {
    $result = $collection->deleteOne(['_id' => 1]);
} catch (Exception $e) {
    die('删除失败:' . $e->getMessage());
}

var_dump($result);
```
