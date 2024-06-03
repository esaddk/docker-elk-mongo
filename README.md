```markdown
# MongoDB Slow Query Log Analysis with Logstash, Elasticsearch, and Kibana

## Overview

This guide explains how to set up a local environment to parse MongoDB slow query logs using Logstash and visualize the data in Kibana.

## Prerequisites

1. **Docker** and **Docker Compose** installed on your machine.
2. Basic knowledge of working with Docker containers.
3. MongoDB logs enabled and slow queries logged.

## Preparation

### 1. Directory Structure

Create a directory structure as follows:

```
mongodb-log-analysis/
├── docker-compose.yml
├── logstash.conf
└── mongod.conf
```

### 2. Docker Compose File
Create a `docker-compose.yml`

### 3. Logstash Configuration
Create a `logstash.conf`

### 4. MongoDB Configuration

Create a `mongod.conf`


## Running the Setup

1. **Start the Services**

Navigate to the `mongodb-log-analysis` directory and run:

```sh
docker-compose up -d
```

This command will start Elasticsearch, Kibana, Logstash, and MongoDB containers.

2. **Verify the Setup**

Check the logs to ensure everything is running correctly:

```sh
docker-compose logs -f
```

## Using Kibana

1. **Access Kibana**

Open Kibana in your web browser by navigating to `http://localhost:5601`.

2. **Create Index Pattern**

- Go to the **Management** section in Kibana.
- Click on **Index Patterns** and create a new index pattern `mongodb-logs-*`.

3. **Discover Data**

- Go to the **Discover** section.
- Select the `mongodb-logs-*` index pattern.
- Use the search bar to filter logs containing slow queries and display `query_time_ms` and `scan_type` fields.

## Visualizing the Data

Create visualizations to analyze query durations and scan types:

### Example: Create a Bar Chart for Query Duration

1. **Go to Visualize**
2. **Click Create Visualization**
3. **Select Vertical Bar**
4. **Choose the existing index pattern (e.g., `mongodb-logs-*`)**
5. **For X-Axis, choose Date Histogram and select `@timestamp`**
6. **For Y-Axis, use Average and select `query_time_ms`**
7. **Add filters to visualize specific scan types or query contexts**

### Example: Create a Pie Chart for Scan Type Distribution

1. **Go to Visualize**
2. **Click Create Visualization**
3. **Select Pie**
4. **Choose the existing index pattern (e.g., `mongodb-logs-*`)**
5. **For Split Slices, choose Terms and select `scan_type`**
6. **Add filters as needed**

By following these steps, you can parse and visualize MongoDB slow query logs, focusing on query duration and scan type, in Kibana.
```
