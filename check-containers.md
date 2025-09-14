# Vérification des Conteneurs et Accès GUI

## Commande de Vérification
```bash
docker-compose ps
```

## Résultat Attendu
Tous les conteneurs doivent afficher le statut `(healthy)`:
```
NAME            STATUS
elasticsearch   Up X minutes (healthy)
kibana          Up X minutes (healthy)  
minio           Up X minutes (healthy)
mongo-express   Up X minutes (healthy)
mongodb         Up X minutes (healthy)
```

## Accès aux Interfaces Web (GUI)

### MinIO Console
- **URL**: http://localhost:9001
- **Utilisateur**: `admin`
- **Mot de passe**: `adminpass123`
- **Description**: Gestion des buckets et fichiers

### Kibana Dashboard  
- **URL**: http://localhost:5601
- **Authentification**: Aucune (désactivée en développement)
- **Description**: Visualisation et analyse Elasticsearch

### Mongo Express
- **URL**: http://localhost:8081  
- **Utilisateur**: `admin`
- **Mot de passe**: `adminpass123`
- **Description**: Interface web pour MongoDB

## Configuration depuis .env

### Ports Configurables
```env
MINIO_API_PORT=9000
MINIO_CONSOLE_PORT=9001
MONGODB_PORT=27017
ELASTICSEARCH_HTTP_PORT=9200
KIBANA_PORT=5601
MONGO_EXPRESS_PORT=8081
```

### Identifiants Configurables
```env
# MinIO
MINIO_ROOT_USER=admin
MINIO_ROOT_PASSWORD=adminpass123

# MongoDB  
MONGO_INITDB_ROOT_USERNAME=admin
MONGO_INITDB_ROOT_PASSWORD=admin

# Mongo Express
MONGO_EXPRESS_USERNAME=admin
MONGO_EXPRESS_PASSWORD=adminpass123
```

## Test de Connectivité
```bash
# Elasticsearch
curl http://localhost:9200/_cluster/health

# MinIO API
curl http://localhost:9001/minio/health/live

# MongoDB
docker exec mongodb mongosh --eval "db.adminCommand('ping')"

# Kibana
curl http://localhost:5601/api/status

# Mongo Express
curl -I http://localhost:8081
```