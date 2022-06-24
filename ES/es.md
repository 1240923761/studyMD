# ES DSL

## Create an index

### Create quickly
```cmd
PUT http://127.0.0.1:9200/yourindex
```
<b>Notice: <font color=red>index name must be lower case</font></b>

As above index, es will set 5 shards and 1 replica. The index's number of shards <font color=red><b>can not be alter</b></font>, but the index's number of replicas <font color=red><b>can alter</b></font> at anytime.

### Create with Settings
```cmd
PUT http://127.0.0.1:9200/yourindex
{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 0
  }
}
```

### Change the number of replicas
```cmd
PUT http://127.0.0.1:9200/yourindex/_settings
{
    "number_of_replicas": 0
}
```
### Create mapping(field)
```cmd
PUT http://127.0.0.1:9200/yourindex/_mapping
{
  "properties": {
    "parent_path": {
      "type": "integer"
    }
  }
}
```
### What is mapping?
