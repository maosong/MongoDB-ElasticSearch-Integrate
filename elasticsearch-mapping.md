# ElasticSearch mapping

## MySQL/ES Number类型对照表

| MySQL           | ElasticSearch   |
| --------------- | --------------- |
| bigint          | long            |
| int, mediumint  | integer         |
| smallint        | short           |
| tinyint         | byte            |
| double          | double          |
| float           | float           |

⚠️ 注意！ElasticSearch的Number类型不支持unsigned属性！

## win_incdr 表

**PUT http://192.168.110.145:9200/ccod**

**PUT http://192.168.110.145:9200/ccod/win_incdr/_mapping**

```json
{
    "win_incdr": {
        "_source": {
            "enabled":  false
        },
        "_all": {
            "enabled":  false
        },
        "properties": {
            "id": {
                "type": "long",
                "index": "not_analyzed"
            },
            "ag_num": {
                "type": "string",
                "index": "not_analyzed"
            },
            "result": {
                "type": "byte",
                "index": "not_analyzed"
            },
            "caller": {
                "type": "string",
                "index": "not_analyzed"
            },
            "called": {
                "type": "string",
                "index": "not_analyzed"
            },
            "que_id": {
                "type": "integer",
                "index": "not_analyzed"
            },
            "server_num": {
                "type": "string",
                "index": "not_analyzed"
            },
            "group_id": {
                "type": "integer",
                "index": "not_analyzed"
            },
            "start_time": {
                "type": "integer",
                "index": "not_analyzed"
            },
            "all_secs": {
                "type": "integer",
                "index": "not_analyzed"
            }
        }
    }
}
```
