### Setup

```
$ docker-compose up
$ curl -H "Content-Type: application/json" -XPUT 127.0.0.1:9200/shakespeare --data-binary @shakes-mapping.json
$ curl -H "Content-Type: application/json" -XPOST 'localhost:9200/shakespeare/doc/_bulk?pretty' --data-binary @shakespeare_6.0.json
$ curl -H "Content-Type: application/json" -XGET 'localhost:9200/shakespeare/_search?pretty' -d '
{
    "query": {
        "match_phrase": {
            "text_entry": "to be or not to be"
        }
    }
}'
```

### Expected

```
...
"hits" : [
      {
        "_index" : "shakespeare",
        "_type" : "doc",
        "_id" : "34229",
        "_score" : 13.874454,
        "_source" : {
          "type" : "line",
          "line_id" : 34230,
          "play_name" : "Hamlet",
          "speech_number" : 19,
          "line_number" : "3.1.64",
          "speaker" : "HAMLET",
          "text_entry" : "To be, or not to be: that is the question:"
        }
      }
```
