# Querying and Filtering


## Queries

### Basic Search

Find *all* documents

`GET /index/type/_search` : Find *all* documents

Let's find all "fox" documents

```
GET /index/type/_search
{
	"query": {
		"match": {
			"title": "fox"
		}
	}
}
```

Find "quick" and "dog". Search all documents with the word quick or dog

```
GET /index/type/_search
{
	"query": {
		"match": {
			"title": "quick dog"
		}
	}
}
```

Let's be more strick and only ask for phrases

```
GET /index/type/_search
{
	"query": {
		"match_phrase": {
			"title": "quick dog"
		}
	}
}
```


### Boolean Combinations

Let's find all docs with "quick" and "lazy dog"

```
GET /index/type/_search
{
	"query": {
		"bool" {
			"must": 
			[
				{
					"match": {
						"title":"quick"
					}
				},
				{
					"match_phrase": {
						"title": "lazy dog"
					}
				}
			]
		}
	}
}
```

Negate parts of a query

```
GET /index/type/_search
{
	"query": {
		"bool" {
			"must_not": 
			[
				{
					"match": {
						"title":"quick"
					}
				},
				{
					"match_phrase": {
						"title": "lazy dog"
					}
				}
			]
		}
	}
}
```

Combinations can be boosted for different effects. It should either have "quick dog" in the title field or "lazy dog"

```
{
	"query": {
		"bool": {
			"should": [
				{
					"match_phrase": {
						"title": {
							"query": "quick dog"
						}
					}
				},
				{
					"match_phrase": {
						"title": {
							"query": "lazy dog",
							"boost": 3
						}
					}
				}
			]
		}
	}
}
```

### Filtering

Filtering is often faster than querying. Because Filtering does not have to calcuate _score

```
{
	"query": {
		"bool": {
			"must": [
				{
					"match": {
						"title":"dog"
					}
				}
			],
			"filter":{
				"range": {
					"price": {
						"gte": 5,
						"lte": 10
					}
				}
			}
		}
	}
}
```

Filtering can also be applied without a query.

```
{
	"query": {
		"bool": {
			"filter":{
				"range": {
					"price": {
						"gte": 5,
						"lte": 10
					}
				}
			}
		}
	}
}
```

