# Aggregations: Get data as a whole

Aggregations can be used to explore your data

```
GET /index/_search
{
	"size": 0,
	"aggs": {
		"popular-colors": {
			"terms": {
				"filed": "colors.keyword"
			}
		}
	}
}
```


And you can search/aggregate at the same time

```
GET /index/_search
{
	"size": 0,
	"query": {
		"match": {
			"title": "dog"
		}
	},
	"aggs": {
		"popular-colors": {
			"terms": {
				"filed": "colors.keyword"
			}
		}
	}
}
```

Multiple aggregations can be calculated at once and can be nested to furhter perform calculations

```
GET /index/_search
{
	"size": 0,
	"aggs": {
		"aggs": {
			"price-statistics": {
				"stats": {
					"filed":"price"
				}
			}
		}
		"popular-colors": {
			"terms": {
				"filed": "colors.keyword"
			},
			"aggs": {
				"avg-price-per-color": {
					"avg": {
						"filed": "price"
					}
				}
			}
		}
	}
}
```