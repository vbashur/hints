

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
