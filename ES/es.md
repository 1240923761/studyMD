# ES DSL

## <b>Create an index</b>

### <b>Create quickly</b>
```json
PUT http://127.0.0.1:9200/yourindex
```
Notice: index name must be <font color=red>lower case</font>

*<font color=grey>注意: 索引名必须全<font color=red>小写</font></font>*

As above index, es will set 5 shards and 1 replica. The index's number of shards <font color=red><b>can not be alter</b></font>, but the index's number of replicas <font color=red><b>can alter at anytime.</b></font>

*<font color=grey>和上面的索引一样，es会设置5个分片和1个副本。索引的shard数量<font color=red>**不能修改**</font>，但是索引的replica数量可以<font color=red>**随时改变**</font>。</font>*

### <b>Create with Settings</b>
```json
PUT http://127.0.0.1:9200/yourindex
{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 0
  }
}
```

### <b>Change the number of replicas</b>
```json
PUT http://127.0.0.1:9200/yourindex/_settings
{
    "number_of_replicas": 0
}
```

### <b>Create mapping(field)</b>
```json
PUT http://127.0.0.1:9200/yourindex/_mapping
{
  "properties": {
    "parent_path": {
      "type": "integer"
    }
  }
}
```

## <b>What is mapping?</b>
Mapping is the process of defining how a document, and the fields it contains, are stored and indexed.

<font color=grey>*映射是定义文档及其包含的字段如何存储和索引的过程。*</font>


Each document is a collection of fields, which each have their own data type. When mapping your data, you create a mapping definition, which contains a list of fields that are pertinent to the document. A mapping definition also includes metadata fields, like the _source field, which customize how a document’s associated metadata is handled.

<font color=grey>*每个文档都是字段的集合，每个字段都有自己的数据类型。在映射数据时，创建一个映射定义，其中包含与文档相关的字段列表。映射定义还包括元数据字段，比如_source字段，它可以定制文档关联元数据的处理方式。*</font>

Use dynamic mapping and explicit mapping to define your data. Each method provides different benefits based on where you are in your data journey. For example, explicitly map fields where you don’t want to use the defaults, or to gain greater control over which fields are created. You can then allow Elasticsearch to add other fields dynamically.

<font color=grey>*使用动态映射和显式映射来定义数据。每种方法根据您在数据旅程中的位置提供不同的好处。例如，显式映射不希望使用默认值的字段，或获得对创建哪些字段的更大控制权。然后，您可以允许Elasticsearch动态添加其他字段。*</font>

### <b>Dynamic mapping</b>

One of the most important features of Elasticsearch is that it tries to get out of your way and let you start exploring your data as quickly as possible. To index a document, you don’t have to first create an index, define a mapping type, and define your fields — you can just index a document and the index, type, and fields will display automatically:

<font color=grey>*Elasticsearch最重要的特性之一是，它试图摆脱您的阻碍，让您尽可能快地开始探索数据。要索引一个文档，你不需要首先创建一个索引，定义一个映射类型，并定义你的字段-你可以只索引一个文档，索引，类型和字段将自动显示:*</font>

```json
PUT data/_doc/1 
{ 
  "count": 5
}
```

This DSL Creates the data index, the _doc mapping type, and a field called count with data type long.

<font color=grey>*这条语句创建了数据索引、_doc映射类型和一个名为count的字段，该字段的数据类型为long。*</font>

<font size=5>Elasticsearch data type</font>

|JSON data type | "dynamic":"true"|"dynamic":"runtime"|
|:----:|:----:|:----:|
|null|No field added|No field added|
|true or false|boolean|boolean|
|double|float|double|
|long|long|long|
|object|object|No field added|
|array|Depends on the first non-null value in the array|Depends on the first non-null value in the array|
|string that passes date detection|date|date|
|string that passes numeric detection|float or long|double or long|
|string that doesn’t pass date detection or numeric detection|text with a .keyword sub-field|keyword|
|||


You can disable dynamic mapping, both at the document and at the object level. Setting the dynamic parameter to <font color=red>**false**</font> ignores new fields, and <font color=red>**strict**</font> rejects the document if Elasticsearch encounters an unknown field.

*<font color=grey>您可以在文档和对象级别禁用动态映射。将动态参数设置为<font color=red>**false**</font>将忽略新字段，如果Elasticsearch遇到未知字段，则<font color=red>**strict**</font>将拒绝该文档。</font>*

If date_detection is enabled (default), then new string fields are checked to see whether their contents match any of the date patterns specified in dynamic_date_formats. If a match is found, a new date field is added with the corresponding format.

*<font color=grey>如果启用了date_detection(默认)，那么将检查新的字符串字段，以查看它们的内容是否匹配dynamic_date_formats中指定的任何日期模式。如果找到匹配，则使用相应的格式添加一个新的日期字段。</font>*

The default value for dynamic_date_formats is:
```json
[ "strict_date_optional_time","yyyy/MM/dd HH:mm:ss Z||yyyy/MM/dd Z"]
```
For example:

```json
PUT my-index-000001/_doc/1
{
  "create_date": "2015/09/02"
}

GET my-index-000001/_mapping 
```
Copy as curl
View in Console
 

The create_date field has been added as a date field with the format:
"yyyy/MM/dd HH:mm:ss Z||yyyy/MM/dd Z".

Disabling date detectionedit
Dynamic date detection can be disabled by setting date_detection to false:

PUT my-index-000001
{
  "mappings": {
    "date_detection": false
  }
}

PUT my-index-000001/_doc/1 
{
  "create_date": "2015/09/02"
}
Copy as curl
View in Console
 

The create_date field has been added as a text field.

Customizing detected date formatsedit
Alternatively, the dynamic_date_formats can be customized to support your own date formats:

PUT my-index-000001
{
  "mappings": {
    "dynamic_date_formats": ["MM/dd/yyyy"]
  }
}

PUT my-index-000001/_doc/1
{
  "create_date": "09/25/2015"
}