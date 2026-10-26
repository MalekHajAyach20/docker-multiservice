# TaskFlow 

Ce projet consiste à conteneuriser une application à l'aide de Docker et Docker Compose.

Je réalise ce projet dans le cadre de mon apprentissage pratique du DevOps. Mon objectif est de comprendre comment organiser, configurer et faire communiquer plusieurs services dans un environnement conteneurisé.
## Architecture de l'application

L'application repose sur trois services principaux :

Frontend : interface utilisateur accessible sur le port 8080.

Backend : API Node.js chargée de traiter les requêtes liées aux tâches, accessible sur le port 4000.

Base de données : PostgreSQL 16, utilisé pour stocker les tâches.
## Technologies

Docker

Docker Compose

Node.js

PostgreSQL 16

YAML

Linux (Ubuntu)

## Ce que j'ai mis en place

- **Dockerfile du backend** : image Node 20 Alpine, installation des dépendances de production uniquement.
- **Dockerfile du frontend en deux étapes** : Node compile l'application avec Vite, puis seule la sortie statique (`dist`) est copiée dans une image Nginx. L'image finale ne contient ni Node ni `node_modules`.
- **Nginx en non-root** : écoute sur le port 8080, fichiers temporaires dans `/tmp`, droits donnés à l'utilisateur `nginx` avant de basculer sur `USER nginx`.
- **Healthchecks** : route `/healthz` sur le frontend, `pg_isready` sur la base.
- **Docker Compose** : trois services, un réseau dédié , un volume  pour garder les données de la base, `depends_on` avec `service_healthy` pour que le backend attende une base réellement prête, et `restart: unless-stopped`.


## Installation et lancement du projet

### Prérequis

Avant de commencer, assure-toi d'avoir installé **Docker** et **Docker Compose** sur ta machine.

### 1. Récupérer le projet

```bash
git clone <URL_DE_MON_DEPOT>
cd <NOM_DU_DEPOT>
```

Remplace les valeurs entre chevrons par l'URL et le nom de ton propre dépôt.

### 2. Préparer la configuration

Créer les fichiers d'environnement à partir des exemples fournis :

```bash
cp .env.example .env
cp backend/.env.example backend/.env
```

Ouvre ensuite les deux fichiers et remplace `change_me` par un mot de passe de ton choix. Utilise le même mot de passe pour PostgreSQL et pour la connexion du backend à la base de données.

### 3. Construire et démarrer l'application

Depuis le répertoire du projet, lance :

```bash
docker compose up -d --build
```

Cette commande construit les images à partir des Dockerfiles locaux, puis démarre les différents services en arrière-plan.

Une fois les services démarrés, l'application est accessible à l'adresse suivante :

**http://localhost:8080**

### 4. Vérifier le fonctionnement

Pour contrôler l'état des conteneurs, exécute :

```bash
docker compose ps
```

Vérifie que la base de données est indiquée comme `healthy` et que les services backend et frontend sont démarrés.

Pour consulter les journaux du backend en cas de problème :

```bash
docker compose logs backend
```

### 5. Arrêter l'application

Pour arrêter les services et supprimer les conteneurs sans supprimer les données persistantes :



## Crédits

Le code applicatif (frontend React et API Node.js) provient du dépôt de référence [taskflow-3tier-docker-deployment](https://github.com/Ramkumar200314/taskflow-3tier-docker-deployment), proposé comme projet pratique de déploiement d'une application multi-tiers.

Mon travail porte principalement sur la conteneurisation et la configuration de l'environnement : les Dockerfiles, le fichier `docker-compose.yaml`, la gestion des variables d'environnement et la documentation du projet.

Le fichier `nginx.conf` a également été adapté à partir de celui du dépôt de référence.
docker compose down
```
