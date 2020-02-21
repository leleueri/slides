* Start containers

```
docker-compose --project-name bbtex -f docker-compose.yml [up -d | start] seed_node
docker-compose --project-name bbtex -f docker-compose.yml [up -d | start] node
docker-compose --project-name bbtex -f docker-compose.yml [up -d | start] kibana
```

* Load Kibana eCommerce example
* Add es_query & es_options :

```
cqlsh> ALTER TABLE kibana_sample_data_ecommerce."_doc" ADD es_query text;
cqlsh> ALTER TABLE kibana_sample_data_ecommerce."_doc" ADD es_options text;
```

* Connect to seed node 
* load aliases & show node status

```
docker exec -it bbtex_seed_node_1 bash
source usr/share/cassandra/aliases.sh
nodetool status
```

* Display Cluster State
  * show cluster info
  * show mapping ==> Nested document
  * show routing table

```
state
```

* CQLSH - show UDT product 

```
DESC KEYSPACES
DESC KEYSPACE kibana_sample_data_ecommerce
```

* Show ES Query in CQL (search & stat)

```
select * from kibana_sample_data_ecommerce."_doc" where es_query = '{ "query" : {"match":{"customer_full_name":"underwood"}}}';
select * from kibana_sample_data_ecommerce."_doc" where es_query = '{ "aggs" : {  "total_quantity_stats" : { "stats" : { "field" : "total_quantity" } } }}' and es_options='json' ALLOW FILTERING;
```

* Show Same query thought ES API

```
curl -X POST -H 'Content-Type: application/json' "http://localhost:9200/kibana_sample_data_ecommerce/_search?pretty" -d '{ "query" : {"match":{"customer_full_name":"underwood"}}, "size" : 2}'

curl -X POST -H 'Content-Type: application/json' "http://localhost:9200/kibana_sample_data_ecommerce/_search?pretty" -d '{ "aggs" : {  "total_quantity_stats" : { "stats" : { "field" : "total_quantity" } } }, "size" : 0}'
```

* Show bidirectionnal mapping

```
CREATE KEYSPACE test01 WITH replication = {'class': 'NetworkTopologyStrategy', 'DC1': 2};
CREATE TABLE test01.tweets(userid text, creation_date timestamp, message text, PRIMARY KEY (userid, creation_date));
INSERT into test01.tweets(userid, creation_date, message) values ('me', toUnixTimestamp(now()), 'Hello bbtex');
INSERT into test01.tweets(userid, creation_date, message) values ('me', toUnixTimestamp(now()), 'Hello WL');
INSERT into test01.tweets(userid, creation_date, message) values ('you', toUnixTimestamp(now()), 'Elassandra is awesome');

indices
# pas d'index autre que l'exemple kibana

# create index with autodiscovery excepted for TimeUUID
curl -XPUT -H'Content-Type: application/json' "http://localhost:9200/test01/" -d'{
  "mappings" : {
    "tweets" : {
        "properties" : {
          "creation_date" : {
            "type" : "date",
            "cql_collection" : "singleton",
            "cql_type" : "timestamp",
            "cql_primary_key_order" : 1
          },
          "message" : {
            "type" : "keyword",
            "cql_collection" : "singleton"
          },
          "userid" : {
            "type" : "keyword",
            "cql_collection" : "singleton",
            "cql_partition_key" : true,
            "cql_primary_key_order" : 0
          }
        }
    }
  }
}'

# index créé
indices

# mapping automatique
get  test01?pretty
post test01/tweets '{ "userid" : "bla", "creation_date": "2015-01-01T12:10:30Z", "message":"hhh"}'
post test01/tweets '{ "userid" : "bla", "creation_date": "2015-01-01T12:10:30Z", "message":"hhh", "newfiedl": 456}'
# newfield dans le mapping
get  test01?pretty

# cqlsh
desc KEYSPACE test01;
```