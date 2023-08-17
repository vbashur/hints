

# Various operations over elastic search


## Analyse searh term and synonyms

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

#### URI search - query lite
```
GET /[index]/_search?q=[search_term]
```
e.g. (be aware of url encoding)
`/movies/_search?q=title:star`
`/movies/_search?q=+year:>2010+title:starwars`
disadvantages: security (access to users who can overload ES), high risk of mistake on typo, it's only appropriate for quick 'curl tests'


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

