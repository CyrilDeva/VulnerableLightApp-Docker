# Mise en œuvre 

Pour le déploiement de l'application web VulnerableLightApp sur Docker Hub, un fichier Dockerfile a été créé afin de définir l'environnement et les étapes nécessaires pour exécuter cette application sur une image Debian. Le fichier Dockerfile est conçu pour configurer une image prête à exécuter l'application.

## Script
Voici les étapes détaillées et le script utilisé :
```
FROM debian:latest

USER root

# Mise à jour du système et installation des outils requis
RUN apt update  && \ 
    apt upgrade -y && \
    apt install -y wget sudo git

# Ajout du dépôt Microsoft pour l'installation de .NET
RUN wget https://packages.microsoft.com/config/debian/12/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
RUN dpkg -i packages-microsoft-prod.deb
RUN rm packages-microsoft-prod.deb

# Installation des outils et bibliothèques nécessaires pour .NET
RUN apt update && \
   sudo apt install -y dotnet-sdk-8.0 && \
   sudo apt install -y aspnetcore-runtime-8.0 dotnet-runtime-8.0

# Clonage du dépôt de l'application
WORKDIR /app
RUN git clone https://github.com/CyrilDeva/VulnerableLightApp.git

# Définition du répertoire de travail et commande de lancement
WORKDIR /app/VulnerableLightApp
CMD ["dotnet", "run", "--url=https://0.0.0.0:3000"]
```  

## Explication des étapes
•	**Base de l'image** : L'image de base utilisée est debian:latest

•	**Mise à jour et installation des outils** : La commande apt met à jour le système et installe des outils essentiels comme wget, sudo, et git.

•	**Ajout du support pour .NET** : 
    Le fichier packages-microsoft-prod.deb est récupéré pour ajouter le dépôt Microsoft.
    Ce dépôt permet l'installation de l'environnement .NET nécessaire à l'exécution de l'application.

•	**Installation de .NET SDK et runtime** : Ces outils sont indispensables pour compiler et exécuter les applications développées avec .NET.

•	**Clonage du dépôt GitHub** : L'application VulnerableLightApp est clonée depuis son dépôt GitHub dans le répertoire /app.

•	**Commande de lancement** : L'application est lancée en utilisant la commande dotnet run avec l'option -url pour écouter sur l'adresse 0.0.0.0 au port 3000.

## Test
Une fois le fichier Dockerfile créé, les tests suivants ont été effectués :
1.	**Construction de l'image Docker avec la commande** :

    `docker build -t vulnerable-light-app .`

Le -t est pour préciser un tag, ici on ne le précise pas donc par defaut le tag sera : latest


2.	**Lancement du conteneur pour vérifier le bon fonctionnement de l'application** :

    `docker run -p 3000:3000 vulnerable-light-app`


3.  **Validation de l'accessibilité** de l'application via l'URL
http://localhost:3000.


4.  **Upload sur DockerHub**
    docker push <DOCKER_USERNAME>/vulnerable-light-app

Ce fichier Dockerfile a permis de préparer une image prête pour le déploiement de l'application sur Docker Hub.
(https://hub.docker.com/r/cyrildeva/vulnerablelightapp-docker)

Enfin, pour lancer l'application depuis DockerHub on peut faire :
`docker pull cyrildeva/vulnerablelightapp-docker`


