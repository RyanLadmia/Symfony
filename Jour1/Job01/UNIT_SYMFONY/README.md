- # **Etape A :**  
  
# Etape 1 : Préparer l'environnement :  
  
- **Installer Docker et Docker compose :**  
  
Pour vérifier que Docker est installé et configuré, ouvrir un terminal et entrer :  
```
docker --version
```  
  
Résultat si docker est installé :  
    
![Image n°1](image/1.png)  
  
-----------------------------------------------------------------------------------------------------------------------------------------------
Pour vérifier que Docker Compose est installé et configurer, ouvrir un terminal et entrer :
```
docker-compose --version
```  
  
Résultat sit Docker compose est installé :  
  
![Image n°2](image/2.png)  
  
-----------------------------------------------------------------------------------------------------------------------------------------------
- **Installer Composer :**  
  
Puisque Symfony utilise Composer pour fonctionner, il faut vérifier qu'il soit bien installé et configuré :  
```
composer --version
```  

et pour le mettre a jour sous Windows :  
```
composer sefl-upgrade
```
  
Résultat si Composer est installé :  
  
![Image n°3](image/3.png)  

-----------------------------------------------------------------------------------------------------------------------------------------------
# Etape 2 : Créer un dossier de projet :  
  
Création du dossier UNIT_SYMFONY :  
  
![Image n°4](image/4.png)  

-----------------------------------------------------------------------------------------------------------------------------------------------
# Etape 3 : Préparer le fichier docker-compose.yml :  
  
- **1 - Création du fichier  à la racine du projet :**  

![Image n°5](image/5.png)  

- **2 - Explication du fichier :**  
  
Ce fichier définit plusieurs services (conteneurs) qui fonctionneront ensemble pour exécuter une application Symfony avec une base de données MySQL et des outils d'administration.  
.  
.  
.  

- Le service app exécute l'application PHP avec PHP-FPM (FastCGI Process Manager).  
```
app:
    image: php:8.2-fpm
```
Utilise l’image officielle de PHP version 8.2 avec FPM pour gérer l’exécution du code PHP.  
```
build:
    context: .
    dockerfile: Dockerfile
```
Permet de construire une image à partir d'un Dockerfile présent dans le même dossier (.).  
Si ce bloc est présent, image: php:8.2-fpm peut être redondant, car l’image sera construite à partir du Dockerfile.  
```
container_name: symfony_app
```
Donne un nom spécifique au conteneur : symfony_app au lieu d’un nom aléatoire généré par Docker.  
```
working_dir: /var/www/html
```
Définit le répertoire de travail à /var/www/html, où Symfony sera installé.  
```
volumes: 
    - ./app:/var/www/html
```
Monte le dossier local ./app dans le conteneur à /var/www/html, ce qui permet d’éditer le code en local et de voir les changements immédiatement sans reconstruire l’image.  
```
networks:
    - symfony_network
```
Connecte le service au réseau Docker symfony_network, permettant aux autres services de communiquer entre eux.   
.  
.  
.  

- Le service webserver exécute un serveur Nginx pour exposer l’application PHP sur un navigateur.  
```
webserver:
    image: nginx:stable
```
Utilise l’image stable de Nginx comme serveur web.  
```
container_name: symfony_webserver
```
Nom du conteneur : symfony_webserver.  
```
ports:
    - "8080:80"
```
Mappe le port 80 du conteneur (port par défaut de Nginx) sur le port 8080 de la machine hôte, ce qui permet d’accéder au site via http://localhost:8080.  
```
volume:
    - ./app:/var/www/html
    - ./nginx:/etc/nginx/conf.d
```
Monte deux volumes :

   1:  ./app est mappé sur /var/www/html pour que Nginx puisse servir les fichiers de l’application.  
   2:  ./nginx est mappé sur /etc/nginx/conf.d pour utiliser un fichier de configuration personnalisé de Nginx.  
```
depends_on:
    - app
```
Indique que ce conteneur dépend de app, donc il ne démarrera qu’après le conteneur PHP.  
```
networks:
    - symfony_network
```
Connecte le conteneur au réseau symfony_network.  
.  
.  
.  
- Le service database crée une base de données MySQL 8.  
```

``` 