---
theme : "sky"
transition: "slide"
highlightTheme: "monokai"
mouseWheel: "true"
logoImg: "img/logo-strapdata.png"
slideNumber: false
title: "Elassandra"
# parallaxBackgroundImage: ''
#
---

# Elassandra
#### Micro-Services Database
###### BBTex 12/03/2020

---

## $ Whoami

* Eric Leleu
* Dev @ Strapdata
* @leleueri

---

## Agenda

* Data Requirements
* Data Replication
* ElasticSearch
* Apache Cassandra
* Elassandra
* Demo

---

## Data Requirements

* Availability
* Scalability
* Searchability
* Analyze & Processing

---

<img  style="border: 0;" data-src="./img/data-everywhere.jpg" />

--

## Models

* Active / Passive Replication
  * Single node accepts writes (Master)
  * Replicas (Slaves) updated by the Master 
* Active / Active Replication
  * All replicas accept writes

--

## Active/Passive

<table>
    <tr>
        <td scope="col"><img  style="border: 0;" data-src="./img/masterslave1.png" /></td>
        <td scope="col"><small>
            <ul>
                <li>Synchronous Replication:
                    <ul>
                        <li>No Writes Availabbility</li>
                    </ul>
                </li>   
                <li>Asynchronous Replication:
                    <ul>
                        <li>Slave may have stale data</li>
                    </ul>
                </li> 
                <li>Manual Failover:
                    <ul>
                        <li>Writes impossible for a while</li>
                    </ul>
                </li> 
                <li>Automatic Failover:
                    <ul>
                        <li>Split Brain may occur (two masters)</li>
                    </ul>
                </li> 
            </ul></small>
        </td>
    </tr>
</table>

--

## Active/Active

<table>
    <tr>
        <td scope="col"><img  style="border: 0;" data-src="./img/masterless.png" /></td>
        <td scope="col"><small>
            <ul>
                <li>R/W High Availability (No SPOF)</li>
                <li>App developers responsible of consistency</li>
                <li>Anti-entropy/read repair</li>
                <li>Conflicts : Last Write Win</li>
            </ul></small>
        </td>
    </tr>
</table>

---

## ElasticSearch

<img  style="border: 0;" data-src="./img/elasticsearch_w3.png" />

<!-- https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-replication.html -->

--

## ElasticSearch

* FullText Search
* Spatial Search 
* Real-time Aggregation  
* No MultiDC replication in OSS 

<!-- https://www.elastic.co/fr/blog/follow-the-leader-an-introduction-to-cross-cluster-replication-in-elasticsearch -->

--

## ElasticSearch: Summary

| Requirements         |     |
|----------------------|-----|
| Availability         |  ++ |
| Scalability          |  +  |
| Multi DC             |  -  |
| Searchability        | +++ |
| Analyze & Processing | +++ |

---

## Apache Cassandra

<img  style="border: 0;" data-src="./img/cassandra_multidc_w2.png" />

--

## Apache Cassandra

* Distributed Hash Table
* Query with partition key
* Secondary indices are limited
* Aggregation possible but limited

--

## Apache Cassandra: Summary

| Requirements         |     |
|----------------------|-----|
| Availability         | +++ |
| Scalability          | +++ |
| Multi DC             | +++ |
| Searchability        |  +  |
| Analyze & Processing |  +  |

---

## Elassandra

<img  style="border: 0;" data-src="./img/DBZ_fusion.gif" />

--

## Elassandra

* Elastic & Cassandra tightly coupled
  * Single Cluster to manage
  * No sync plumbing
  * No duplicate data
* Real-time search & Analytics
* Effortless scale
* High-Avalability & Multi-DC
* Cloud-agnostic or On-Premise

--

<img style="border: 0;" height="200%" width="200%" data-src="./img/elassandra1.jpg" />

--

## Write Path

<img  style="border: 0;" data-src="./img/Elassandra-WP.png" />

--

## Read Path

<img  style="border: 0;" data-src="./img/Elassandra-RP.png" />

--

## Bi-Directionnal Mapping

* Adapt CQL Schema according to Document Fields
* Discover ElasticSearch mapping base on CQL Schema

--

## Nested Document

* Nested documents are stored in a Cassandra User Defined Type
* UDT dynamically generated from the Elasticsearch mapping.

--

## CQL Extension

ElasticSearch queries though CQL

```markdown
cassandra@cqlsh> SELECT * FROM twitter.tweet WHERE 
                          es_query='{ "query": 
                             {"query_string": {"query":"bar2*"}}
                          }';
```

--

## Multiple Indices

* On single Keyspace
* On single table
  * Partition Indices
  * Virtual Indices

--

## Partitionned Indices

<img  style="border: 0;" data-src="./img/partition_function.png" />

--

## Virtual Indices

<img  style="border: 0;" data-src="./img/virtual_index.png" />

--

## Elassandra : Summary

| Requirements         |     |
|----------------------|-----|
| Availability         | +++ |
| Scalability          | +++ |
| Multi DC             | +++ |
| Searchability        | +++ |
| Analyze & Processing | +++ |

--

### Ecosystem

<img  style="border: 0;" data-src="./img/eco-system.png" />

--

## Elassandra Enterprise

* ElasticSearch API security
  * SSL
  * Auth & Authz
  * Content-Based Security
* Audit logs
* Join Queries
* JMX Support
* Backup & Restore
* Kubernetes Operator

---

## Demo

---

## Q&A

<small> [@leleueri](https://twitter.com/leleueri) </small>
<br />
<small> eric@strapdata.com </small>

