

# Various operations over elastic search


## Analyse search term and synonyms

Explain what's hapening with search term

**POST** http://[host]/[index]/_analyze
```
{
  "analyzer" : "search_custom_lang",
  "text" : "badschuhe frauen",
  "explain":"true"

}
```

## Bulk insert data

Sample

`curl -H "Content-Type: application/json" -XPUT '127.0.0.1:9200/shakespeare/_bulk' --data-binary @shakespeare_8.0.json`

## Print all the indices
```
http://localhost:29200/_cat/indices
```

## Print all the documents from the index

```
http://localhost:29200/webshop-plp-products-local/_search?pretty=true&q=*:*
```

## Query for a particular document in ES
Default query to get source and and document data
```
http://localhost:29200/webshop-plp-products-local/_doc/000001__01__de
```

Query to get the source data only
```
http://localhost:29200/webshop-plp-products-local/_source/000001__01__de
```

Query to get the doc data only
```
http://localhost:29200/webshop-plp-products-local/_doc/000001__01__de?_source=false
```
Query to get the particular source fields
```
http://localhost:29200/webshop-plp-products-local/_source/000001__01__de?_source_includes=color,baseProductCode,price_eur,productName
```

Query to sort out the particular source fields
```
http://localhost:29200/webshop-plp-products-local/_source/000001__01__de?_source_excludes=color,baseProductCode,price_eur,productName
```

## Filtering
Show the documents with non-empty tag
```
curl -X POST -H 'Content-Type: application/json' -i 'http://[host]:[port]/[index]/_search' --data '{
  "query":{
    "bool":{
      "must":[
        {
          "exists":{
            "field":"[field_name]"
          }
        }
      ],      
      "must_not": [
         {"term": {"[field_name]": ""}}
      ]
    }
  }
}'
```

## Search operations
**Filters** - ask yes/no question of your data (use filters when you can - they are faster and cacheable)

In a filter context, a _query clause_ answers the question “Does this document match this query clause?” The answer is a simple Yes or No — no scores are calculated

**Queries** - return data in terms of relevance

In the query context, a _query clause_ answers the question “How well does this document match this query clause?” Besides deciding whether or not the document matches, the query clause also calculates a relevance score in the `_score` metadata field

[Diff between query and filter with samples](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-filter-context.html)

#### search by some field
examples
```
# query with QUERY, works like a bool filter, but results are scored by relevance
curl -X GET 'http://[host]:[port]/[index]/_search?pretty' --data '{
  "query":{
    "match":{    #other options would be 'match_all' or 'multi_match'
      "title": "star"
    }
  }
}'

# query with FILTER
curl -X GET 'http://[host]:[port]/[index]/_search?pretty' --data '{
  "query":{
    "bool":{
      "must": {"term": {"title": "star"}}
      "filter" : {"range": {"year": {"gte": 2010}}}
    }
  }
}'
```
some samples of useful filters
```
# find documents where a field exists
{"exists": {"field": "tags"}}

# find documents where a field is missing
{"missing": {"field": "tags"}}

```

#### search by phrase
```
curl -X GET 'http://[host]:[port]/[index]/_search?pretty' --data '{
  "query":{
    "match_phrase":{
      "title": {"query": "star wars", "slop": 1 } # slop value relaxes the search to get for example 'star beoynd wars' in output
     }
  }
}'
# for 'slop' 100 might get any document with 'star' and 'wars' with 100 terms to each other, the documents where these phrases closer get higher relevance
```

#### pagination
```
curl -X GET 'http://[host]:[port]/[index]/_search?pretty' --data '{
  "from":0,
  "size":2,
  "query":{ "match":{ "title": "star"}}
}'
```
* deep pagination can kill performance
* every result must be retrieved, collected, and sorted
One can limit the number of results with URL parameter `limit=X`, where X is an upper bound limit

#### prefix/wildcard querying
```
# prefix
curl -X GET 'http://[host]:[port]/[index]/_search?pretty' --data '{
  "query":{ "prefix":{ "year": "20"}}
}'

# wildcard
curl -X GET 'http://[host]:[port]/[index]/_search?pretty' --data '{
  "query":{ "wildcard":{ "year": "20*"}}
}'

```


#### URI search - query lite
```
GET /[index]/_search?q=[search_term]
```
e.g. (be aware of url encoding)
`/movies/_search?q=title:star`
`/movies/_search?q=+year:>2010+title:starwars`
disadvantages: security (access to users who can overload ES), high risk of mistake on typo, it's only appropriate for quick 'curl tests'

#### autocompletion
[Script](http://media.sundog-soft.com/es/sayt.txt) to check autocompletion for queried term
```
while true
do
 IFS= read -rsn1 char
 INPUT=$INPUT$char
 echo $INPUT
 curl --silent --request GET 'http://localhost:9200/autocomplete/_search' \
 --data-raw '{
     "size": 5,
     "query": {
         "multi_match": {
             "query": "'"$INPUT"'",
             "type": "bool_prefix",
             "fields": [
                 "title",
                 "title._2gram",
                 "title._3gram"
             ]
         }
     }
 }' | jq .hits.hits[]._source.title | grep -i "$INPUT"
done
```


### sorting
#### URI search sorting
```
GET /[index]/_search?q=[search_term]&sort=year&pretty
```
* a string field that is **analyzed** for full text search can't be used to sort documents, because it exists in the inverted index as individual terms, not as the entire string
* to overcome the restriction above we'd need to define the target field with "raw" tag at the mapping, it'll give us unanalyzed version of the string for sorting
* within 'raw' mapping the search would be `/_search?sort=title.raw&pretty`

## Basic operations over documents

### add document
```
PUT /[index]/_doc/[document_id]
{ ... }
```

### update document 
#### simple update
```
POST /[index]/_update/[document_id]
```

#### update with script

```
POST /[index]/_update/[document_id]
{
  "script": {
    "source": """
      if(ctx._source.boxoffice_gross_in_millions > 125) 
        {ctx._source.blockbuster = true}
      else 
        {ctx._source.blockbuster = false}
    """
  }
}

```

### delete document
#### simple delete
```
DELETE /[index]/_doc/[document_id]
```

#### delete by query
```
POST /[index]/_delete_by_query
{
  "query" { "match" : { "author" : "Tolstoi" } }
}
```

### update mapping
```
PUT http://[host]:[port]/[index]/_mapping
{
  "properties": {
    "[new_property_name]": {
      "type": "keyword"
    }
  }
}
```

bulk update via curl from file.json
```
~/curlw -XPUT "[host]:[port]/_bulk?pretty" --data-binary @file.json
```

# Aggregations

Sample: get the list of countries encountered for particular query
POST `http://[host]:[port]/[index]/_search?size=0` beware **size=0** at the end to reduce results
```
{
 "query": {
    "match": { "colorName" : "Rot" }
  },
  "aggs": {
    "countries_meet": {
      "significant_terms": {
        "field": "countries",
        "exclude": [ "at", "be", "de", "lv" ]
      }
    }
  }
}
```
#### histograms
Sample: get the list of available prices with the dedicated interval
```
POST `http://[host]:[port]/[index]/_search?size=0` beware **size=0** at the end to reduce results
{
 "query": {
    "match": { "colorName" : "Rot" }
  },
  "aggs": {
    "prices": {
      "histogram": {
        "field": "price_eur",
        "interval": 10.0
      }
    }
  }
}
```

Sample: date histogram, get the monthly documents starting from 1.01.2023
```
{
  "query": {
    "range": {
      "onlineTS": {
        "gte": 1672531200000
      }
    }
  }, 
   
  "aggs": {
    "colors": {
      "date_histogram": {
         "field": "onlineTS",
         "calendar_interval": "month"
         
      }
    }
  }
}
```

#### timeseries
Sample: track by hours some action or since when the product has been product 
```
POST `http://[host]:[port]/[index]/_search?size=0`
{
 "query": {
    "match": { "colorName" : "Rot" }
  },
  "aggs": {
    "timestamp": {
      "date_histogram": {
        "field": "@online_since",
        "calendar_interval": "hour"
      }
    }
  }
}
```


#### nested aggregations
Sample: get the avegarge amount of products on stock for the given price
```
POST `http://[host]:[port]/[index]/_search?size=0`
{
 "query": {
    "match": { "colorName" : "Rot" }
  },
  "aggs": {
    "prices": {
      "terms" : {
         "field": "price_eur",
      },
      "aggs": {
         "avg_stock": {
            "avg" : {
              "field": "stock"
            }      
         }
       }      
    }
  }
}
```

# Operations on shards and indices
Documents are hashed to a particular **shard**

Each shard may be a differnet **node** in a **cluster**

Every shard is a self-contained Lucene index of its own. You can't add more shards later without re-indexing

Create a new index

```
PUT `http://[host]:[port]/[new_index_name]
{
 "settings": {
    "number_of_shards": 10,
    "number_of_replicas": 2
  }
}
```
`number_of_replicas` defines number of replicas per shard, so in the sample above we are getting 30 shards in total: 10 primary + 2*10 replicas

Index lifecycle policies define states of the index: hot, warm, cold, frozen, delete - those policies are relevant to a frequency of index update

we can set those policies based on the size of the index, e.g. after reaching 5 Gb capacity index is marked as 'delete'

Get the status of the cluster
```
GET http://[host]:[port]/_cluster/health
```

Get the shards info
```
GET http://[host]:[port]/_cat/shards?v
```

Get the cluster allocation info
```
GET http://[host]:[port]/_cluster/allocation/explain?pretty
```

### settings of ES indices
* Dynamic - can be changed after index creation
  ** number of replicas
  ** refresh interval
  ** blocks - disabling wirtability/readability of the index
  ** _pipeline - selecting preprocessing pipeline applied to all documents
  
* Static - cannot be changed after index creation
  ** number of shards - the primary shards
  ** codec - compression scheme of the data

  There are two API to configure the number of shards
  * split api - splits the number of shards to an even number, e.g 2 shards splitted to 4 (factor of 2), 2 shards splitted to 6 (factor of 3)
  * shring api - shards down, e.g from 4,2 shards to 1
 
    




