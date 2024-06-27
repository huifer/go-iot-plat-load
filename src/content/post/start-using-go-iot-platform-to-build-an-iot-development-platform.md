---
publishDate: 2024-06-27T00:00:00Z
author: Zen HuiFer
title: Start using Go IoT Platform to build an IoT development platform
excerpt: Embark on your journey to the Internet of Things using the Go Iot Platform
image: https://images.unsplash.com/photo-1516996087931-5ae405802f9f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80
category: Tutorials
tags:
  - Go
  - IoT
  - Platform
  - First
  - Tutorial
  - Environmental
  - Deployment
---


## Environmental Configuration

Before using this project, you need to install the following environment:



| Service Name | Version Requirement           | Installation Method | Remarks         |
|--------------|-------------------------------|--------------------|-----------------|
| InfluxDB     | 2.6-alpine                   | Docker              | Time-series database |
| MySQL        | 8.0.33                       | Manual installation  | Relational database |
| MQTT         | emqx:5.4.1                   | Docker              | Messaging protocol broker |
| RabbitMQ     | 3-management-alpine           | Docker              | Message queue service |
| Redis        | 6.2.14                       | Manual installation  | In-memory data structure store |
| Go           | 1.22                         | Manual installation  | Programming language environment |



## Backend Service Deployment


### 1. Compilation

Compile the project to generate an executable file igp.

**Operation Steps:**：

- Navigate to the project directory:：

```
cd iot-go-project
```

- Download dependencies:：

```
go mod tidy
```

- Compile the project:：

```
go build -o igp
```

**Result:** After compilation, an executable file named igp will be generated in the project directory.


### 2. Modify Configuration File

Create or modify the configuration file app-node1.yml to configure the database, message queue, and other information required by the service.

**Configuration Example**：

```
node_info:
  port: 8080
redis_config:
  host: 127.0.0.1
  port: 6379
  db: 0
  password: eYVX7EwVmmxKPCDmwMtyKVge8oLd2t81
mq_config:
  host: 127.0.0.1
  port: 5672
  username: guest
  password: guest
influx_config:
  host: 127.0.0.1
  port: 8086
  token: 111
  org: test
  bucket: test
mysql_config:
  username: root
  password: root123@
  host: 127.0.0.1
  port: 3306
  dbname: iot
```

**Notes:**

- Please modify the parameters in the configuration file according to the actual environment, such as database password, Redis password, etc.
- Ensure that all services (such as Redis, MQTT, MySQL, etc.) are installed and running correctly.

### 3. Start Service

Start the backend service using the following command。

```
./igp -config app-node1.yml
```



## MQTT client management project deployment

### 1. Compilation

Compile the project to generate the executable file 'go iot'.


**Operation Steps:**：

- Navigate to the project directory:：

```
cd go-iot
```

- Download dependencies:：

```
go mod tidy
```

- Compile the project:：

```
go build -o go-iot
```

**Result**: After the Compilation is completed, an executable file named 'go iot' will be generated in the project directory.

### 2. Modify Configuration File

Create or modify the configuration file `app-node1.yml` to set up the Redis and message queue information required for the MQTT client management project.

**Configuration Example**：

```
node_info:
  host: 127.0.0.1
  port: 8081
  name: m1
  type: mqtt
  size: 1000
redis_config:
  host: 127.0.0.1
  port: 6379
  db: 0
  password: eYVX7EwVmmxKPCDmwMtyKVge8oLd2t81

mq_config:
  host: 127.0.0.1
  port: 5672
  username: guest
  password: guest
```



### 3. Start Service

Start the MQTT client management project using the following command:

```
./go-iot -config app-node1.yml
```




## Rabbit Consumer Deployment Steps

### 1. Compilation

Compile the project to generate the executable file 'gim'.


**Operation Steps:**：

- Navigate to the project directory:：

```
cd go-iot-mq
```

- Download dependencies:：

```
go mod tidy
```

- Compile the project:：

```
go build -o gim
```


**Result:** After the Compilation is completed, an executable file named 'gim' will be generated in the project directory.


### 2. Modify Configuration File

Create or modify configuration files to configure Redis, message queues, MongoDB, and InfluxDB information required for MQTT client management projects.

**Configuration Example**：

```
node_info:
  host: 127.0.0.1
  port: 29001
  name: mq1
  type: calc_queue # pre_handler、 waring_handler、 calc_queue、waring_delay_handler
redis_config:
  host: 127.0.0.1
  port: 6379
  db: 10
  password: eYVX7EwVmmxKPCDmwMtyKVge8oLd2t81

mq_config:
  host: 127.0.0.1
  port: 5672
  username: guest
  password: guest
influx_config:
  host: 127.0.0.1
  port: 8086
  token: i6XHSnNXeUoU3GoFXMm4qqrrgt69JKvQLqm0FCtnYG-rjb-nkDcry0pdwv4fpcXsSwi-mTGmAUTygkJtR-6CWA==
  org: myorg
  bucket: buc
mongo_config:
  host: 127.0.0.1
  port: 27017
  username: admin
  password: admin
  db: iot
  collection: calc
  waring_collection: waring
  script_waring_collection: script_waring
```

**Notes**:

- Please modify the parameters in the configuration file according to the actual environment, such as database password, Redis password, etc.
- Ensure that all services (such as Redis, RabbitMQ, InfluxDB, MongoDB, etc.) are installed and running correctly.

### 3. Start Service

Start the MQTT client management project using the following command.


**Start command**：

```
./gim -config app-local-calc.yml
./gim -config app-local-pre_handler.yml
./gim -config app-local-waring_handler.yml
./gim -config app-local-wd.yml
```


##  Precautions

- During the deployment process, ensure that firewall rules allow corresponding port access.
- Regularly backup configuration files and databases to prevent data loss.
- In production environments, it is recommended to use more secure passwords and restrict access to configuration files.
