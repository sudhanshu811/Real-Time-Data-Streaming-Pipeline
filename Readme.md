# **Real-Time Data Streaming Pipeline: API → Kafka → Spark → Cassandra**

## **Introduction**

This project demonstrates an end-to-end real-time data streaming architecture using Python, Kafka, Spark Structured Streaming, and Cassandra.
The pipeline simulates continuous data ingestion from an API, processes the incoming data in real time, and stores it in a NoSQL database for analytical use.

It reflects a production-grade data engineering workflow with modular, fault-tolerant, and scalable components.

## **System Architecture**

[External API] → [Kafka Topic] → [Spark Structured Streaming] → [Cassandra]

## **Components Overview**

| Layer | Technology | Purpose |
|---|---|---|
| Data Source | Python API Script | Generates user data in JSON and sends to Kafka |
| Message Queue | Apache Kafka | Buffers real-time data for downstream processing |
| Stream Processor | Apache Spark Structured Streaming | Reads from Kafka, transforms, and loads to Cassandra |
| Storage | Apache Cassandra | Stores structured, query-ready data |

## **What You’ll Learn**

* Designing a real-time data streaming pipeline
* Integrating Kafka producers with Spark Structured Streaming consumers
* Writing Spark data directly into Cassandra
* Using Airflow for orchestration (optional)
* Running everything locally or via Docker
* Implementing scalable, fault-tolerant data pipelines

## **Technologies Used**

* Python 3.10+
* Apache Kafka
* Apache Spark 3.x (Structured Streaming)
* Apache Cassandra
* Apache Airflow
* Docker & Docker Compose
* pyspark, kafka-python, cassandra-driver

## **Getting Started**

1. **Clone the Repository**
   ```bash
   git clone https://github.com/sudhanshu811/Real-Time-Data-Streaming-Pipeline.git
   cd data_engineering
   
2. Create and Activate a Virtual Environment
    ```bash
    python3 -m venv venv
    source venv/bin/activate

3. Install Dependencies
    ```bash
    pip install -r requirements.txt

4. Pull images defined in docker-compose.yml
    ```bash
    docker-compose pull

This pulls every image referenced in your docker-compose.yml automatically.

5. Start the services
    ```bash
   docker-compose up -d

This will start:
* Zookeeper
* Kafka Broker
* Cassandra Database
* Spark Master & Worker Nodes

6. Set Up Cassandra Keyspace and Table

7. **Open Cassandra shell:**
    ```bash
    docker exec -it cassandra cqlsh
    ```

8. **Run the following CQL commands:**
    
The following commands will create the `spark_streams` keyspace and the `created_users` table.

    ```cql
    CREATE KEYSPACE IF NOT EXISTS spark_streams WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};
    USE spark_streams;
    CREATE TABLE IF NOT EXISTS created_users (
      id UUID PRIMARY KEY,
      first_name TEXT,
      last_name TEXT,
      gender TEXT,
      email TEXT,
      username TEXT,
      phone TEXT,
      address TEXT,
      post_code TEXT,
      registered_date TEXT,
      picture TEXT
    );
    ```

9. Exit the Cassandra shell:
    ```bash
    exit
    ```
10. **Run the API Producer Script**
    This script simulates data generation and sends JSON user records to Kafka.

    ```bash
    python script/kafka_producer.py
    ```

11. **Run the Spark Streaming Job**
    Start Spark to continuously read from Kafka and write to Cassandra:

    ```bash
    python spark_stream.py
    ```

12. **Verify the Data in Cassandra**

    After Spark runs for a while, check the data in Cassandra by executing the following command in your terminal to access the Cassandra shell:

    ```bash
    docker exec -it cassandra cqlsh
    ```

    Once inside the Cassandra shell (`cqlsh`), run the following CQL commands:

    ```cql
    USE spark_streams;
    SELECT * FROM created_users LIMIT 10;
    ```

You should see rows of data, confirming the Spark job is successfully writing to Cassandra.

13. Stop the pipeline

```bash
docker-compose down
```

## **Advantages of This Architecture**

1. **Real-Time Processing:** Immediate transformation of incoming events.
2. **Scalable:** Each component can scale horizontally.
3. **Fault-Tolerant:** Kafka and Spark ensure zero data loss.
4. **Extensible:** Easy integration with BI tools, ML pipelines, or dashboards.
5. **Modular:** Each layer (API, Kafka, Spark, Cassandra) operates independently.

## **Future Enhancements**

1. Integrate Apache Airflow for orchestration.
2. Extend Spark to perform ML-based predictions in real time.
3. Connect Cassandra with BI tools for live dashboards.
4. Add data lake integration (S3, Delta Lake, or HDFS) for historical data storage.

## **Conclusion**

This project demonstrates a modern data engineering pipeline using open-source technologies.
It integrates Kafka, Spark, and Cassandra to achieve a reliable, scalable, and real-time data flow from ingestion to storage.

This architecture is a practical foundation for real-world applications such as:

* Real-time analytics
* IoT event monitoring
* User activity tracking
* Data lake ingestion pipelines