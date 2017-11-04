# Apache Zeppelin and Dremio tests

## Installation

* For Apache Zeppelin, see https://zeppelin.apache.org/
* For Dremio, see https://www.dremio.com/
* For MongoDB, see https://www.mongodb.com/
* For Elasticsearch, see https://www.elastic.co/fr/products/elasticsearch


## Elasticsearch

Import data in Elasticsearch: see https://www.dremio.com/tutorials/unlocking-sql-on-elasticsearch/


## Dremio configuration

* Open http://localhost:9047/
* Add a new source
	* Select ElasticSearch
	* Set a name, the host, the port, ...


## Apache Zeppelin

In Zeppelin, we are going to use the JDBC interpreter. As the Dremio jar is not in a public Maven repository, we are going to install it in the local repo.

### Install JDBC driver

* Either install jar from Dremio installation directory (DREMIO_HOME: /opt/dremio)

```bash
cd <DREMIO_HOME>/jars/jdbc-driver/
mvn install:install-file -Dfile=dremio-jdbc-driver-1.2.1-201710030121530889-8e49316.jar -DgroupId=com.dremio -DartifactId=jdbc-driver -Dversion=1.2.1 -Dpackaging=jar
```

* Or build the jar from the source code:

```bash
git clone https://github.com/dremio/dremio-oss.git
cd dremio-oss
mvn clean install -DskipTests
```

> You have to install protobuffer-compiler version 2.5.0 !!
> Donwload source (see https://github.com/google/protobuf/releases?after=v3.0.0-alpha-4.1) and compile/install it


### Configure interpreter

* Open http://localhost:8080/#/
* Add a new interpreter with the following properties:
	* default.driver: com.dremio.jdbc.Driver
	* default.url: jdbc:dremio:direct=localhost:31010
	* default.username: dremio
	* default.password: *************
	* Dependencies:
		* com.dremio.client:dremio-client-jdbc:1.2.2-201710100154510864-d40e31c

> Version depends on your local Dremio


## MongoDB

> Does not work at the moment

In these tests, I use a dataset from (DataSF)[https://datasf.org/opendata/]: (Police Department Incidents 2017)[https://data.sfgov.org/Public-Safety/Police-Department-Incidents-Current-Year-2017-/9v2m-8wqu]

Import the csv in MongoDB:

```bash
mongoimport -h localhost:27017  -d datasf -c police_dep_incidents --type csv --headerline Police_Department_Incidents_-_Current_Year__2017_.csv

2017-11-04T17:58:42.492+0100	connected to: localhost:27017
2017-11-04T17:58:44.791+0100	imported 122544 documents
```
