$ `docker-compose up`

## Shakespeare Dataset

```
$ wget http://media.sundog-soft.com/es6/shakes-mapping.json
$ curl -H "Content-Type: application/json" -XPUT 127.0.0.1:9200/shakespeare --data-binary @shakes-mapping.json
$ wget http://media.sundog-soft.com/es6/shakespeare_6.0.json
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

## Movies dataset

### Add mapping

```
$ curl -H "Content-TYpe: application/json" -XPUT 127.0.0.1:9200/movies -d '{"mappings":{"movie":{"properties":{"year":{"type": "date"}}}}}'
```

### Verify it took

```
$ curl -H "Content-Type: application/json" -XGET 127.0.0.1:9200/movies/_mapping/movie
```

### Insert a single movie

Using the escurl hack in bin

```
bin/escurl -XPUT 127.0.0.1:9200/movies/movie/109487 -d '
quote> {"genre": ["IMAX","Sci-Fi"],"title":"Interstellar","year":2014}'
```
### Get all the movies (just one for now)

```
bin/escurl -XGET "127.0.0.1:9200/movies/movie/_search?pretty"
```

### Expected

```
{
  "took" : 24,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 1,
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "movies",
        "_type" : "movie",
        "_id" : "109487",
        "_score" : 1.0,
        "_source" : {
          "genre" : [
            "IMAX",
            "Sci-Fi"
          ],
          "title" : "Interstellar",
          "year" : 2014
        }
      }
    ]
  }
}
```
