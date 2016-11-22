# PHP ElasticSearch Library

[elasticsearch-php](https://github.com/elastic/elasticsearch-php) 是官方提供的PHP客户端，具备非常好的扩展能力。比如我们可以实现ES插件的批量删除功能。

## 安装

```bash
$ composer install "elasticsearch/elasticsearch~5.0"
```

## 使用

### 连接

```php
//配置
$config = [
    'hosts' => [
        'http://192.168.110.145:9200',
        'http://192.168.110.138:9200'
    ]
];

//连接
$client = Elasticsearch\ClientBuilder::fromConfig($config);
```

### 创建

```php
//配置
$index = 'my_index';
$type = 'my_type';

//创建
$result = $client->index([
    'index' => $index,
    'type' => $type,
    'id' => 1,
    'body' => ['id' => 1, 'title' => 'my title 1']
]);

var_dump($result);
```

### 批量创建

```php
//配置
$index = 'my_index';
$type = 'my_type';

//批量创建
$params['body'][] = ['index' => ['_index' => $index, '_type' => $type, '_id' => 2]];
$params['body'][] = ['id' => 2, 'title' => 'my title 2'];
$params['body'][] = ['index' => ['_index' => $index, '_type' => $type, '_id' => 3]];
$params['body'][] = ['id' => 3, 'title' => 'my title 3'];
$result = $client->bulk($params);

var_dump($result);
```

### 获取

```php
//配置
$index = 'my_index';
$type = 'my_type';
$id = 1;

//获取
$result = $client->get([
    'index' => $index,
    'type' => $type,
    'id' => $id
]);

var_dump($result);
```

### 更新

```php
//配置
$index = 'my_index';
$type = 'my_type';
$id = 1;

//更新
$result = $client->update([
    'index' => $index,
    'type' => $type,
    'id' => $id,
    'body' => [
        'doc' => [
            'title' => 'my title update'
        ]
    ]
]);

var_dump($result);
```

### 删除

```php
//配置
$index = 'my_index';
$type = 'my_type';
$id = 2;

//删除
$result = $client->delete([
    'index' => $index,
    'type' => $type,
    'id' => $id
]);

var_dump($result);
```

### 查询

```php
//配置
$index = 'my_index';
$type = 'my_type';

//查询
$result = $client->search([
    'index' => $index,
    'type' => $type,
    'body' => [
        'sort' => [
            [
                'id' => [
                    'order' => 'desc' //按ID倒序排序
                ]
            ]
        ],
        'query' => [
            'bool' => [
                'filter' => [
                    [
                        'range' => [
                            'id' => [
                                'gte' => 1,
                                'lte' => 3
                            ]
                        ]
                    ]
                ]
            ]
        ]
    ]
]);

var_dump($result);
```
