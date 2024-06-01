# Stock Market Data Streaming using Kafka and AWS

This project demonstrates a real-time stock market data streaming pipeline using Kafka, AWS EC2, S3, Glue, and other AWS services. The architecture is designed to simulate stock market data, stream it via Kafka, store it in S3, catalog it using AWS Glue, and query it with Amazon Athena.

## Architecture Overview

![Architecture Diagram](Architecture.jpg)

## Prerequisites

- AWS Account
- EC2 Instance with necessary permissions
- Apache Kafka
- Python 3.x
- AWS CLI configured
- boto3 library
- Dataset for stock market simulation

## Setup Instructions

### 1. Setting up Kafka on EC2

1. Download and extract Kafka:
    ```sh
    wget https://downloads.apache.org/kafka/3.3.1/kafka_2.12-3.7.0.tgz
    tar -xvf kafka_2.12-3.7.0.tgz
    ```

2. Install Java (if not already installed):
    ```sh
    sudo yum install java-1.8.0-openjdk
    java -version
    ```

3. Start ZooKeeper:
    ```sh
    cd kafka_2.12-3.7.0
    bin/zookeeper-server-start.sh config/zookeeper.properties
    ```

4. Start Kafka server (in a new terminal):
    ```sh
    export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"
    cd kafka_2.12-3.7.0
    bin/kafka-server-start.sh config/server.properties
    ```

5. Configure Kafka to use the public IP of your EC2 instance by editing `server.properties`:
    ```sh
    sudo nano config/server.properties
    # Change ADVERTISED_LISTENERS to the public IP of the EC2 instance
    ```

6. Create a Kafka topic:
    ```sh
    bin/kafka-topics.sh --create --topic demo_testing2 --bootstrap-server {Public_IP_of_EC2_Instance:9092} --replication-factor 1 --partitions 1
    ```

7. Start a Kafka producer:
    ```sh
    bin/kafka-console-producer.sh --topic demo_testing2 --bootstrap-server {Public_IP_of_EC2_Instance:9092}
    ```

8. Start a Kafka consumer (in a new terminal):
    ```sh
    bin/kafka-console-consumer.sh --topic demo_testing2 --bootstrap-server {Public_IP_of_EC2_Instance:9092}
    ```

### 2. Stock Market Data Simulation

Use the provided Python scripts to simulate stock market data and produce messages to the Kafka topic.

- [KafkaProducer.ipynb](KafkaProducer.ipynb): Contains the producer logic for streaming stock market data.
- [KafkaConsumer.ipynb](KafkaConsumer.ipynb): Contains the consumer logic for reading streamed data.

### 3. Storing Data in S3

Configure your AWS S3 bucket and use Boto3 to store the Kafka data:
- Ensure your EC2 instance has the necessary IAM role with S3 permissions.
- Use the Boto3 library in your consumer script to upload data to S3.

### 4. Cataloging Data with AWS Glue

1. Create a Glue Crawler:
    - Set the S3 bucket as the data source.
    - Run the crawler to catalog the data.

2. Use AWS Glue Data Catalog to query and analyze the data with Amazon Athena.

## Running the Project

1. Start the Kafka broker and ZooKeeper as described in the setup instructions.
2. Run the Kafka producer script to simulate and stream stock market data.
3. Run the Kafka consumer script to read the streamed data and upload it to S3.
4. Use AWS Glue and Athena to catalog and query the data.

## Conclusion

This project provides a scalable and efficient pipeline for real-time stock market data streaming and analysis using Kafka and AWS services. The architecture leverages the power of distributed systems and cloud computing to handle large volumes of data with ease.

## Resources

- [Kafka Documentation](https://kafka.apache.org/documentation/)
- [AWS Boto3 Documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)
- [AWS Glue Documentation](https://docs.aws.amazon.com/glue/index.html)
- [Amazon Athena Documentation](https://docs.aws.amazon.com/athena/index.html)

## Contact

For any queries, please reach out to Kartik Pandit at kartikpandit712@gmail.com.
