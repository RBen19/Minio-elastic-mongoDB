# 🚀 Stack

[![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)](https://www.docker.com/)
[![MinIO](https://img.shields.io/badge/MinIO-C72E49?style=flat&logo=minio&logoColor=white)](https://min.io/)
[![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat&logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=flat&logo=elasticsearch&logoColor=white)](https://www.elastic.co/)
[![Kibana](https://img.shields.io/badge/Kibana-005571?style=flat&logo=kibana&logoColor=white)](https://www.elastic.co/kibana/)

## 📖 English | 🇫🇷 Français

## 📚 Table of Contents | Table des Matières

### 🇺🇸 English Documentation
- [📋 Overview](#overview)
- [🏗️ Architecture](#architecture) 
- [🐳 Docker Images Used](#docker-images-used)
- [🚀 Quick Start](#quick-start)
- [🔧 Environment Configuration](#environment-configuration)
- [🌐 Web Interfaces Guide](#web-interfaces-guide)
- [🧪 Health Checks](#health-checks)
- [🔒 Production Security Notes](#production-security-notes)

### ☸️ Kubernetes Documentation
- [📋 What is Kubernetes?](#what-is-kubernetes)
- [🎯 Why Use Kubernetes?](#why-use-kubernetes-for-this-stack)
- [🛠️ Prerequisites & Installation](#prerequisites-and-installation)
- [🏗️ Architecture & Structure](#architecture--project-structure)
- [🚀 Deployment Guide](#deployment-guide)
- [🔧 Resource Management](#resource-management--configuration)
- [🌐 Service Access](#service-access--networking)
- [📊 Monitoring & Debugging](#monitoring-and-debugging)
- [🔍 Commands Reference](#useful-commands-reference)
- [🔒 Production Optimization](#production-optimization)

### 🇫🇷 Documentation Française
- [📋 Aperçu](#aperçu)
- [🏗️ Architecture](#architecture-1)
- [🚀 Démarrage Rapide](#démarrage-rapide)
- [🔧 Configuration](#configuration-denvironnement)
- [🌐 Interfaces Web](#guide-des-interfaces-web)
- [🧪 Vérifications](#vérifications-de-santé)
- [🔒 Sécurité Production](#notes-de-sécurité-pour-la-production)

### ☸️ Documentation Kubernetes (FR)
- [📋 Qu'est-ce que Kubernetes ?](#quest-ce-que-kubernetes)
- [🎯 Pourquoi Kubernetes ?](#pourquoi-utiliser-kubernetes-pour-cette-stack)
- [🛠️ Prérequis & Outils](#prérequis-et-outils)
- [🚀 Guide de Déploiement](#démarrage-rapide-avec-minikube)
- [🔧 Gestion des Ressources](#allocation-des-ressources)
- [🌐 Accès aux Services](#accéder-aux-services)
- [📊 Surveillance](#surveillance-et-débogage)
- [🔍 Référence des Commandes](#référence-des-commandes-utiles)
- [🔒 Optimisation Production](#optimisation-pour-la-production)

### 📚 Additional Resources
- [Resources & Links](#additional-resources--ressources-supplémentaires)
- [License](#license--licence)

---

## 🇺🇸 English Documentation

### 📋 Overview

This project provides a complete enterprise-ready storage and analytics stack using Docker Compose. It includes object storage, document database, search engine, and administrative web interfaces.

📋 **Quick Navigation:**
- New to the project? Start with [Quick Start](#quick-start)
- Want to deploy on Kubernetes? See [Kubernetes Documentation](#kubernetes-deployment--déploiement-kubernetes)
- Need production setup? Check [Production Security Notes](#production-security-notes)
- Troubleshooting? See [Health Checks](#health-checks)

Make sure to read the all file before process to use 

### 🏗️ Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│     MinIO       │    │    MongoDB      │    │ Elasticsearch   │
│ Object Storage  │    │   Database      │    │ Search Engine   │
│   Port: 9000    │    │  Port: 27017    │    │  Port: 9200     │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ MinIO Console   │    │  Mongo Express  │    │     Kibana      │
│ Web Interface   │    │ Web Interface   │    │ Web Interface   │
│   Port: 9001    │    │  Port: 8081     │    │  Port: 5601     │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### 🐳 Docker Images Used

| Service | Image | Version | Purpose |
|---------|-------|---------|---------|
| **MinIO** | `minio/minio` | `RELEASE.2024-12-18T13-15-44Z` | S3-compatible object storage |
| **MongoDB** | `mongo` | `7.0` | NoSQL document database |
| **Elasticsearch** | `docker.elastic.co/elasticsearch/elasticsearch` | `8.11.3` | Full-text search engine |
| **Kibana** | `docker.elastic.co/kibana/kibana` | `8.11.3` | Elasticsearch dashboard |
| **Mongo Express** | `mongo-express` | `1.0.2` | MongoDB web admin interface |

### 🚀 Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/RBen19/Minio-elastic-mongoDB.git
   cd Minio-elastic-mongoDB
   ```

2. **Start the services**
   ```bash
   docker-compose up -d
   ```

3. **Verify all services are running**
   ```bash
   docker-compose ps
   ```

4. **Access the web interfaces**
   - MinIO Console: http://localhost:9001
   - Kibana Dashboard: http://localhost:5601
   - Mongo Express: http://localhost:8081

### 🔧 Environment Configuration

The `.env` file contains all configurable parameters:

#### MinIO Configuration
```env
MINIO_ROOT_USER=admin                    # MinIO admin username
MINIO_ROOT_PASSWORD=adminpass123         # MinIO admin password (min 8 chars)
MINIO_API_PORT=9000                      # MinIO API port
MINIO_CONSOLE_PORT=9001                  # MinIO web console port
MINIO_BROWSER_REDIRECT_URL=http://localhost:9001  # Console redirect URL
```

#### MongoDB Configuration
```env
MONGO_INITDB_ROOT_USERNAME=admin         # MongoDB root username
MONGO_INITDB_ROOT_PASSWORD=admin         # MongoDB root password
MONGO_INITDB_DATABASE=storage_project    # Initial database name
MONGODB_PORT=27017                       # MongoDB connection port
```

#### Elasticsearch Configuration
```env
ELASTICSEARCH_HTTP_PORT=9200             # Elasticsearch HTTP API port
ELASTICSEARCH_TRANSPORT_PORT=9300        # Elasticsearch transport port
DISCOVERY_TYPE=single-node               # Cluster discovery type
XPACK_SECURITY_ENABLED=false            # Security features (disabled for dev)
XPACK_ML_ENABLED=false                   # Machine learning features
ES_JAVA_OPTS=-Xms1g -Xmx1g             # JVM heap size settings
NETWORK_HOST=0.0.0.0                    # Network binding
BOOTSTRAP_MEMORY_LOCK=true              # Memory lock for performance
```

#### Kibana Configuration
```env
KIBANA_PORT=5601                         # Kibana web interface port
KIBANA_SERVER_NAME=kibana-server         # Server name
KIBANA_SERVER_HOST=0.0.0.0              # Server binding host
KIBANA_ENCRYPTION_KEY=kibana-encryption-key-32chars-min  # Encryption key
```

#### Mongo Express Configuration
```env
MONGO_EXPRESS_PORT=8081                  # Mongo Express web port
MONGO_EXPRESS_USERNAME=admin             # Web interface username
MONGO_EXPRESS_PASSWORD=adminpass123      # Web interface password
MONGO_EXPRESS_BASEURL=/                  # Base URL path
```

### 🌐 Web Interfaces Guide

#### MinIO Console (Object Storage)
- **URL**: http://localhost:9001
- **Credentials**: admin / adminpass123
- **Features**:
  - Create and manage S3 buckets
  - Upload/download files
  - Set bucket policies and permissions
  - Monitor storage usage
  - User and access key management

#### Kibana (Elasticsearch Dashboard)
- **URL**: http://localhost:5601
- **Authentication**: None (disabled for development)
- **Features**:
  - Create visualizations and dashboards
  - Query Elasticsearch data
  - Monitor cluster health
  - Index pattern management
  - DevTools for direct ES queries

#### Mongo Express (MongoDB Admin)
- **URL**: http://localhost:8081
- **Credentials**: admin / adminpass123
- **Features**:
  - Browse databases and collections
  - View and edit documents
  - Execute MongoDB queries
  - Database administration
  - Import/export data

### 🧪 Health Checks

```bash
# Check all containers status
docker-compose ps

# Test Elasticsearch
curl http://localhost:9200/_cluster/health

# Test MinIO API
curl http://localhost:9000/minio/health/live

# Test MongoDB
docker exec mongodb mongosh --eval "db.adminCommand('ping')"

# Test Kibana
curl http://localhost:5601/api/status

# Test Mongo Express
curl -I http://localhost:8081
```

### 🔒 Production Security Notes

⚠️ **Important**: This configuration is optimized for development. For production deployment:

1. **Change all default passwords** (see [Environment Configuration](#environment-configuration))
2. **Enable TLS/SSL certificates**
3. **Enable Elasticsearch security features**
4. **Configure proper firewall rules**
5. **Use Docker secrets for sensitive data**
6. **Implement backup strategies**
7. **Set up monitoring and logging**

🔗 **Next Steps**: Ready for Kubernetes? See [Kubernetes Documentation](#kubernetes-deployment--déploiement-kubernetes)

---

## 🇫🇷 Documentation Française

### 📋 Aperçu

Ce projet fournit une stack complète de stockage et d'analyse prête pour l'entreprise utilisant Docker Compose. Il inclut le stockage d'objets, une base de données documentaire, un moteur de recherche et des interfaces web d'administration.

📋 **Navigation Rapide :**
- Nouveau sur le projet ? Commencez par [Démarrage Rapide](#démarrage-rapide)
- Vous voulez déployer sur Kubernetes ? Voir [Documentation Kubernetes](#kubernetes-deployment--déploiement-kubernetes)
- Besoin d'une configuration de production ? Consultez [Notes de Sécurité](#notes-de-sécurité-pour-la-production)
- Dépannage ? Voir [Vérifications de Santé](#vérifications-de-santé)

Assurez vous de lire toute la documentation avant d'utiliser 

### 🏗️ Architecture

La stack est composée de 5 services interconnectés :
- **MinIO** : Stockage d'objets compatible S3
- **MongoDB** : Base de données NoSQL documentaire
- **Elasticsearch** : Moteur de recherche full-text
- **Kibana** : Interface de visualisation pour Elasticsearch
- **Mongo Express** : Interface web d'administration pour MongoDB

### 🚀 Démarrage Rapide

1. **Cloner le dépôt**
   ```bash
   git clone https://github.com/RBen19/Minio-elastic-mongoDB.git
   cd Minio-elastic-mongoDB
   ```

2. **Démarrer les services**
   ```bash
   docker-compose up -d
   ```

3. **Vérifier que tous les services fonctionnent**
   ```bash
   docker-compose ps
   ```

4. **Accéder aux interfaces web**
   - Console MinIO : http://localhost:9001
   - Tableau de bord Kibana : http://localhost:5601
   - Mongo Express : http://localhost:8081

### 🔧 Configuration d'Environnement

Le fichier `.env` contient tous les paramètres configurables :

#### Configuration MinIO
- `MINIO_ROOT_USER` : Nom d'utilisateur administrateur
- `MINIO_ROOT_PASSWORD` : Mot de passe (minimum 8 caractères)
- `MINIO_API_PORT` / `MINIO_CONSOLE_PORT` : Ports d'accès

#### Configuration MongoDB
- `MONGO_INITDB_ROOT_USERNAME/PASSWORD` : Identifiants root
- `MONGO_INITDB_DATABASE` : Base de données initiale
- `MONGODB_PORT` : Port de connexion

#### Configuration Elasticsearch
- `ELASTICSEARCH_HTTP_PORT` : Port API HTTP
- `XPACK_SECURITY_ENABLED` : Fonctionnalités de sécurité
- `ES_JAVA_OPTS` : Paramètres JVM (mémoire heap)

### 🌐 Guide des Interfaces Web

#### Console MinIO
- **Utilisation** : Gestion des buckets S3, téléchargement de fichiers
- **Fonctionnalités** : Politiques de bucket, gestion des utilisateurs

#### Tableau de bord Kibana
- **Utilisation** : Visualisation des données Elasticsearch
- **Fonctionnalités** : Dashboards, requêtes, monitoring du cluster

#### Mongo Express
- **Utilisation** : Administration de MongoDB
- **Fonctionnalités** : Navigation des collections, édition de documents

### 🧪 Vérifications de Santé

Utilisez les commandes du fichier `check-containers.md` pour vérifier le bon fonctionnement de tous les services.

### 🔒 Notes de Sécurité pour la Production

⚠️ **Important** : Cette configuration est optimisée pour le développement. Pour un déploiement en production :

1. **Modifier tous les mots de passe par défaut**
2. **Activer les certificats TLS/SSL**
3. **Activer les fonctionnalités de sécurité d'Elasticsearch**
4. **Configurer des règles de pare-feu appropriées**
5. **Utiliser les secrets Docker pour les données sensibles**
6. **Mettre en place des stratégies de sauvegarde**
7. **Configurer la surveillance et la journalisation**

---

---

## ☸️ Kubernetes Deployment | Déploiement Kubernetes

[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Minikube](https://img.shields.io/badge/Minikube-326CE5?style=flat&logo=kubernetes&logoColor=white)](https://minikube.sigs.k8s.io/)

---

## 🇺🇸 English - Kubernetes Documentation

### ☸️ Kubernetes Fundamentals

#### 📋 What is Kubernetes?

Kubernetes (K8s) is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. 

💡 **New to Kubernetes?** Check the [Prerequisites & Installation](#prerequisites-and-installation) section first.

It provides:

- **Container Orchestration**: Automatically manages container lifecycle
- **Service Discovery**: Built-in load balancing and service networking
- **Auto-scaling**: Horizontal and vertical scaling based on resource usage
- **Self-healing**: Automatic restart of failed containers and health monitoring
- **Rolling Updates**: Zero-downtime deployments with rollback capabilities
- **Resource Management**: Efficient allocation of CPU, memory, and storage

#### 🎯 Why Use Kubernetes for This Stack?

1. **High Availability**: Ensures services remain accessible even during failures
2. **Scalability**: Easily scale databases and storage based on demand
3. **Resource Efficiency**: Optimized resource allocation and sharing
4. **Production-Ready**: Enterprise-grade orchestration with monitoring and logging
5. **Consistency**: Same deployment method across development, staging, and production
6. **Resilience**: Automatic recovery from node or pod failures

---

### 🛠️ Prerequisites and Installation

### Advice : don't use if it's old and depecreated.  Before install chechk if the current installation guide it's still aviable at the date where you fill find out this repo

#### 🔧 Required Tools
```bash
# 1. Docker (Container Runtime)
sudo apt update && sudo apt install docker.io
sudo usermod -aG docker $USER

# 2. kubectl (Kubernetes CLI)
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# 3. Minikube (Local Kubernetes Cluster)
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```


#### ⚙️ Alternative Kubernetes Options
- **Local Development**: Minikube, Kind, Docker Desktop
- **Cloud Managed**: AWS EKS, Google GKE, Azure AKS
- **Self-Managed**: kubeadm, Rancher, OpenShift

---

### 🏗️ Architecture & Project Structure

#### 🎯 Kubernetes Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    Kubernetes Cluster                          │
│                                                                 │
│  ┌─────────────────┐    ┌─────────────────┐    ┌──────────────┐ │
│  │   Elasticsearch │    │     MongoDB     │    │    MinIO     │ │
│  │                 │    │                 │    │              │ │
│  │ ┌─────────────┐ │    │ ┌─────────────┐ │    │ ┌──────────┐ │ │
│  │ │    Pod      │ │    │ │    Pod      │ │    │ │   Pod    │ │ │
│  │ │ priority:   │ │    │ │ priority:   │ │    │ │priority: │ │ │
│  │ │   HIGH      │ │    │ │   HIGH      │ │    │ │  HIGH    │ │ │
│  │ └─────────────┘ │    │ └─────────────┘ │    │ └──────────┘ │ │
│  │       ▲         │    │       ▲         │    │      ▲       │ │
│  │ ┌─────────────┐ │    │ ┌─────────────┐ │    │ ┌──────────┐ │ │
│  │ │    PVC      │ │    │ │  PVC (data) │ │    │ │   PVC    │ │ │
│  │ │   2Gi       │ │    │ │   2Gi       │ │    │ │   2Gi    │ │ │
│  │ └─────────────┘ │    │ │             │ │    │ └──────────┘ │ │
│  └─────────────────┘    │ │ PVC(config) │ │    └──────────────┘ │
│                         │ │   2Gi       │ │                      │
│  ┌─────────────────┐    │ └─────────────┘ │    ┌──────────────┐ │
│  │     Kibana      │    └─────────────────┘    │ Mongo Express│ │
│  │                 │                           │              │ │
│  │ ┌─────────────┐ │                           │ ┌──────────┐ │ │
│  │ │    Pod      │ │                           │ │   Pod    │ │ │
│  │ │ priority:   │ │                           │ │priority: │ │ │
│  │ │    LOW      │ │                           │ │   LOW    │ │ │
│  │ └─────────────┘ │                           │ └──────────┘ │ │
│  └─────────────────┘                           └──────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

#### 📁 Project Structure

The `K8S/` directory contains organized Kubernetes manifests:

```
K8S/
├── priority-classes.yaml           # Pod scheduling priorities
├── elasticsearch/                  # Elasticsearch service
│   ├── elasticsearch-deployment.yaml
│   ├── elasticsearch-service.yaml
│   └── elasticsearch-data-persistentvolumeclaim.yaml (2Gi)
├── mongoDB/                        # MongoDB service
│   ├── mongodb-deployment.yaml
│   ├── mongodb-service.yaml
│   ├── mongodb-data-persistentvolumeclaim.yaml (2Gi)
│   └── mongodb-config-persistentvolumeclaim.yaml (2Gi)
├── minio/                          # MinIO object storage
│   ├── minio-deployment.yaml
│   ├── minio-service.yaml
│   └── minio-data-persistentvolumeclaim.yaml (2Gi)
├── kibana/                         # Kibana dashboard
│   ├── kibana-deployment.yaml
│   └── kibana-service.yaml
└── mongo-express/                  # MongoDB admin interface
    ├── mongo-express-deployment.yaml
    └── mongo-express-service.yaml
```

---

### 🚀 Deployment Guide

#### 🎬 Quick Start with Minikube

1. **Start Minikube cluster**
   ```bash
   # Start with Docker driver (recommended)
   minikube start --driver=docker --cpus=4 --memory=8192
   
   # Enable useful addons
   minikube addons enable metrics-server
   minikube addons enable dashboard
   ```

2. **Verify cluster is ready**
   ```bash
   kubectl cluster-info
   kubectl get nodes
   ```

3. **Apply priority classes**
   ```bash
   kubectl apply -f K8S/priority-classes.yaml
   ```

4. **Deploy individual services**
   ```bash
   # Deploy data services (high priority)
   kubectl apply -f K8S/elasticsearch/
   kubectl apply -f K8S/mongoDB/
   kubectl apply -f K8S/minio/
   
   # Deploy UI services (low priority)
   kubectl apply -f K8S/kibana/
   kubectl apply -f K8S/mongo-express/
   ```

5. **Check deployment status**
   ```bash
   kubectl get pods -o wide
   kubectl get svc
   kubectl get pvc
   ```

---

### 🔧 Resource Management & Configuration

#### 💾 Resource Allocation

Each service has optimized resource limits for local development:

| Service | CPU Request | CPU Limit | Memory Request | Memory Limit | Priority |
|---------|-------------|-----------|----------------|--------------|----------|
| **Elasticsearch** | 500m | 1000m | 1Gi | 2Gi | HIGH (1000) |
| **MongoDB** | 250m | 500m | 512Mi | 1Gi | HIGH (1000) |
| **MinIO** | 100m | 250m | 256Mi | 512Mi | HIGH (1000) |
| **Kibana** | 100m | 300m | 512Mi | 1Gi | LOW (100) |
| **Mongo Express** | 50m | 100m | 128Mi | 256Mi | LOW (100) |

**Total Resources**: ~5Gi RAM, ~2.1 CPU cores maximum

#### 🌐 Service Access & Networking

⚠️ **Development Configuration**: All services use `ClusterIP` type for security. Access is via port-forwarding.

```bash
# Access services via port-forwarding (recommended for development)
kubectl port-forward svc/elasticsearch 9200:9200
kubectl port-forward svc/minio 9000:9000
kubectl port-forward svc/kibana 5601:5601
kubectl port-forward svc/mongo-express 8081:8081

# Then access in browser:
# - Elasticsearch: http://localhost:9200
# - MinIO: http://localhost:9000
# - Kibana: http://localhost:5601
# - Mongo Express: http://localhost:8081
```

**Why `minikube service --url` doesn't work?**
Services are configured as `ClusterIP` (internal only) for security. Port-forwarding is the preferred method for development as it avoids exposing services directly.

---

### 📊 Operations & Maintenance

#### 🔍 Monitoring and Debugging

```bash
# View Kubernetes dashboard
minikube dashboard

# Monitor pods in real-time
kubectl get pods -w

# Check pod logs
kubectl logs -f deployment/elasticsearch
kubectl logs -f deployment/mongodb
kubectl logs -f deployment/minio

# Describe resources for troubleshooting
kubectl describe pod <pod-name>
kubectl describe pvc <pvc-name>

# Resource usage (requires metrics-server)
kubectl top nodes
kubectl top pods

# Check events for issues
kubectl get events --sort-by=.metadata.creationTimestamp
```

#### ⌨️ Useful Commands Reference

```bash
# Cluster Management
kubectl cluster-info                    # Cluster information
kubectl get nodes                      # List cluster nodes
kubectl get namespaces                 # List all namespaces

# Deployment Operations
kubectl apply -f <file/directory>       # Deploy resources
kubectl delete -f <file/directory>      # Remove resources
kubectl scale deployment <name> --replicas=3  # Scale deployment

# Resource Inspection
kubectl get all                        # List all resources
kubectl get pods -o wide               # Detailed pod information
kubectl get svc                        # List services
kubectl get pvc                        # List persistent volume claims
kubectl get priorityclass              # List priority classes

# Troubleshooting
kubectl describe <resource> <name>     # Detailed resource info
kubectl logs <pod-name>               # Container logs
kubectl exec -it <pod-name> -- /bin/bash  # Access pod shell
kubectl port-forward <pod-name> <local-port>:<pod-port>  # Port forwarding

# Cleanup
kubectl delete pod <pod-name>          # Delete pod (recreated by deployment)
kubectl delete deployment <name>       # Delete deployment
kubectl delete pvc <pvc-name>         # Delete persistent volume claim
```

---

### 🔒 Production Readiness

#### 🚀 Production Optimization

⚠️ **Current Configuration**: This setup is optimized for **development** with local resource constraints.

🔗 **See Also**: [Docker Production Security Notes](#production-security-notes) for additional security considerations.

For production deployments, implement these optimizations:

##### 1. **Service Exposure & Networking**
```yaml
# Change service type from ClusterIP to LoadBalancer
spec:
  type: LoadBalancer  # For cloud providers (AWS ELB, GCP LB, Azure LB)
  ports:
    - port: 9200
      targetPort: 9200
```
- **External access**: LoadBalancer provides public IP addresses
- **SSL termination**: Configure TLS certificates at load balancer level
- **Domain names**: Use ingress controllers with custom domains

##### 2. **High Availability & Scaling**
- **Multi-node cluster**: Minimum 3 master nodes, 3+ worker nodes
- **Pod anti-affinity**: Spread pods across different nodes
```yaml
spec:
  template:
    spec:
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            - labelSelector:
                matchLabels:
                  app: elasticsearch
              topologyKey: kubernetes.io/hostname
```
- **Horizontal Pod Autoscaler**: Auto-scale based on CPU/memory
- **Multiple replicas**: 3+ replicas for each data service

##### 3. **Resource Management**
```yaml
# Production resource limits (example for Elasticsearch)
resources:
  requests:
    memory: "4Gi"      # Increased from 1Gi
    cpu: "1000m"       # Increased from 500m
  limits:
    memory: "8Gi"      # Increased from 2Gi
    cpu: "2000m"       # Increased from 1000m
```
- **Dedicated node pools**: Separate compute for data vs UI services
- **Resource quotas**: Namespace-level resource limits
- **QoS classes**: Guaranteed class for critical services

##### 4. **Storage & Persistence**
```yaml
# Production storage configuration
apiVersion: v1
kind: StorageClass
metadata:
  name: fast-ssd
provisioner: kubernetes.io/gce-pd
parameters:
  type: pd-ssd
  replication-type: regional-pd
```
- **High-performance storage**: SSD-based StorageClasses
- **Backup strategies**: Automated daily/weekly backups
- **Database clustering**: 
  - MongoDB replica sets (3+ nodes)
  - Elasticsearch clusters (3+ master, 5+ data nodes)
- **Regional persistence**: Multi-zone storage replication

##### 5. **Security Hardening**
```yaml
# RBAC configuration example
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: elasticsearch-operator
rules:
- apiGroups: [""]
  resources: ["pods", "services"]
  verbs: ["get", "list", "create", "update"]
```
- **RBAC**: Role-Based Access Control for all services
- **Network policies**: Micro-segmentation between services
- **Pod security policies**: Enforce security standards
- **Secret management**: HashiCorp Vault or AWS Secrets Manager
- **Image scanning**: Vulnerability scanning in CI/CD pipeline
- **mTLS**: Mutual TLS between services

##### 6. **Monitoring & Observability**
```yaml
# Prometheus monitoring example
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: elasticsearch-monitor
spec:
  selector:
    matchLabels:
      app: elasticsearch
```
- **Metrics**: Prometheus + Grafana stack
- **Logging**: Centralized with Fluentd/Fluent Bit → Elasticsearch → Kibana
- **Tracing**: Jaeger or Zipkin for distributed tracing
- **Alerting**: AlertManager with PagerDuty/Slack integration
- **SLO/SLA**: Service Level Objectives with error budgets

##### 7. **Disaster Recovery & Business Continuity**
- **Multi-region deployment**: Active-passive or active-active setup
- **Automated failover**: DNS-based or load balancer failover
- **Data replication**: Cross-region data synchronization
- **Backup testing**: Regular restore testing procedures
- **RPO/RTO targets**: Recovery Point/Time Objectives definition

---

### 🇫🇷 Français - Documentation Kubernetes

#### 📋 Qu'est-ce que Kubernetes ?

Kubernetes (K8s) est une plateforme open-source d'orchestration de conteneurs qui automatise le déploiement, la mise à l'échelle et la gestion des applications conteneurisées. Il fournit :

- **Orchestration de Conteneurs** : Gestion automatique du cycle de vie des conteneurs
- **Découverte de Services** : Équilibrage de charge intégré et réseau de services
- **Auto-scaling** : Mise à l'échelle horizontale et verticale basée sur l'utilisation des ressources
- **Auto-réparation** : Redémarrage automatique des conteneurs défaillants et surveillance de la santé
- **Mises à Jour Continues** : Déploiements sans interruption avec capacités de rollback
- **Gestion des Ressources** : Allocation efficace du CPU, de la mémoire et du stockage

#### 🎯 Pourquoi Utiliser Kubernetes pour Cette Stack ?

1. **Haute Disponibilité** : Assure que les services restent accessibles même en cas de pannes
2. **Scalabilité** : Mise à l'échelle facile des bases de données et du stockage selon la demande
3. **Efficacité des Ressources** : Allocation et partage optimisés des ressources
4. **Prêt pour la Production** : Orchestration de niveau entreprise avec monitoring et logging
5. **Cohérence** : Même méthode de déploiement en développement, staging et production
6. **Résilience** : Récupération automatique des pannes de nœuds ou de pods

#### 🛠️ Prérequis et Outils

### Adivce :  Avant de proceder à l'installation vérifier si une nouvelle façon de faire est d'actualité si celle si est old-school par rapport au moment où vous trouverez ce repo ne l'utiliser pas !

##### Outils Requis :
```bash
# 1. Docker (Runtime de Conteneurs)
sudo apt update && sudo apt install docker.io
sudo usermod -aG docker $USER

# 2. kubectl (CLI Kubernetes)
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# 3. Minikube (Cluster Kubernetes Local)
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```


##### Options Kubernetes Alternatives :
- **Développement Local** : Minikube, Kind, Docker Desktop
- **Cloud Managé** : AWS EKS, Google GKE, Azure AKS
- **Auto-géré** : kubeadm, Rancher, OpenShift

#### 🚀 Démarrage Rapide avec Minikube

1. **Démarrer le cluster Minikube**
   ```bash
   # Démarrer avec le driver Docker (recommandé)
   minikube start --driver=docker --cpus=4 --memory=8192
   
   # Activer les addons utiles
   minikube addons enable metrics-server
   minikube addons enable dashboard
   ```

2. **Vérifier que le cluster est prêt**
   ```bash
   kubectl cluster-info
   kubectl get nodes
   ```

3. **Appliquer les classes de priorité**
   ```bash
   kubectl apply -f K8S/priority-classes.yaml
   ```

4. **Déployer les services individuels**
   ```bash
   # Déployer les services de données (haute priorité)
   kubectl apply -f K8S/elasticsearch/
   kubectl apply -f K8S/mongoDB/
   kubectl apply -f K8S/minio/
   
   # Déployer les services UI (basse priorité)
   kubectl apply -f K8S/kibana/
   kubectl apply -f K8S/mongo-express/
   ```

5. **Vérifier le statut du déploiement**
   ```bash
   kubectl get pods -o wide
   kubectl get svc
   kubectl get pvc
   ```

#### 🔧 Allocation des Ressources

Chaque service a des limites de ressources optimisées pour le développement local :

| Service | CPU Demande | CPU Limite | Mémoire Demande | Mémoire Limite | Priorité |
|---------|-------------|------------|-----------------|----------------|----------|
| **Elasticsearch** | 500m | 1000m | 1Gi | 2Gi | HAUTE (1000) |
| **MongoDB** | 250m | 500m | 512Mi | 1Gi | HAUTE (1000) |
| **MinIO** | 100m | 250m | 256Mi | 512Mi | HAUTE (1000) |
| **Kibana** | 100m | 300m | 512Mi | 1Gi | BASSE (100) |
| **Mongo Express** | 50m | 100m | 128Mi | 256Mi | BASSE (100) |

**Ressources Totales** : ~5Gi RAM, ~2.1 cœurs CPU maximum

#### 🌐 Accéder aux Services

⚠️ **Configuration de Développement** : Tous les services utilisent le type `ClusterIP` pour la sécurité. L'accès se fait via port-forwarding.

```bash
# Accéder aux services via port-forwarding (recommandé pour le développement)
kubectl port-forward svc/elasticsearch 9200:9200
kubectl port-forward svc/minio 9000:9000
kubectl port-forward svc/kibana 5601:5601
kubectl port-forward svc/mongo-express 8081:8081

# Puis accéder dans le navigateur :
# - Elasticsearch : http://localhost:9200
# - MinIO : http://localhost:9000
# - Kibana : http://localhost:5601
# - Mongo Express : http://localhost:8081
```

**Pourquoi `minikube service --url` ne fonctionne pas ?**
Les services sont configurés comme `ClusterIP` (interne uniquement) pour la sécurité. Le port-forwarding est la méthode préférée pour le développement car elle évite d'exposer les services directement.

#### 📊 Surveillance et Débogage

```bash
# Voir le tableau de bord Kubernetes
minikube dashboard

# Surveiller les pods en temps réel
kubectl get pods -w

# Vérifier les logs des pods
kubectl logs -f deployment/elasticsearch
kubectl logs -f deployment/mongodb
kubectl logs -f deployment/minio

# Décrire les ressources pour le dépannage
kubectl describe pod <nom-pod>
kubectl describe pvc <nom-pvc>

# Utilisation des ressources (nécessite metrics-server)
kubectl top nodes
kubectl top pods

# Vérifier les événements pour les problèmes
kubectl get events --sort-by=.metadata.creationTimestamp
```

#### 🔍 Référence des Commandes Utiles

```bash
# Gestion du Cluster
kubectl cluster-info                    # Informations du cluster
kubectl get nodes                      # Lister les nœuds du cluster
kubectl get namespaces                 # Lister tous les namespaces

# Opérations de Déploiement
kubectl apply -f <fichier/répertoire>   # Déployer des ressources
kubectl delete -f <fichier/répertoire>  # Supprimer des ressources
kubectl scale deployment <nom> --replicas=3  # Mettre à l'échelle un déploiement

# Inspection des Ressources
kubectl get all                        # Lister toutes les ressources
kubectl get pods -o wide               # Informations détaillées des pods
kubectl get svc                        # Lister les services
kubectl get pvc                        # Lister les réclamations de volumes persistants
kubectl get priorityclass              # Lister les classes de priorité

# Dépannage
kubectl describe <ressource> <nom>     # Informations détaillées d'une ressource
kubectl logs <nom-pod>                 # Logs du conteneur
kubectl exec -it <nom-pod> -- /bin/bash  # Accès au shell du pod
kubectl port-forward <nom-pod> <port-local>:<port-pod>  # Redirection de port

# Nettoyage
kubectl delete pod <nom-pod>           # Supprimer un pod (recréé par le déploiement)
kubectl delete deployment <nom>        # Supprimer un déploiement
kubectl delete pvc <nom-pvc>          # Supprimer une réclamation de volume persistant
```

#### 🔒 Optimisation pour la Production

⚠️ **Configuration Actuelle** : Cette configuration est optimisée pour le **développement** avec des contraintes de ressources locales.

Pour les déploiements en production, implémentez ces optimisations :

##### 1. **Exposition des Services & Réseau**
```yaml
# Changer le type de service de ClusterIP à LoadBalancer
spec:
  type: LoadBalancer  # Pour les fournisseurs cloud (AWS ELB, GCP LB, Azure LB)
  ports:
    - port: 9200
      targetPort: 9200
```
- **Accès externe** : LoadBalancer fournit des adresses IP publiques
- **Terminaison SSL** : Configurer les certificats TLS au niveau du load balancer
- **Noms de domaine** : Utiliser des contrôleurs ingress avec des domaines personnalisés

##### 2. **Haute Disponibilité & Mise à l'Échelle**
- **Cluster multi-nœuds** : Minimum 3 nœuds master, 3+ nœuds worker
- **Anti-affinité des pods** : Répartir les pods sur différents nœuds
```yaml
spec:
  template:
    spec:
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            - labelSelector:
                matchLabels:
                  app: elasticsearch
              topologyKey: kubernetes.io/hostname
```
- **Horizontal Pod Autoscaler** : Auto-scaling basé sur CPU/mémoire
- **Multiples réplicas** : 3+ réplicas pour chaque service de données

##### 3. **Gestion des Ressources**
```yaml
# Limites de ressources pour la production (exemple Elasticsearch)
resources:
  requests:
    memory: "4Gi"      # Augmenté de 1Gi
    cpu: "1000m"       # Augmenté de 500m
  limits:
    memory: "8Gi"      # Augmenté de 2Gi
    cpu: "2000m"       # Augmenté de 1000m
```
- **Pools de nœuds dédiés** : Calcul séparé pour données vs services UI
- **Quotas de ressources** : Limites de ressources au niveau namespace
- **Classes QoS** : Classe garantie pour les services critiques

##### 4. **Stockage & Persistance**
```yaml
# Configuration de stockage pour la production
apiVersion: v1
kind: StorageClass
metadata:
  name: fast-ssd
provisioner: kubernetes.io/gce-pd
parameters:
  type: pd-ssd
  replication-type: regional-pd
```
- **Stockage haute performance** : StorageClasses basées sur SSD
- **Stratégies de sauvegarde** : Sauvegardes automatisées quotidiennes/hebdomadaires
- **Clustering de bases de données** :
  - Replica sets MongoDB (3+ nœuds)
  - Clusters Elasticsearch (3+ master, 5+ nœuds de données)
- **Persistance régionale** : Réplication de stockage multi-zones

##### 5. **Durcissement de la Sécurité**
```yaml
# Exemple de configuration RBAC
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: elasticsearch-operator
rules:
- apiGroups: [""]
  resources: ["pods", "services"]
  verbs: ["get", "list", "create", "update"]
```
- **RBAC** : Contrôle d'accès basé sur les rôles pour tous les services
- **Politiques réseau** : Micro-segmentation entre services
- **Politiques de sécurité des pods** : Appliquer des standards de sécurité
- **Gestion des secrets** : HashiCorp Vault ou AWS Secrets Manager
- **Analyse d'images** : Scan de vulnérabilités dans le pipeline CI/CD
- **mTLS** : TLS mutuel entre services

##### 6. **Surveillance & Observabilité**
```yaml
# Exemple de surveillance Prometheus
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: elasticsearch-monitor
spec:
  selector:
    matchLabels:
      app: elasticsearch
```
- **Métriques** : Stack Prometheus + Grafana
- **Logging** : Centralisé avec Fluentd/Fluent Bit → Elasticsearch → Kibana
- **Tracing** : Jaeger ou Zipkin pour le tracing distribué
- **Alertes** : AlertManager avec intégration PagerDuty/Slack
- **SLO/SLA** : Objectifs de niveau de service avec budgets d'erreur

##### 7. **Reprise après Sinistre & Continuité d'Activité**
- **Déploiement multi-régional** : Configuration actif-passif ou actif-actif
- **Basculement automatisé** : Basculement basé sur DNS ou load balancer
- **Réplication de données** : Synchronisation de données inter-régionales
- **Test de sauvegarde** : Procédures de test de restauration régulières
- **Objectifs RPO/RTO** : Définition des objectifs de point/temps de récupération

---

## 📚 Additional Resources | Ressources Supplémentaires

- [MinIO Documentation](https://docs.min.io/)
- [MongoDB Manual](https://docs.mongodb.com/)
- [Elasticsearch Guide](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
- [Kibana User Guide](https://www.elastic.co/guide/en/kibana/current/index.html)
- [Docker Compose Documentation](https://docs.docker.com/compose/)

## 📄 License | Licence

This project is open source and available under the [MIT License](LICENSE).

---