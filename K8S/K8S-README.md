# ☸️ Kubernetes Deployment Guide | Guide de Déploiement Kubernetes

[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Minikube](https://img.shields.io/badge/Minikube-326CE5?style=flat&logo=kubernetes&logoColor=white)](https://minikube.sigs.k8s.io/)

## 📚 Table of Contents | Table des Matières

### 🇺🇸 English
- [Quick Start](#quick-start)
- [What is Kubernetes?](#what-is-kubernetes)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Deployment Guide](#deployment-guide)
- [Accessing Services](#accessing-services)
- [Troubleshooting](#troubleshooting)
- [Production Notes](#production-notes)

### 🇫🇷 Français
- [Démarrage Rapide](#démarrage-rapide)
- [Qu'est-ce que Kubernetes ?](#quest-ce-que-kubernetes)
- [Prérequis](#prérequis)
- [Structure du Projet](#structure-du-projet)
- [Guide de Déploiement](#guide-de-déploiement)
- [Accéder aux Services](#accéder-aux-services)
- [Dépannage](#dépannage)
- [Notes de Production](#notes-de-production)

---

## 🇺🇸 English Documentation

### Quick Start

**Prerequisites:** Docker, kubectl, and Minikube installed.

```bash
# 1. Start Minikube
minikube start --driver=docker --cpus=4 --memory=8192

# 2. Enable addons
minikube addons enable metrics-server
minikube addons enable dashboard

# 3. Deploy services
kubectl apply -f priority-classes.yaml
kubectl apply -f minio/
kubectl apply -f mongoDB/
kubectl apply -f elasticsearch/
kubectl apply -f kibana/
kubectl apply -f mongo-express/

# 4. Check status
kubectl get pods
```

### What is Kubernetes?

Kubernetes (K8s) is an open-source container orchestration platform that provides:

- **Container Orchestration**: Automatic container lifecycle management
- **Service Discovery**: Built-in load balancing and networking
- **Auto-scaling**: Horizontal scaling based on resource usage
- **Self-healing**: Automatic restart of failed containers
- **Rolling Updates**: Zero-downtime deployments

**Why use K8s for this stack?**
- High availability for your data services
- Easy scaling based on demand
- Production-ready orchestration
- Consistent deployment across environments

### Prerequisites

⚠️ **Version Warning**: Always check for the latest installation methods as these tools evolve rapidly.

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

**Alternative Options:**
- **Local Development**: Kind, Docker Desktop
- **Cloud**: AWS EKS, Google GKE, Azure AKS

### Project Structure

```
K8S/
├── priority-classes.yaml           # Pod scheduling priorities
├── elasticsearch/                  # Search engine
│   ├── elasticsearch-deployment.yaml
│   ├── elasticsearch-service.yaml
│   └── elasticsearch-data-persistentvolumeclaim.yaml (2Gi)
├── mongoDB/                        # Document database
│   ├── mongodb-deployment.yaml
│   ├── mongodb-service.yaml
│   ├── mongodb-data-persistentvolumeclaim.yaml (2Gi)
│   └── mongodb-config-persistentvolumeclaim.yaml (2Gi)
├── minio/                          # Object storage
│   ├── minio-deployment.yaml
│   ├── minio-service.yaml
│   └── minio-data-persistentvolumeclaim.yaml (2Gi)
├── kibana/                         # Elasticsearch dashboard
│   ├── kibana-deployment.yaml
│   └── kibana-service.yaml
└── mongo-express/                  # MongoDB admin interface
    ├── mongo-express-deployment.yaml
    └── mongo-express-service.yaml
```

**Resource Allocation (Development):**

| Service | CPU Request | CPU Limit | Memory Request | Memory Limit | Priority |
|---------|-------------|-----------|----------------|--------------|----------|
| Elasticsearch | 500m | 1000m | 1Gi | 2Gi | HIGH (1000) |
| MongoDB | 250m | 500m | 512Mi | 1Gi | HIGH (1000) |
| MinIO | 100m | 250m | 256Mi | 512Mi | HIGH (1000) |
| Kibana | 100m | 300m | 512Mi | 1Gi | LOW (100) |
| Mongo Express | 50m | 100m | 128Mi | 256Mi | LOW (100) |

**Total:** ~5Gi RAM, ~2.1 CPU cores maximum

### Deployment Guide

#### 1. Start Minikube
```bash
minikube start --driver=docker --cpus=4 --memory=8192
minikube addons enable metrics-server dashboard
```

#### 2. Apply Priority Classes
```bash
kubectl apply -f priority-classes.yaml
```

#### 3. Deploy Data Services (High Priority)
```bash
kubectl apply -f elasticsearch/
kubectl apply -f mongoDB/
kubectl apply -f minio/
```

#### 4. Deploy UI Services (Low Priority)
```bash
kubectl apply -f kibana/
kubectl apply -f mongo-express/
```

#### 5. Verify Deployment
```bash
kubectl get pods -o wide
kubectl get svc
kubectl get pvc
```

### Accessing Services

⚠️ **Development Setup**: Services use `ClusterIP` for security. Access via port-forwarding.

```bash
# Access services
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

**Why not `minikube service --url`?**
Services are configured as `ClusterIP` (internal only) for security. Port-forwarding is the preferred development method.

### Troubleshooting

#### Common Issues

**Pod stuck in `Pending`:**
```bash
kubectl describe pod <pod-name>
# Usually: Insufficient resources or PVC issues
```

**Pod restarting:**
```bash
kubectl logs <pod-name> --previous
# Check liveness probe failures
```

**PVC not bound:**
```bash
kubectl get pvc
kubectl describe pvc <pvc-name>
# Check storage class and available space
```

#### Useful Commands

```bash
# Monitoring
kubectl get pods -w                 # Watch pod changes
kubectl top nodes                   # Resource usage
kubectl top pods                    # Pod resource usage

# Debugging
kubectl logs -f <pod-name>          # Follow logs
kubectl exec -it <pod-name> -- /bin/bash  # Pod shell
kubectl describe <resource> <name>  # Detailed info

# Management
minikube dashboard                  # Web UI
kubectl get events --sort-by=.metadata.creationTimestamp
```

### Production Notes

⚠️ **Current Setup**: Optimized for development. For production:

**Service Exposure:**
```yaml
spec:
  type: LoadBalancer  # Instead of ClusterIP
```

**High Availability:**
- Multi-node cluster (3+ masters, 3+ workers)
- Multiple replicas for each service
- Pod anti-affinity rules

**Security:**
- RBAC configuration
- Network policies
- Secret management (Vault)

**Monitoring:**
- Prometheus + Grafana
- Centralized logging
- Alert management

---

## 🇫🇷 Documentation Française

### Démarrage Rapide

**Prérequis :** Docker, kubectl et Minikube installés.

```bash
# 1. Démarrer Minikube
minikube start --driver=docker --cpus=4 --memory=8192

# 2. Activer les addons
minikube addons enable metrics-server
minikube addons enable dashboard

# 3. Déployer les services
kubectl apply -f priority-classes.yaml
kubectl apply -f minio/
kubectl apply -f mongoDB/
kubectl apply -f elasticsearch/
kubectl apply -f kibana/
kubectl apply -f mongo-express/

# 4. Vérifier le statut
kubectl get pods
```

### Qu'est-ce que Kubernetes ?

Kubernetes (K8s) est une plateforme open-source d'orchestration de conteneurs qui fournit :

- **Orchestration de Conteneurs** : Gestion automatique du cycle de vie
- **Découverte de Services** : Équilibrage de charge et réseau intégrés
- **Auto-scaling** : Mise à l'échelle basée sur l'utilisation des ressources
- **Auto-réparation** : Redémarrage automatique des conteneurs défaillants
- **Mises à Jour Continues** : Déploiements sans interruption

**Pourquoi utiliser K8s pour cette stack ?**
- Haute disponibilité pour vos services de données
- Mise à l'échelle facile selon la demande
- Orchestration prête pour la production
- Déploiement cohérent entre environnements

### Prérequis

⚠️ **Avertissement Version** : Vérifiez toujours les dernières méthodes d'installation car ces outils évoluent rapidement.

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

**Options Alternatives :**
- **Développement Local** : Kind, Docker Desktop
- **Cloud** : AWS EKS, Google GKE, Azure AKS

### Structure du Projet

La structure K8S/ contient des manifests organisés par service, chacun avec son deployment, service et storage persistant. Consultez la section anglaise pour le détail complet.

**Allocation des Ressources (Développement) :**
- **Total** : ~5Gi RAM, ~2.1 cœurs CPU maximum
- **Services de données** : Priorité HAUTE (1000)
- **Interfaces utilisateur** : Priorité BASSE (100)

### Guide de Déploiement

Suivez les mêmes étapes que la section anglaise :
1. Démarrer Minikube avec les ressources adéquates
2. Appliquer les classes de priorité
3. Déployer les services de données en premier
4. Déployer les interfaces utilisateur
5. Vérifier le déploiement

### Accéder aux Services

⚠️ **Configuration Développement** : Les services utilisent `ClusterIP` pour la sécurité. Accès via port-forwarding.

```bash
# Accéder aux services
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

### Dépannage

#### Problèmes Courants

**Pod bloqué en `Pending` :**
```bash
kubectl describe pod <nom-pod>
# Généralement : Ressources insuffisantes ou problèmes PVC
```

**Pod qui redémarre :**
```bash
kubectl logs <nom-pod> --previous
# Vérifier les échecs de liveness probe
```

#### Commandes Utiles

```bash
# Surveillance
kubectl get pods -w                 # Surveiller les changements
kubectl top nodes                   # Utilisation des ressources
kubectl top pods                    # Ressources des pods

# Débogage
kubectl logs -f <nom-pod>           # Suivre les logs
kubectl exec -it <nom-pod> -- /bin/bash  # Shell du pod
kubectl describe <ressource> <nom>  # Informations détaillées

# Gestion
minikube dashboard                  # Interface web
kubectl get events --sort-by=.metadata.creationTimestamp
```

### Notes de Production

⚠️ **Configuration Actuelle** : Optimisée pour le développement. Pour la production :

**Exposition des Services :**
```yaml
spec:
  type: LoadBalancer  # Au lieu de ClusterIP
```

**Haute Disponibilité :**
- Cluster multi-nœuds (3+ masters, 3+ workers)
- Multiples réplicas pour chaque service
- Règles d'anti-affinité des pods

**Sécurité :**
- Configuration RBAC
- Politiques réseau
- Gestion des secrets (Vault)

**Surveillance :**
- Prometheus + Grafana
- Logging centralisé
- Gestion des alertes

---

## 📚 Additional Resources | Ressources Supplémentaires

- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Minikube Documentation](https://minikube.sigs.k8s.io/docs/)
- [Main Project README](../README.md)

## 📄 License | Licence

This project is open source and available under the [MIT License](../LICENSE).