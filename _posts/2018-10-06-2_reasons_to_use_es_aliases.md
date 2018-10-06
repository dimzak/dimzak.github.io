---
title: 2 reasons to use ElasticSearch aliases
key: 20181006
tags: elasticsearch
---

ElasticSearch has a very handy feature to use different names to point to indices, [aliases](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-aliases.html).  
Why should I care?  
`Aliases` can simplify and fasten deployment if used properly.  
Below are 2 examples.

## Mapping changes
Elasticsearch has come a long way from `v0.9.x` to `v7.x`.  
I still remember that prior to `v2.x`, default mapping of string fields was `analysed`.  
That was a terrible idea as usually a string field just contains a single word and if someone  
wants some text-search features, she/he declares a custom mapping.  
But no, it was `analysed`, and you were thinking that everything is fine, until the first `prefix` query.  

How aliases can help alleviate this pain?
Mapping changes require a full [reindex](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html).  
By using an alias instead of index names you can point to a newly created index with corrected/improved mappings so that your
applications can index documents without issues while you reIndex existing documents from the old index to the new one.

## Logs generally
Most of Elasticsearch users use it as a log aggregation system.  
That means that there are indices named `application_logs.2018-10-06`,`20181006.data` etc.  
By having an alias pointing to the newest index, it's not mandatory to update properties of all software pieces that need to
connect to that latest index.  
For example, an application that previously used these settings:
```
elasticsearch.search.index=application_logs.2018-10-06
```
will need to be updated every day  
while by having this:
```
elasticsearch.search.index=application_logs
```
where `application_logs` is an alias to the latest index, application will always look at the latest index.

## Notes
* An alias can't have the same name as index. Duh of course.  
But usually indices are not designed in the beginning to have good aliases names.
* Aliases can't be used to [restore](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html#_restore) an index, index name must be declared.
* Below is a simple add/remove alias flow taken from [official documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-aliases.html):
1. Add alias to an existing index
```
POST /_aliases
{
    "actions" : [
        { "add" : { "index" : "test1", "alias" : "alias1" } }
    ]
}
``` 
2. Remove and add alias to another index  
```
POST /_aliases
{
    "actions" : [
        { "remove" : { "index" : "test1", "alias" : "alias1" } },
        { "add" : { "index" : "test2", "alias" : "alias1" } }
    ]
}
```
