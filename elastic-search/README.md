# Elastic Search

- Inverted Index
- About Elastic Search
- Basics
- Executing CRUD operations
- Executing search requests

**NOTE:** Using version 5.4.0 for this tutorial.

## Inverted Index

- Inverted Index
  - Apache Lucene
    - Solr
    - Nutch
    - Elastic Search

Searches using inerted indices:

- Geo-hashes for geographical search
- Metaphone for phonetic matching
- "Do you mean?" uses Levenshtein automaton

### Apache Lucene

The indexing and search library for a high performance, full text search engines.

- provides no crawling
- written in java
- analogous to hadoop in distributed computing world. and is the center of other technologies built around it.

### Apache Solr

Its a search server with:

- distibuted indexing
- load balancing
- replication
- automated recovery
- centralized configuration
- distributed computng features

### Apache Nutch

Provides web crawling and index parsing. This can be used with lucene to form a complete search engine.

### CrateDB

Open source SQL distributed database, built on top lucene.

### Elastic Search

Distributed search and analytics engine, which runs on Lucene.

- written in java
- uses lucene as its search library

## About Elastic Search

Search and analytics engine written in Java build on Apache Lucene.

- Distributed: Scales to thousands of nodes
- High avaialablity: Multiple copies of data
- RESTful API: CRUD, admistrating, monitorying ond other operation via simple JSON based HTTP calls
- Powerful Query DSL: Easy to express complex queries
- Schemaless: Index data without an explicit schema

Features:

- 'Near Real Time Search' - Very low latency (less than a second) for document to be indexed and be searchable.
- 'Distributed' - Runs on multiple server, each server instance is called a node.
- 'Index' - Data to be indexed is in the form of documents, documents are divided into different types. All these documents make up an index.
- 'JSON' - Documents are expressed in JSON and each has the type and category information regarding the document.
- 'Shards' - Documents might be too big to fit onto a single machine. So documents can be split across different nodes and search in such case would happen across all these nodes in parallel.
- 'Replicas' - Replica is setup for each node of the Shard. In case of node failure, replica is used.

### Use cases

- Product catalog
- Autocomplete
- Video clips catogories and tags
- Courses, Authors and Topics
- Mining log data for ingisht
- Pricing alert service
- Business analytics and intelligence

### Running Elastic Search on local

> ./bin/elasticsearch -Ecluster.name=pluralsight_test -Enode.name=j_node

## Basics

Elastic search provides a very low latency, `~1 second` from the time a document is `indexed` untill it becomes `searchable`.

Elastic search is distributed by nature.

### Nodes and Cluster

Node

- Single server
- Performs indexing
- Allows search
- Has a unique id and name

Cluster

- collection of nodes
- can scale to single or thousand node
- holds the entire indexed data
- nodes join a cluster using the cluster name
- nodes communicate by sending each other messages
- the machines on the cluster have to be on the same network

### Documents

- Documents are divided into categories or types.
- All the documents make up an index.
- Basic unit of information to be indexed.
- Expressed as a JSON.
- Assigned with a `type`.

#### Index

- Index is a collection of similar documents.
- An index is uniquely identified by its name.
- A cluster can consist of a number of indexes.
- Different indexs are used for different logical groupings.

#### Types

- Documents are logically partitioned using types.
- Blog post can be one type, blog comment can be aonther.
- Documents with the same fields belong to one type.

### Sharding

A document may be too large to fit into one single hard disk. And it would be too slow to serve all search request from one node.

- Sharding is a process of splitting the index across multiple nodes in a cluster.
- This allows us to search in parallel on multiple nodes.
- To make the nodes highly available, we create replicas to handle the request in case of node failures.
- By default an index has 5 shards and 1 replica by default.

### Health

- http://localhost:9200
- http://localhost:9200/_cat/health?v&pretty
- http://localhost:9200/_cat/nodes?v&pretty

## Executing CRUD operations

All operations are exposed using REST endpoints.

PUT request is idempotent, you will get the same result irrespective of number of times its run.

POST request is not idempotent.

Creating index:

> curl -XPUT http://localhost:9200/products?pretty

Listing indices:

> curl -XGET http://localhost:9200/_cat/indices?pretty

Adding document to an index:

> curl -XPUT http://localhost:9200/products/mobiles/1?pretty -d '{ "name": "Nokia N70", "text" : "My phone long time ago" }'

### Getting document

Getting document to an index:

> curl -XGET http://localhost:9200/products/mobiles/1?pretty

    {
        "_index" : "products",
        "_type" : "mobiles",
        "_id" : "1",
        "_version" : 1,
        "found" : true,
        "_source" : {
            "name" : "hello world",
            "text" : "dummy text field"
        }
    }

Getting document that doesnt exist would return a response that says:

> curl -XGET http://localhost:9200/products/mobiles/10?pretty

    {
        "_index" : "products",
        "_type" : "mobiles",
        "_id" : "10",
        "found" : false
    }

Getting partial details from a document:

> curl -XGET 'http://localhost:9200/products/mobiles/1?pretty&_source=text'

    {
        "_index" : "products",
        "_type" : "mobiles",
        "_id" : "1",
        "_version" : 2,
        "found" : true,
        "_source" : {
            "text" : "dummy text field"
        }
    }

### Updating document

When we pass a document id that exists, we perform a update instead of create. Response would have an `updated version number`.

> curl -XPUT http://localhost:9200/products/mobiles/1\?pretty -d '{ "name": "Nokia N70", "text" : "My phone long time ago", , "reviews": [] }'

    {
        "_index" : "products",
        "_type" : "mobiles",
        "_id" : "1",
        "_version" : 2,
        "result" : "updated",
        "_shards" : {
            "total" : 2,
            "successful" : 1,
            "failed" : 0
        },
        "created" : false
    }

Updating using `_update`:

> curl -XPOST http://localhost:9200/products/mobiles/1/_update?pretty -d '{ "doc": { "reviews": ["i love this phone"], "launchYear": 2005 } }'

    {
        "_index" : "products",
        "_type" : "mobiles",
        "_id" : "1",
        "_version" : 3,
        "found" : true,
        "_source" : {
            "name" : "Nokia N70",
            "text" : "My phone long time ago",
            "reviews" : [
            "i love this phone"
            ],
            "launchYear" : 2005
        }
    }

Doing `POST` without `_update` would cause a replace instead of an update to the existing fields.

> curl -XPOST http://localhost:9200/products/mobiles/1?pretty -d '{ "doc": { "reviews": ["i love this phone"], "launchYear": 2005 } }'

    {
        "_index" : "products",
        "_type" : "mobiles",
        "_id" : "1",
        "_version" : 4,
        "found" : true,
        "_source" : {
            "doc" : {
                "reviews" : [
                    "i love this phone"
                ],
                "launchYear" : 2000
            }
        }
    }

Doing update which doesnt change the document would have reponse containing `result: "noop'`.

#### Update using script field

Here we create a document that we would update using the script tag:

> curl -XPUT http://localhost:9200/products/shoes/1?pretty -d '{ "name": "Nike", "size": 10 }'

    {
        "_index" : "products",
        "_type" : "shoes",
        "_id" : "1",
        "_version" : 1,
        "found" : true,
        "_source" : {
            "name" : "Nike",
            "size" : 10
        }
    }

Now we use the script field to update this:

> curl -XPOST http://localhost:9200/products/shoes/1/_update?pretty -d '{ "script": "ctx.\_source.size += 2" }'

This would increment the size from 10 to `size: 12`. We can do a decrement similarly.

### Deleting

#### Deleting document

Lets create a test index and dummy document that we would delete using DELETE endpoint.

> curl -XPUT http://localhost:9200/test?pretty

> curl -XPUT http://localhost:9200/test/dummy/1\?pretty -d '{ "name": "useless", "desc": "self explanatory" }'

Now we can use the `HEAD` request to check if the a index or a document exists:

> curl -I -XHEAD http://localhost:9200/test?pretty

    HTTP/1.1 200 OK
    content-type: application/json; charset=UTF-8
    content-length: 871

> curl -I -XHEAD http://localhost:9200/test/dummy/1?pretty

    HTTP/1.1 200 OK
    content-type: application/json; charset=UTF-8
    content-length: 173

> curl -XDELETE http://localhost:9200/test/dummy/1?pretty

    {
        "found" : true,
        "_index" : "test",
        "_type" : "dummy",
        "_id" : "1",
        "_version" : 2,
        "result" : "deleted",
        "_shards" : {
            "total" : 2,
            "successful" : 1,
            "failed" : 0
        }
    }

#### Deleting Index

Getting the list of indexes before deletion of index:

> curl -XGET http://localhost:9200/_cat/indices?pretty

    yellow open products 8hPTkNAzQZi_qAfBz__taQ 5 1 3 0 12.6kb 12.6kb
    yellow open test     F_uGxmyJQZ-JHWFeTjwdiw 5 1 0 0  4.4kb  4.4kb

> curl -I -XDELETE http://localhost:9200/test?pretty

    HTTP/1.1 200 OK
    content-type: application/json; charset=UTF-8
    content-length: 28

Now check if the index has been deleted:

> curl -XGET http://localhost:9200/_cat/indices\?pretty

    yellow open products 8hPTkNAzQZi_qAfBz__taQ 5 1 3 0 12.6kb 12.6kb

### BULK operations on documents

#### Retrieve multiple documents `_mget`

Getting documents without specifing index in the URL:

```
curl -XGET http://localhost:9200/_mget?pretty -d '
{
    "docs": [
        {
            "_index": "products",
            "_type": "mobiles",
            "_id": "1"
        },
        {
            "_index": "products",
            "_type": "mobiles",
            "_id": "2"
        }
    ]
}
```

If you specify the index in the url, your request becomes:

```
curl -XGET http://localhost:9200/products/_mget?pretty -d '
{
    "docs": [
        {
            "_type": "mobiles",
            "_id": "1"
        },
        {
            "_type": "mobiles",
            "_id": "2"
        }
    ]
}
```

If you further specify the document type, then only document id needs to be specified within the body:

```
curl -XGET http://localhost:9200/products/mobiles/_mget?pretty -d '
{
    "docs": [
        {
            "_id": "1"
        },
        {
            "_id": "2"
        }
    ]
}
```

#### Adding multiple documents

You can specify what documents to add and where to add them using following command:

```
curl -XPOST "http://localhost:9200/_bulk?pretty" -d '
{ "index" : { "_index": "products", "_type": "mobiles", "_id": "2"}}
{ "name": "N71", "text": "successor but wasnt so differnt", "color": "black" }
{ "index" : { "_index": "products", "_type": "mobiles", "_id": "3"}}
{ "name": "N72", "text": "this was loved by many", "color": "black" }
'
```

In the params, the first object specifies the index and document and document id. The second object specifies the document value.

Even if adding few of them resulted into an error, elastic would process the remaining. The error details would be returned as part of the response.

We could add index and document name in the URL:

```
curl -XPOST "http://localhost:9200/products/mobiles/_bulk?pretty" -d '
{ "index" : { "_id": "6"}}
{ "name": "N73", "text": "it had an headphone jack", "color": "black" }
{ "index" : { "_id": "7"}}
{ "name": "N75", "text": "only for people with big pockets", "color": "black" }
'
```

Or if you dont want to specify id as well, it will be generated for you:

```
curl -XPOST "http://localhost:9200/products/mobiles/_bulk?pretty" -d '
{ "index" : { }}
{ "name": "N90", "text": "i would have bought this", "color": "black" }
{ "index" : { }}
{ "name": "N95", "text": "if only this was created", "color": "black" }
'
```

#### Multiple operations in one command

Instead of just performing one operation, we can use the bulk api to perform combination of CRUD operation within a single command:

```
curl -XPOST "http://localhost:9200/products/mobiles/_bulk?pretty" -d '
{ "index" : { }}
{ "name": "N90", "text": "i would have bought this", "color": "black" }
{ "delete" : { "_id": "2" }}
{ "create" : { "_id": "9" } }
{ "name": "N95", "text": "if only this was created", "color": "black" }
{ "update" : { "_id": "3" }}
{ "doc" : { "color": "white" }}
'
```

### BULK indexing of documents

You can supply a json file to add more document to add to an index:

> curl -H "Content-Type: application/x-ndjson" -XPOST 'localhost:9200/customers/personal/\_bulk?pretty&refersh' data-binary @"customers.json"

Where,

- you specify the type of the file you are reading.
- you specify the index and document name even if they dont exist.
- and specify the file name.

## Executing Search Requests

Before a search is performed, a document is indexed. Relevance score is calculated for the terms indexed.

Document is indexed by a web crawler, for which a inverted index is created for the terms contained in the document.

Post a search, documents are returned based on the relevance.

### Query DSL

Flexible search language that exposes power of Lucene through simple JSON interface.

- `How well` does the document match the query. This is a score based determination.
- `Does` this document match the query. This is a boolean evaluation.

### Setting up fake data

> curl -H "Content-Type: application/x-ndjson" -XPOST 'localhost:9200/customers/personal/\_bulk?pretty&refresh' --data-binary @"customers_full.json"

### Types of searches

- Query Context
- Filter Context

#### Query based

> curl -XGET 'localhost:9200/customers/\_search?q=wyoming&pretty'

This returns all the documents within `customers` index matching the query term. It includes the index detail, document type and relevance score. Documents returned are ordered based on high relevance to low.

You could also query all the indexes without specifying a particular index:

> curl -XGET 'localhost:9200/\_search?q=wyoming&pretty'

You could specify sorting(`sort`) in the query params:

> curl -XGET 'localhost:9200/customers/\_search?q=wyoming&pretty&sort=age:desc'

YOu could fetch results using limit(`from`) and offset(`size`):

> curl -XGET 'localhost:9200/customers/\_search?q=wyoming&pretty&from=10&size=5'

You could dig down more on how relevance score was calculated for a particular document using `explain`:

> curl -XGET 'localhost:9200/customers/\_search?q=wyoming&pretty&explain'

#### Request body based

This is the standard way of writing and maintaining queries. Query provides a subset of options that are available through request body.

> curl -XGET 'localhost:9200/customers/\_search?pretty' -d '{"query":{"match_all": {} }, "sort": {age: {order: desc}}, "size": 5 }'

To perform **exact** matches using `term`:

> curl -XGET 'localhost:9200/customers/\_search?pretty' -d '{"query":{"term":{"street":"chestnut"}}}'

To make the results even shorter:

> curl -XGET 'localhost:9200/customers/\_search?pretty' -d '{"\_source":"false","query":{"term":{"street":"chestnut"}}}'

We can include the source fields matching a regex. Following returns any field containing `st` in the attribute name:

> curl -XGET 'localhost:9200/customers/\_search?pretty' -d '{"\_source":["st*"],"query":{"term":{"street":"chestnut"}}}'

#### Full text searches

To perform **full text** searches we use `match` instead of `term`:

    curl -XGET 'localhost:9200/customers/_search?pretty' -d '
    {
        "query": {
            "match": {
                "name": {
                    "query": "frank norris",
                    "operator": "or"
                }
            }
        }
    }

`"operator": "or"` which is the default for full text searches means that it matches any document containing one or the other query term words.

To **match the entire phrase** we use `match_phrase`:

     curl -XGET 'localhost:9200/customers/_search?pretty' -d '
    {
        "query": {
            "match_phrase": {
                "name": {
                    "query": "frank norris"
                }
            }
        }
    }

To **match phrases beginning with** we use `match_phrase_prefix`. _This can be used for autocomplete functionality_:

     curl -XGET 'localhost:9200/customers/_search?pretty' -d '
    {
        "query": {
            "match_phrase": {
                "name": {
                    "query": "fr"
                }
            }
        }
    }

### Relevance calculations

Higher the value of `_score` higher the relevance. Each document has a different relevance score based on the query, which is calculated `on the fly` by elastic search.

`Fuzzy searches` look at how similar the search term is to the word present in the document.

`Term searches` look at the percentage of search terms that were found in the document.

Elastic search uses `TF/IDF` Term Frequency/Inverse Document Frequency to determine relevance.

- Term frequency refers to how often does the term appear in the field. Eg. a field containing 4 mentions of a term is more relavant than one which has just one mention.
- Inverse document frequency determines how often does the term appear in the entire index of documents. Here the relevance is opposite of term frequency. `More often, less relevant`. If a term is common across documents, eg the, this, its relevance is considered low.
- Field length norm refers to using the length of the field that was searched. `Title of book is more relavant than the content`. So, if a term appears in a longer set, its less relevant than one occuring in smallar set.

The TF/IDF score can be combined with other factors based on the query.

#### Commen Term Problem

Consider a search term `the quick brown fox`. The word `the` is likely to match a lot of documents, with low relevance to the actual search. So, words like `the` is considered as stopwords.

But skipping stopwords might impact the search. Consider the search terms `great` and `not great`.

To overcome this, elastic search would **split the query into low and high frequency terms**. Low frequency term would be `quick brown fox` and high frequency would be `the`.

- Elastic now performs **search first for low frequency term**.
- **Documents returned would then be used to perform search** for high frequency term.
- This kind of search has improved relevance and performs better.

Elastic search allow us to configure `cutoff_frequency` above which a term is **considered as common term**.

Following query indicates that common terms are to be ignored by providing the query under `common`. And it specifies `cutoff_frequency` as 0.001 which means any term matching more that 0.1% should be considered as common term.

     curl -XGET 'localhost:9200/products/_search?pretty' -d '
    {
        "query": {
            "common": {
                "reviews": {
                    "query": "this is great",
                    "cutoff_frequency": 0.001
                }
            }
        }
    }

### Implement queries using REST api
