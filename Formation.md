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





