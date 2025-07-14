# Definition

Docker est une plateforme de virtualisation légère qui permet de créer, déployer et exécuter des applications dans des environnements isolés appelés conteneurs.

# Premier Conteneur
  ```
docker run hello-world

  ```
- Docker cherche l'image localement
- Crée un conteneur éphémère

# Gestion des Images
```
docker images

```
- Liste les images locales
```
docker ps -a
  
```
- Montre tous les conteneurs (même arrêtés)

# Conteneur Nginx
```
docker run -d -p 8080:80 --name mon-nginx nginx
  
```
- -d : Détache le terminal (mode démon)

- -p 8080:80 : Mappe le port 8080 de l'hôte vers le 80 du conteneur

- --name : Donne un nom explicite au conteneur

# Créer une image personnalisée
Créez un fichier Dockerfile avec ce contenu:
```
FROM alpine
RUN apk add --no-cache curl
CMD ["curl", "https://www.google.com"]

```
Puis exécutez:
```
docker build -t mon-curleur .

```
- Construire l'image
```
docker run mon-curleur

```
- Lancer le conteneur

# Gestion des volumes
```
docker volume create mon-volume

```
- Créer un volume
```
docker run -d -v mon-volume:/data --name conteneur-volume alpine tail -f /dev/null

```
- Lancer un conteneur avec le volume
```
echo "Bonjour!" > test.txt
docker cp test.txt conteneur-volume:/data/

```
- Copier un fichier dans le volume
```
docker exec conteneur-volume cat /data/test.txt

```
- Vérifier le contenu

# Docker Compose
Créez un fichier docker-compose.yml:
```
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8000:80"
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
```
Puis exécutez:
```
docker-compose up -d

```
- Démarrer les services
```
docker-compose ps

```
- Vérifier l'état
```
docker-compose down

```
- Arrêter les services

# Nettoyage
```
docker container prune

```
- Supprimer tous les conteneurs arrêtés

```
docker image prune -a

```
- Supprimer toutes les images non utilisées
```
docker volume prune

```
- Supprimer tous les volumes non utilisés

# Les réseaux Docker
## Types de réseaux disponibles
```
docker network ls

```
- Lister les réseaux existants
## Lister les réseaux existants
```
docker network create --driver bridge mon-reseau

```
- Créer un réseau bridge personnalisé
```
docker network inspect mon-reseau

```
- Inspecter le réseau
## Connecter des conteneurs à un réseau
```
docker run -d --name conteneur1 --network mon-reseau alpine sleep 3600
docker run -d --name conteneur2 --network mon-reseau alpine sleep 3600

```
- Lancer deux conteneurs sur le même réseau
```
docker exec conteneur1 ping conteneur2

```
- Tester la connexion
## Types de réseaux avancés
```
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  mon-macvlan

```
- Créer un réseau macvlan (nécessite des privilèges)
# Dockerfiles avancés
## Multi-stage builds
Dockerfile :
```

FROM golang:1.18 as builder
WORKDIR /app
COPY . .
RUN go build -o monapp .

FROM alpine:latest
WORKDIR /root/
COPY --from=builder /app/monapp .
CMD ["./monapp"]

```
## Optimisation du cache
Dockerfile :
```

FROM node:14
WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

ARG APP_VERSION
ENV VERSION=${APP_VERSION}

HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1

EXPOSE 3000
CMD ["node", "server.js"]

```
##  Build context et .dockerignore
```
node_modules
.git
*.md
*.log

```
## Build avec arguments
```

docker build --build-arg APP_VERSION=1.0.0 -t mon-app .

```







