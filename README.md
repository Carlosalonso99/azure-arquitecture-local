# Arquitectura Local para Análisis y Modelado de Datos con Docker

Este proyecto tiene como objetivo replicar la arquitectura básica de Azure para trabajos de análisis y modelado de datos en un entorno local, utilizando contenedores Docker. La infraestructura incluye servicios para ingesta de datos, almacenamiento, procesamiento, modelado y visualización, así como herramientas para la orquestación de flujos de trabajo.

## Tecnologías Utilizadas
Este proyecto utiliza los siguientes contenedores:

- **Kafka (Apache Kafka)**: Para la ingesta de datos en tiempo real, simulando a Azure Event Hubs.
- **Zookeeper (Apache Zookeeper)**: Servicio de coordinación para Apache Kafka.
- **MinIO**: Para el almacenamiento de objetos, actuando de manera similar a Azure Blob Storage/Data Lake.
- **PostgreSQL** y **MongoDB**: Para el almacenamiento de datos relacionales y NoSQL, similar a Azure SQL Database y Cosmos DB.
- **Apache Spark**: Para el procesamiento y transformación de datos, similar a Azure Databricks.
- **Jupyter Notebook**: Para la exploración de datos interactiva.
- **Metabase**: Para la visualización de datos y creación de dashboards, similar a Power BI.
- **Apache Airflow**: Para la orquestación de flujos de trabajo, similar a Azure Data Factory.

## Requisitos Previos

Antes de comenzar, asegúrate de tener instalados:

- **Docker**: [Instrucciones de instalación](https://docs.docker.com/get-docker/)
- **Docker Compose**: [Instrucciones de instalación](https://docs.docker.com/compose/install/)

## Pasos para Ejecutar el Proyecto

1. **Clonar el Repositorio**

   Clona este repositorio en tu máquina local:

   ```sh
   git clone <url-del-repositorio>
   cd <nombre-del-directorio>
   ```

2. **Crear el Directorio de DAGs para Airflow**

   Crea un directorio llamado `dags` para almacenar los DAGs de Apache Airflow:

   ```sh
   mkdir dags
   ```

3. **Iniciar los Servicios**

   Ejecuta el siguiente comando para iniciar todos los contenedores en segundo plano:

   ```sh
   docker-compose up -d
   ```

4. **Verificar el Estado de los Contenedores**

   Verifica si todos los contenedores se están ejecutando correctamente:

   ```sh
   docker-compose ps
   ```

## Acceder a los Servicios

- **Kafka**: Puedes interactuar con Kafka mediante la CLI (requerirá instalación manual). Asegúrate de que el servicio Zookeeper esté funcionando y Kafka esté conectado a él correctamente.
- **MinIO**: Accede a [http://localhost:9000](http://localhost:9000) con el usuario `minioadmin` y contraseña `minioadmin`.
- **PostgreSQL**: Conéctate a la base de datos en `localhost:5432` usando `user` y `password`.
- **MongoDB**: Accede a `localhost:27017` mediante una herramienta como MongoDB Compass.
- **Apache Spark**: Accede a la interfaz web de Spark en [http://localhost:8080](http://localhost:8080).
- **Jupyter Notebook**: Abre [http://localhost:8888](http://localhost:8888) en tu navegador.
- **Metabase**: Accede a [http://localhost:3000](http://localhost:3000) para crear dashboards.
- **Airflow**: Accede a la interfaz web de Airflow en [http://localhost:8085](http://localhost:8085).

## Procesos de Comprobación de Servicios

A continuación, se describen los pasos para verificar que cada servicio esté funcionando correctamente:

### Verificar Kafka
1. **Crear un Tema**: Accede al contenedor de Kafka:
   ```sh
   docker exec -it <nombre_contenedor_kafka> bash
   ```
   Crea un tema de prueba:
   ```sh
   kafka-topics --create --topic prueba --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
   ```
   Lista los temas para confirmar:
   ```sh
   kafka-topics --list --bootstrap-server localhost:9092
   ```

### Verificar MinIO
1. **Acceder a la Interfaz Web**: Abre [http://localhost:9000](http://localhost:9000).
2. **Crear un Bucket**: Crea un bucket llamado `prueba` para confirmar que MinIO está funcionando correctamente.

### Verificar PostgreSQL
1. **Acceder al Contenedor**:
   ```sh
   docker exec -it <nombre_contenedor_postgres> bash
   ```
2. **Conectarse a PostgreSQL**:
   ```sh
   psql -U user
   ```
3. **Crear una Base de Datos**:
   ```sql
   CREATE DATABASE prueba_pgadmin;
   ```
   Verifica en pgAdmin si la base de datos aparece.

### Verificar MongoDB
1. **Acceder al Contenedor**:
   ```sh
   docker exec -it <nombre_contenedor_mongodb> bash
   ```
2. **Conectar al Cliente Mongo**:
   ```sh
   mongo
   ```
3. **Crear una Base de Datos y Colección**:
   ```javascript
   use prueba;
   db.ejemplos.insert({nombre: "ejemplo"});
   db.ejemplos.find();
   ```

### Verificar Apache Spark
1. **Acceder al Contenedor**:
   ```sh
   docker exec -it <nombre_contenedor_spark> bash
   ```
2. **Ejecutar el Ejemplo SparkPi**:
   ```sh
   spark-submit --class org.apache.spark.examples.SparkPi --master local examples/jars/spark-examples_2.12-3.5.3.jar 10
   ```

### Verificar Airflow
1. **Crear Usuario Admin**: Accede al contenedor de Airflow:
   ```sh
   docker exec -it <nombre_contenedor_airflow> bash
   ```
2. **Crear Usuario**:
   ```sh
   airflow users create --username admin --firstname Admin --lastname User --role Admin --email admin@example.com --password admin
   ```
3. **Acceder a la Interfaz Web**: Inicia sesión en [http://localhost:8085](http://localhost:8085) usando `admin` / `admin`.

## Detener los Servicios

Para detener todos los contenedores, ejecuta:

```sh
docker-compose down
```

Este comando detendrá y eliminará los contenedores, pero preservará los datos de los volúmenes.

## Estructura del Proyecto

- **docker-compose.yml**: Archivo principal que define todos los servicios necesarios para la arquitectura.
- **dags/**: Directorio para almacenar los DAGs de Apache Airflow.

## Tips Adicionales

- Puedes editar el archivo `docker-compose.yml` para realizar ajustes en la configuración de los servicios.
- Utiliza `docker logs <nombre_contenedor>` para ver los registros de cada contenedor y depurar problemas.

## Contribuir
Si deseas contribuir a este proyecto, siéntete libre de abrir un **pull request** o crear un **issue** para cualquier sugerencia o mejora.

## Licencia
Este proyecto está bajo la licencia MIT. Consulta el archivo `LICENSE` para más detalles.
