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


