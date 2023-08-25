

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
## Print all the indices
```
http://localhost:29200/_cat/indices
```

## Print all the documents from the index

```
http://localhost:29200/webshop-plp-products-local/_search?pretty=true&q=*:*
```

## Query for a particular document in ES
```
http://localhost:29200/webshop-plp-products-local/_doc/000001__01__de
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
**Queries** - return data in terms of relevance

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

#### URI search - query lite
```
GET /[index]/_search?q=[search_term]
```
e.g. (be aware of url encoding)
`/movies/_search?q=title:star`
`/movies/_search?q=+year:>2010+title:starwars`
disadvantages: security (access to users who can overload ES), high risk of mistake on typo, it's only appropriate for quick 'curl tests'


### sorting
#### URI search sorting
```
GET /[index]/_search?q=[search_term]&sort=year&pretty
```
* a string field that is **analyzed** for full text search can't be used to sort documents, because it exists in the inverted index as individual terms, not as the entire string
* to overcome the restriction above we'd need to define the target field with "raw" tag at the mapping, it'll give us unanalyzed version of the string for sorting
* within 'raw' mapping the search would be `/_search?sort=title.raw&pretty`

## Basic operations over documents
add document
```
PUT /[index]/_doc/[document_id]
{ ... }
```

update document
```
POST /[index]/_update/[document_id]
```

delete document
```
DELETE /[index]/_doc/[document_id]
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

