# selenium prerequisites:
1. download a webdriver: \
there's a list of 4 drivers here: https://pypi.org/project/selenium/ \
if you want the latest chromedriver is here: https://googlechromelabs.github.io/chrome-for-testing/#stable \
```bash
wget https://storage.googleapis.com/chrome-for-testing-public/127.0.6533.4/linux64/chromedriver-linux64.zip
```
3. install selenium:
```bash
pip install -U selenium
```
3. install beautifulsoup:
```bash
 pip install beautifulsoup4
```
4. run the scraper files


to list connectors:
```bash
curl -X GET http://connect:8083/connectors
```
to delete a connector:
```bash
curl -X DELETE http://connect:8083/connectors/<name_of_connector>
```
to create from a file:
```bash
curl -X POST -H "Content-Type: application/json" -d @simplesource.json http://connect:8083/connectors -w "\n"
# if you want to pase the json's content directly: \
# replace -d @simplesource.json with --data '<json's_content>'
```
a simple sink connector:
```bash
{
  "name": "mongo-tutorial-sink",
  "config": {
    "connector.class": "com.mongodb.kafka.connect.MongoSinkConnector",
    "topics": "<topic_name_the_connector_will_listen_to>",
    "connection.uri": "mongodb://<host_addr>",
    "key.converter": "org.apache.kafka.connect.storage.StringConverter",
    "value.converter": "org.apache.kafka.connect.json.JsonConverter",
    "value.converter.schemas.enable": false,
    "database": "<db_name>",
    "collection": "<collection_name>"
  }
}
```
a simple source connector:
```bash
{
  "name": "<name_of_connector>",
  "config": {
  "connector.class":"com.mongodb.kafka.connect.MongoSourceConnector",
  "connection.uri":"mongodb://<host_addr>:<port_of_mongo>/?replicaSet=rs0",
  "database":"<db_name>",
  "collection":"<collection_name>",
  "pipeline":"[{\"$match\": {\"operationType\": \"insert\"}}, {$addFields : {\"fullDocument.travel\":\"MongoDB Kafka Connector\"}}]"
  }
}
```
list topics in kafka:
```bash
/bin/kafka-topics --list --bootstrap-server broker:29092
```
delete topic:
```bash
/bin/kafka-topics --delete --bootstrap-server broker:29092 --topic quickstart.sampleData
```