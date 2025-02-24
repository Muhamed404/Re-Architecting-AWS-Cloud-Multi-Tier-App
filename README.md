# Java Web Application - AWS Architecture

![AWS Architecture Diagram](architecture-diagram.png)

## Overview
This repository contains a Java web application deployed on AWS using Elastic Beanstalk with **Apache Tomcat** as the web application server. The architecture ensures scalability, security, and high availability by leveraging various AWS services.

## AWS Architecture Components

### 1. **User Access**
- Users access the application via a domain managed by **Amazon Route 53**.
- Requests are routed through **Amazon CloudFront**, which provides caching and enhances performance.

### 2. **Load Balancing & Auto Scaling**
- The application is hosted on **AWS Elastic Beanstalk**, utilizing an **Application Load Balancer (ALB)** to distribute traffic.
- The **Auto Scaling Group** ensures dynamic scaling of application instances based on traffic demand.
- The web application runs on **Apache Tomcat**, which is managed by AWS Elastic Beanstalk.

### 3. **Security & Access Control**
- **Beanstalk Load Balancer Security Group (SG)**: Controls access to the load balancer.
- **Beanstalk Instances Security Group (SG)**: Restricts access to EC2 instances running the application.
- **Backend Security Group**: Protects backend services such as databases and caching layers.
- **IAM Role**: Grants permissions for accessing AWS services like S3 and CloudWatch.

### 4. **Backend Services**
- **Amazon RDS (MySQL)**: Manages the relational database for the application.
- **Amazon ElastiCache (Memcached)**: Provides in-memory caching to improve performance.
- **Amazon MQ (RabbitMQ)**: Handles message queuing for asynchronous communication between services.

### 5. **Monitoring & Storage**
- **Amazon CloudWatch**: Monitors application health and performance.
- **S3 Bucket (Artifacts)**: Stores application artifacts and deployment packages.

## Deployment Process
The application is deployed using AWS Elastic Beanstalk. Follow these steps for deployment:

### Prerequisites
Ensure the following tools are installed and configured:
- **Maven** ([Installation Guide](https://maven.apache.org/install.html))

### Steps to Deploy
1. **Update Application Properties**
   Before packaging the application, update the necessary endpoints in the `application.properties` file under `src/main/resources`.
   ```sh
   vi src/main/resources/application.properties
   ```
   Ensure the correct values for:
   - Database URL (`spring.datasource.url`)
   - Message queue (`rabbitmq.host`)
   - Cache server (`cache.host`)

2. **Package the Application**
   ```sh
   mvn clean package
   ```

3. **Deploy the Application**
   - Upload the generated WAR file manually to AWS Elastic Beanstalk using the AWS Management Console.
   - Configure the environment settings as required in AWS Elastic Beanstalk, ensuring the Tomcat runtime is selected.

## Configuration Management
Application-specific configurations, such as database credentials and caching settings, are managed using AWS Parameter Store or AWS Secrets Manager instead of environment variables. Ensure your application fetches configurations securely.

## Monitoring & Logging
- **AWS CloudWatch Logs**: Stores and analyzes application logs.
- **Elastic Beanstalk Health Monitoring**: Provides real-time application health status.
- **Amazon RDS Performance Insights**: Monitors and optimizes database performance.

## Conclusion
This document outlines the deployment architecture and best practices for managing a Java web application on AWS with Apache Tomcat. Ensure compliance with security and performance best practices for a robust and scalable application deployment.

