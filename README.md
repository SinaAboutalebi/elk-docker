# ELK Stack Setup with Docker Compose 🚀

This repository contains the setup files for running a **single-node Elastic Stack (Elasticsearch, Kibana, Logstash)** with Docker Compose. This is designed for development and testing purposes. The stack runs as a **single-node Elasticsearch cluster** and includes Kibana for visualization and Logstash for log ingestion.

## Requirements 📋

- Docker 🐳
- Docker Compose ⚙️
  
## Architecture: Single-Node Cluster 🏙️

This setup uses **Elasticsearch in a single-node cluster** configuration. It’s perfect for local development or testing environments, but it’s not recommended for production.

- **Elasticsearch**  runs as a single node.
- **Kibana** connects to Elasticsearch through the internal Docker network.
- **Logstash** is set up to forward logs to Elasticsearch.
Since it's a single-node cluster, all services run on the same machine and communicate via Docker’s internal network. 🌐

## Security Considerations: Reverse Proxy 🔒

By default, the **Elasticsearch** and **Kibana** ports are exposed directly to the host machine. However, in a production environment, it's recommended to use a **reverse proxy (e.g., NGINX or Traefik)** to hide the actual ports and securely route traffic to the services. This will help you avoid exposing sensitive services directly to the public internet. 🌍

Feel free to set up a reverse proxy to handle requests and provide an additional layer of security for your ELK stack! 🔐

## Configuration: `.env` File 📄

Before running the Docker Compose setup, make sure to configure the `.env` file. This file contains essential environment variables for the setup, such as passwords, port mappings, memory limits, and other settings.

### Required Variables in `.env`

Ensure that the following variables are defined in your `.env` file:

```
# Project Name
COMPOSE_PROJECT_NAME=elk-stack

# Elastic Stack Version
STACK_VERSION=8.16.1

# Set your Cluster Name
CLUSTER_NAME=elk-singleNode

# 'elastic' user's password 
ELASTIC_PASSWORD=changeme

# 'kibana_system' user's password
KIBANA_PASSWORD=changeme

# Port to expose Elasticsearch API
ES_PORT=9200

# Port to expose Kibana
KIBANA_PORT=5601

# Set memory limits based on the host (in bytes)
ES_MEM_LIMIT=2147483648 # 2GB
KB_MEM_LIMIT=2147483648 # 2GB
LS_MEM_LIMIT=1073741824 # 1GB

# Predefined Key for Encryption (for POC environments)
ENCRYPTION_KEY=2eaf03a40b85cee6265e34f871073155174b0305e0712ff12165941a3b83d43c

# License type
LICENSE=basic
```

You can modify the values as necessary. Make sure to set strong passwords for `ELASTIC_PASSWORD` and `KIBANA_PASSWORD`. 🔑

## Steps to Set Up the ELK Stack ⚙️

**1. Clone the Repository 🖥️**

```
git clone https://github.com/SinaAboutalebi/elk-docker
cd elk-docker
```

**2. Build and Start Containers with Docker Compose 🏗️**
Run the following command to start up the services:

```
docker-compose up -d
```

This will pull the required Docker images and start the Elasticsearch, Kibana, Logstash, and Setup services in detached mode. The `setup` service will generate the required certificates for Elasticsearch and Kibana. 🔄

**3. Wait for Services to Initialize ⏳**
The `setup` service will run first to generate the necessary certificates. It will wait until Elasticsearch is available before creating the SSL certificates and setting passwords. Once all services are set up, the containers will be ready. ✅

- **Elasticsearch** will be available on `https://localhost:${ES_PORT}`.
- **Kibana** will be available on `http://localhost:${KIBANA_PORT}`.
**4. Access the Services 🌐**
- **Elasticsearch API**: `https://localhost:${ES_PORT}`
- **Kibana UI:** `http://localhost:${KIBANA_PORT}`
You can access Kibana by navigating to `http://localhost:5601` in your browser. Use the `elastic` user and the password defined in the `.env` file for authentication. 🔑

**5. Logstash Configuration 🔄**
Logstash is set up to listen on port 5044 for incoming logs. Ensure your log shipping tool is configured to forward logs to `http://localhost:5044`. 📥

**6. Stopping the Services 🛑**
To stop the running services, use the following command:

```
docker-compose down
```

This will stop and remove all running containers while keeping the generated volumes intact. 🧹

**7. Checking Logs 📝**
To view the logs of a specific service, use:

```
docker-compose logs <service_name>
```

For example, to check the Elasticsearch logs:

```
docker-compose logs elasticsearch
```

---

This setup is suitable for a single-node development cluster. In a production scenario, you may want to scale the Elasticsearch cluster, ensure high availability, and set up proper security with a reverse proxy and SSL certificates. 🔐

Let me know if you need further modifications or additional steps! ✨
