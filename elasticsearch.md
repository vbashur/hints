

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

http://localhost:29200/webshop-plp-products-local/_search?pretty=true&q=*:*



