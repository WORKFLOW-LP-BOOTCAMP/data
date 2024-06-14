# MongoDB avec Docker

Ce guide explique comment installer MongoDB dans un conteneur Docker.

Vous devez donc au préalable installer Docker sur votre système avant de continuer : https://docs.docker.com/get-docker/

Si vous possédez **Windows**, il est plutôt conseillé d'installer MongoDB manuellement (c.f. guide [installation manuel](02_chap_install_manuelle_OS.md
) ) car Docker prend du temps à installer et à paramétrer si vous ne l'avez pas déjà.
Si vous possédez **macOS** ou **Linux**, vous pouvez en revanche continuer la lecture ici.

## Initialisation 

Tout au long de cette semaine, vous allez utiliser MongoDB dans un **conteneur Docker**, lequel a déjà été préparé pour vous.

Commencez d'abord par créer l'image Docker ainsi que votre conteneur avec le fichier `docker-compose.yml` fourni :

```bash
docker-compose up
# Démonisé en arrière-plan
docker-compose up -d
# Reconstruire les images
docker-compose up --build
# Arrêter les conteneurs
docker-compose stop
# Arrêter et supprimer les conteneurs,
docker-compose down
```

Attention pour stopper le conteneur il faut se trouver dans le dossier où se trouve docker-compose.yaml

Une fois le conteneur lancé, vous pouvez vous connecter à un terminal bash à l'intérieur : 

```bash
docker exec -it docker_mongo bash
```

## Se connecter à MongoDB

Tout en restant dans le conteneur, connectez-vous à MongoDB grâce au programme `mongosh`

*Le login est "root" et le mot de passe est "example" (voir le fichier stack.yml)*

```bash
mongosh -u root -p example
```

## Installation des données pour les exercices

Connectez vous au conteneur mongoDB, les données sont partagées dans le dossier data sur votre machine hôte avec le conteneur docker_mongo.

```bash
docker exec -it docker_mongo bash
# Une fois dans le conteneur créer et insérer les données.
cd data/db

mongoimport --db ny \
            --collection restaurants \
            --authenticationDatabase admin \
            --username root \
            --password example \
            --drop \
            --file ./restaurants.json
```
