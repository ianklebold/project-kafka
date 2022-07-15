# APACHE KAFKA

## Brokers

Un servidor de Kafka es un Broker y un closter es un conjunto de Brokers. 

![image](https://user-images.githubusercontent.com/56406481/179126430-b6d1ab84-073f-43dd-8a6b-2d6193e6b933.png)

Para correr estos brokers se utiliza Zookeper. 

![image](https://user-images.githubusercontent.com/56406481/179128015-b7c397be-b5a7-471b-a112-441b5bf5ca4c.png)

El puerto 2181 es por defecto tanto para Zookeper como Kafaka. 

```
Para iniciar kafka : sh bin/kafka-server-start.sh config/server.properties
Para iniciar Zookeper : sh bin/zookeeper-server-start.sh config/zookeeper.properties

```

## Topics

Es un flujo de mensajes, cada topic se compone de mas de una particion y cada mensaje se colocan estas particiones y son ordenados por un offset


![image](https://user-images.githubusercontent.com/56406481/179128640-902374bb-9494-432a-a86e-e46a0031c4a6.png)

Un topic de kafka es donde pongo mis mensajes, compuestas por "n" particiones, los mensajes entran a estas particiones donde cada mensaje tiene un identificador denominada offset. 

### Creando un TOPIC

```
sh bin/kafka-topics.sh --bootstrap-server localhost:9092 --create topic ian-topic -partitions 5 --replication-factor 1

Con : "sh bin/kafka-topics.sh --bootstrap-server localhost:9092" Decimos que queremos crear un topic en el 9092 justo donde tenemos el broker de kafka
Con : "--create topic ian-topic -partitions 5 --replication-factor 1" Decimos que queremos crear un topic de 5 particiones un 1 replicacion

```
![image](https://user-images.githubusercontent.com/56406481/179129735-c2ed376e-e42e-4884-816d-e09422f32f9c.png)

### Listando TOPIC

```

sh bin/kafka-topics.sh --list --bootstrap-server localhost:9092 

```
![image](https://user-images.githubusercontent.com/56406481/179129991-c0ae3d0d-6dd1-4677-a81f-20dabbbdc7d3.png)
