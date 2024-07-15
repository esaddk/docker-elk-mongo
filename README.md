# MongoDB Slow Query Log Analysis with Logstash, Elasticsearch, and Kibana

This guide will walk you through setting up the ELK (Elasticsearch, Logstash, Kibana) stack with Filebeat to monitor and analyze PostgreSQL logs. We will use Docker and Docker Compose for easy setup and management.

## Prerequisites

Before you begin, ensure you have the following installed on your local machine:

- Docker
- Docker Compose

## Directory Structure

Your project directory should have the following structure:

.
├── docker-compose.yml
├── filebeat
│   ├── filebeat.yml
│   └── modules.d
│       └── postgresql.yml
├── logs
└── data


## Step-by-Step Setup

### 1. Set Up Docker Network

Create a Docker network so that the containers can communicate with each other.

```bash
docker network create elk-net
```

### 2. Create Docker Compose File

Create a `docker-compose.yml` file with the following content to define the services for Elasticsearch, Kibana, PostgreSQL, and Filebeat:

### 3. Create Filebeat Configuration

Create a directory called `filebeat` and inside it, create a `filebeat.yml` file with the following content:

### 4. Enable PostgreSQL Module

Create a directory `filebeat/modules.d` and inside it, create a `postgresql.yml` file with the following content to ensure the PostgreSQL module is enabled:

### 5. Create Logs Directory

Ensure the logs directory exists and has the appropriate permissions:

```bash
mkdir -p logs
sudo chown -R 999:999 logs
```

### 6. Start the Services

Bring up the services using Docker Compose:

```bash
docker-compose up -d
```

### 7. Verify PostgreSQL Database

Connect to the PostgreSQL container to verify that the database `mydb` has been created and generate a slow query:

```bash
docker exec -it postgresql psql -U user -d mydb
```

If the above command doesn't work, connect to the default `postgres` database first:

<<<<<<< HEAD
```bash
docker exec -it postgresql psql -U user -d postgres
```

Inside the PostgreSQL shell, create the `mydb` database if it doesn't exist:

```sql
CREATE DATABASE mydb;
\q
```

### 8. Generate Dummy Slow Query

Once connected to `mydb`, generate a slow query to ensure it is logged:

```bash
docker exec -it postgresql psql -U user -d mydb
```

Inside the PostgreSQL shell:

```sql
-- Create a test table with some dummy data
CREATE TABLE test_table (
    id SERIAL PRIMARY KEY,
    data TEXT
);

-- Insert dummy data
INSERT INTO test_table (data)
SELECT generate_series(1, 1000000)::text;

-- Run a slow query
SELECT pg_sleep(2), count(*) FROM test_table;
```

### 9. Load Filebeat Dashboards

Load the default dashboards into Kibana:

```bash
docker exec -it filebeat filebeat setup --index-management -E output.elasticsearch.hosts=["http://elasticsearch:9200"]
docker exec -it filebeat filebeat setup --dashboards
```

### 10. Verify Filebeat Logs

Check the Filebeat logs to ensure it's reading the PostgreSQL logs and sending them to Elasticsearch:

```bash
docker logs filebeat
```

### 11. Analyze Logs in Kibana

1. Open Kibana by navigating to `http://localhost:5601` in your browser.
2. Go to the "Dashboard" section.
3. Look for the PostgreSQL dashboards. They should be named something like "Filebeat PostgreSQL".

### Troubleshooting

If you encounter issues, check the following:

- Ensure PostgreSQL is correctly writing logs to `/var/log/postgresql/postgresql.log`.
- Ensure docker has read permissions on the filebeat.yaml & filebeat directory.
  - sudo chown -R root:root filebeat
  - sudo chmod -R 755 filebeat
- Ensure Filebeat has read permissions on the PostgreSQL logs directory.
- Check the Filebeat logs for any errors.
- Ensure Elasticsearch and Kibana are running and accessible.
- Verify that the `filebeat-*` index pattern is correctly set up in Kibana.

By following these steps, you should be able to set up PostgreSQL slow query logging with Filebeat, Elasticsearch, and Kibana using Docker, and analyze the logs on the Kibana dashboard.
=======
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
>>>>>>> 8de996ed64ec87d43ebed48eb97e0d941d935e1a
