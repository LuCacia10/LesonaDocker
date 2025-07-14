# Definition

Docker est une plateforme permettant de lancer certaines applications dans des conteneurs logiciels lancée en 2013. C'est un outil qui peut empaqueter une application et ses dépendances dans un conteneur isolé, qui pourra être exécuté sur n'importe quel serveur ». Il ne s'agit pas de virtualisation, mais de conteneurisation, une forme plus légère qui s'appuie sur certaines parties de la machine hôte pour son fonctionnement.

# Premier Conteneur
  ```
docker run hello-world

  ```
- Docker cherche l'image localement
- Crée un conteneur éphémère

# Gestion des Images
- Liste les images locales :
```
docker images

```
- Montre tous les conteneurs (même arrêtés)
```
docker ps -a
  
```

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
- Construire l'image :
```
docker build -t mon-curleur .

```
- Lancer le conteneur :
```
docker run mon-curleur

```

# Gestion des volumes
- Créer un volume :

```
docker volume create mon-volume

```
- Lancer un conteneur avec le volume :
```
docker run -d -v mon-volume:/data --name conteneur-volume alpine tail -f /dev/null

```
- Copier un fichier dans le volume : 
```
echo "Bonjour!" > test.txt
docker cp test.txt conteneur-volume:/data/

```
- Vérifier le contenu :
```
docker exec conteneur-volume cat /data/test.txt

```

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
- Démarrer les services :
```
docker-compose up -d

```
- Vérifier l'état :
```
docker-compose ps

```
- Arrêter les services :
```
docker-compose down

```
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
- Lister les réseaux existants :
```
docker network ls

```
## Lister les réseaux existants

- Créer un réseau bridge personnalisé :
```
docker network create --driver bridge mon-reseau

```
- Inspecter le réseau :
```
docker network inspect mon-reseau

```

## Connecter des conteneurs à un réseau
- Lancer deux conteneurs sur le même réseau :
```
docker run -d --name conteneur1 --network mon-reseau alpine sleep 3600
docker run -d --name conteneur2 --network mon-reseau alpine sleep 3600

```
- Tester la connexion :
```
docker exec conteneur1 ping conteneur2

```

## Types de réseaux avancés
- Créer un réseau macvlan (nécessite des privilèges) :
```
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  mon-macvlan

```

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







