# How does it work? *Analysis*

Analysis = Tokenization + Token Filters

Tokenization breaks sentences into discrete tokens

```
GET /index/_analyze
{
	"tokenizer": "standard",
	"text": "Brown fox brown dog"
}
```

And filters manipulation those tokens

```
GET /index/_analyze
{
	"tokenizer": "standard",
	"filter": ["lowercase"],
	"text": "Brown fox brown dog"
}
```

A tokenizer + 0 or more token filters == analyzer

```
GET /index/_analyze
{
	"analyzer": "standard",
	"text": "Brown fox brown dog"
}
```

Understanding analysis is very important, because the emitted tokens can change a lot.

Example:

```
GET /index/_analyze
{
	"tokenizer": "standard",
	"filter": ["lowercase"],
	"text": "THE quick.brown_FOx Jumped! $3232.323 @ 3.0"
}
```

```
GET /index/_analyze
{
	"tokenizer": "letter",
	"filter": ["lowercase"],
	"text": "THE quick.brown_FOx Jumped! $3232.323 @ 3.0"
}
```

Example: Another example with uax_url_email tokenizer

GET /index/_analyze
{
	"tokenizer": "standard",
	"text": "elastic@example.com website: https://www.elastic.co"
}

// elastic example.com website https www.elastic.co
```


GET /index/_analyze
{
	"tokenizer": "uax_url_email",
	"text": "elastic@example.com website: https://www.elastic.co"
}

elastic@example.com website https://www.elastic.co
```

