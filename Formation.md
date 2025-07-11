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


