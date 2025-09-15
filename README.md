# 🚀 Stack

[![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)](https://www.docker.com/)
[![MinIO](https://img.shields.io/badge/MinIO-C72E49?style=flat&logo=minio&logoColor=white)](https://min.io/)
[![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat&logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=flat&logo=elasticsearch&logoColor=white)](https://www.elastic.co/)
[![Kibana](https://img.shields.io/badge/Kibana-005571?style=flat&logo=kibana&logoColor=white)](https://www.elastic.co/kibana/)

## 📖 English | 🇫🇷 Français

---

## 🇺🇸 English Documentation

### 📋 Overview

This project provides a complete enterprise-ready storage and analytics stack using Docker Compose. It includes object storage, document database, search engine, and administrative web interfaces.

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

1. **Change all default passwords**
2. **Enable TLS/SSL certificates**
3. **Enable Elasticsearch security features**
4. **Configure proper firewall rules**
5. **Use Docker secrets for sensitive data**
6. **Implement backup strategies**
7. **Set up monitoring and logging**

---

## 🇫🇷 Documentation Française

### 📋 Aperçu

Ce projet fournit une stack complète de stockage et d'analyse prête pour l'entreprise utilisant Docker Compose. Il inclut le stockage d'objets, une base de données documentaire, un moteur de recherche et des interfaces web d'administration.

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
   git clone <url-du-depot>
   cd testMinio-elastic-mongoDB
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

## 📚 Additional Resources | Ressources Supplémentaires

- [MinIO Documentation](https://docs.min.io/)
- [MongoDB Manual](https://docs.mongodb.com/)
- [Elasticsearch Guide](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
- [Kibana User Guide](https://www.elastic.co/guide/en/kibana/current/index.html)
- [Docker Compose Documentation](https://docs.docker.com/compose/)

## 📄 License | Licence

This project is open source and available under the [MIT License](LICENSE).

---