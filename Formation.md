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
echo "Bonjour Docker" > test.txt
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








