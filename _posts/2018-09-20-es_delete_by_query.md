---
title: Delete Elasticsearch index, keep mappings/settings
key: 20180920
tags: elasticsearch tricks
---

One of the most used problems when I develop against `ElasticSearch` is that I often require a clean index to debug or investigate an issue.  
But that index needs to also keep it's original settings and mappings so `dropping` it, is not an option.  
Luckily there is an easy workaround that utilises `delete_by_query api`from `5.x`  
(`delete_by_query plugin` for previous versions, although they are not even supported nowadays).  

###### Simply run:  
```javascript
curl -XDELETE 'http://localhost:9200/index_name/_doc/_delete_by_query?conflicts=proceed' -d '{
    "query" : { 
        "match_all" : {}
    }
}'
```

###### Note 1 
`conflicts=procceed` is needed so that all docs can be removed, even those updated after the execution of above command. 

###### Note 2
As with all heavy queries, ElasticSearch provides a `wait_for_completion=false` parameter to run it as a `_task` (not wait for completion but query tasks api).

###### Note 3
If target contains contains lot of documents, it might be easier to drop and recreate that index. `Delete_by_query` doesn't immediately remove documents,  
it marks them for deletion so they do not appear when searching but actual deletion from disk space happens way after in `segment merge`.

More [here](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-delete-by-query.html)