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

Si yo quisiera 3 replicaciones, no voy a poder porque solo tengo un broker, si quiero 3 replicaciones minimo necesito 3 brokers en mi cluster

```
![image](https://user-images.githubusercontent.com/56406481/179129735-c2ed376e-e42e-4884-816d-e09422f32f9c.png)

### Listando TOPIC

```

sh bin/kafka-topics.sh --list --bootstrap-server localhost:9092 

```
![image](https://user-images.githubusercontent.com/56406481/179129991-c0ae3d0d-6dd1-4677-a81f-20dabbbdc7d3.png)

### Para acceder al detalle del TOPIC

```
sh bin/kafka-topics.sh --describe --topic ian-topic --bootstrap-server localhost:9092

```

![image](https://user-images.githubusercontent.com/56406481/179131815-9478060e-8469-42ec-98fd-e2dbb92cecbc.png)

Con esto es posible conocer cuantas particiones y replicas tenia mi topic

## Particiones y replicas

Multiple particiones permite tener consumidore de mensajes trabajando concurrentemente, mientras más consumidores afectará a la velocidad de envio de mensajes.

Para incrementar la disponibilidad de la informacion los topics se deben replicar en multiples brokers. 

![image](https://user-images.githubusercontent.com/56406481/179130927-6f4764b0-55d9-4c46-a8d9-6bc3b8edd88b.png)

Como vemos aquí tenemos un cluster de tres brokers, lo que nos posibilita a tener como maximo 3 replicas de un topic. En este caso se asignan
2 replicas de un mismo topic, generando un lider y un seguidor, si tengo 3 replicas tendré un lider y dos seguidores, asi sucesivamente. Como vemos la replica del topic es exacta incluso en la cantidad de particiones de estos. 

Se dice que si una replica se encuentra totalmente actualizada, se encuentra en un estado "in-sync", si por alguna razón un lider se cae, deja de estar actualizado, otra replica seguidora podría tomar su lugar de lider. 


## Mensajes

Un mensaje es una unidad de datos en kafka, compuesto por una llave y valor. Un mensaje es generador por un productor y recibido por un consumidor.

![image](https://user-images.githubusercontent.com/56406481/179132069-23c16c2a-3d07-4850-a057-971c77184316.png)


## Productor

Publica mensajes en uno o mas TOPICS y es el que indica en que particion colocar ese mensaje

### Creando un PRODUCTOR

```
sh bin/kafka-console-producer.sh --topic ian-topic --bootstrap-server localhost:9092
```
![image](https://user-images.githubusercontent.com/56406481/179134892-12a23bb9-a8a5-4da8-9a3d-71d14ad21eee.png)

Como vemos nos permite escribir un mensaje. 


## Consumidor

Lee mensajes de uno o más TOPICS y los procesa. El consumidor siempre tendrá una posicion, la posicion es el ultimo mensaje que leyó el consuimdor.
La diferencia entre la posicion del consumidor y el mensaje más nuevo que ingresó en la partición se conoce como offset lag. El offset lag es muy importante porque es una medida que me indica que tan rapido está procesando los mensajes el consumidor. Aquel periodo de tiempo que se mantienen los mensajes sin leer se llama rentention period.

En Kafka los mensajes se almacenan por un periodo de tiempo, si este periodo es igual al offset lag estamos ante un gran problema. 

### Creando un CONSUMIDOR

```
sh bin/kafka-console-consumer.sh --topic ian-topic --bootstrap-server localhost:9092
```

![image](https://user-images.githubusercontent.com/56406481/179135067-9f540c9b-740c-4fdf-90f4-6db087913ef1.png)

El consumidor queda esperando mensajes del productor. 

![image](https://user-images.githubusercontent.com/56406481/179135270-bbb3ad2e-f740-4342-89a4-e0daf186b8a9.png)

Hasta que el productor genera el mensaje y le llega al consumidor

### Configuracion en CONSOLE CONSUMER

Si detengo mi consumidor y mi productor sigue mandando mensajes 
![image](https://user-images.githubusercontent.com/56406481/179135804-391e9af4-a621-489e-ac49-c24a43a0d729.png)

Ahora si creo nuevamente un consumidor y en ese instante envio un mensaje, vamos a ver que el consumidor comienza a leer los mensajes pero a partir de los mensajes que se enviaron desde que él estuvo en linea. 

![image](https://user-images.githubusercontent.com/56406481/179135989-9e45b92b-f537-4755-a391-91c7ad6f54ea.png)

Se perdió de 2 mensajes

```
Solucionamos este problema, con la bandera --from-beginning

sh bin/kafka-console-consumer.sh --topic ian-topic --from-beginning --bootstrap-server  localhost:9092
```
![image](https://user-images.githubusercontent.com/56406481/179136433-2c2af055-868d-4504-abd9-371522dd9918.png)














