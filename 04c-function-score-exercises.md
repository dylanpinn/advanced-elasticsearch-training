# Elasticsearch Hands On Part 2 Exercises

By default, the function_score query multiplies together the scores from the applicable functions.

You can over-ride that behaviour by specifying a score_mode of "sum":

eg.

```
GET /listings/_search
{
  "query": {
    "function_score": {
      "query": {
        "match_all": {}
      },
      "score_mode": "sum",
      "functions": [
        {
          "filter": {"match": { "product": "premier" }},
          "weight": 30
        },
        {
          "filter": {"match": { "product": "highlight" }},
          "weight": 20
        },
        {
          "filter": {"match": { "product": "standard" }},
          "weight": 10
        },
        {
          "linear": {
            "price": {
              "origin": 800,
              "scale": 1000,
              "decay": 0.5
            }
          }
        }
      ]
    }
  }
}
```


With that in mind:

Write a function_score query that gives a score of

- 30 + x to premier
- 20 + x to highlight
- 10 + x to standard

Where x is a value between 0 and 1 that reflects how close the property price is to 800.
For example,

- A premier property with a price of 800 should get a score of 31.0
- A highlight property with a price of 460 should get a score something like 20.83
(i.e. between 20 and 21)



