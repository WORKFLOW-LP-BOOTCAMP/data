# Installer MongoDB manuellement

Ce guide explique comment installer manuellement MongoDB sur votre machine.

Si vous possédez **macOS** ou **Linux**, il est plutôt conseillé d'utiliser Docker (c.f. guide `Docker.md`) pour éviter d'avoir à paramétrer complètement votre environnement pour accueillir MongoDB.
Si vous possédez **Windows** *et que vous n'avez pas déjà Docker*, vous pouvez en revanche continuer la lecture ici.

> Sommaire :
> - [Windows](#windows)
> - [macOS](#macos)
> - [Linux](#linux)

## Windows

Vous pouvez installer MongoDB en utilisant l'installeur officiel **MongoDB Community Server** :

https://www.mongodb.com/try/download/community

**ATTENTION** : Veillez à choisir l'installation **Custom** et non complète, puis choisissez **et notez** l'emplacement des fichiers et dossiers de Mongo dans votre système (par défaut : `C:\Program Files\MongoDB\Server\`).
Choisissez l'installation de MongoD en tant que Service, et **n'installez pas MongoDB Compass** : c'est une interface graphique pour Mongo qui peut être installée séparament. Mais nous utiliserons la ligne de commande pour dialoguer avec Mongo dans ce cours.

Une fois l'installation terminée, ouvrez un terminal et vérifiez que vous avez accès à la commande `mongod` en tapant : `mongod --v`.

Si vous obtenez une erreur type *- commande non reconnue -*, il faut alors modifier votre variable d'environnement `Path` :

1. Ouvrez le menu "Démarrer"
2. Tapez *"Modifier les variables d'environnement système"*
3. Cliquez sur le bouton *"Variables d'environnement..."*
4. Cliquez sur la variable `Path` dans la liste des variables utilisateur et cliquez sur "Modifier"
5. Créez une nouvelle entrée contenant le chemin vers `/bin` à l'emplacement où vous avez installé Mongo (par défaut : `C:\Program Files\MongoDB\Server\<VOTRE_VERSION>\bin`)
6. Refermez toutes les fenêtres en appuyant sur OK.
7. Relancez **complètement** votre terminal et tapez à nouveau la commande `mongo --version`

---

Mongo dispose de 2 éléments :

- Le serveur de base de donnée (`mongod.exe`).
- Le client (`mongo.exe`) pour se connecter au serveur.

Comme vous avez installé MongoD en tant que Service, vous n'aurez pas besoin de lancer manuellement le serveur 👍.

### Outils supplémentaires

#### MongoDB Tools

Il s'agit d'un ensemble d'outils pour aider notamment à l'import/export de données dans une base Mongo.

[Installer les MongoDB Database Tools ](https://www.mongodb.com/try/download/database-tools)

Récupérez l'archive et décompressez-la sur votre ordinateur, de préférence dans un répertoire personnalisé (par ex : `C:/dev-programs/mongodb-tools/`)

Il vous faudra aussi ajouter le dossier `/bin` à votre Path, comme expliqué plus haut.

#### Mongo SH

Le programme Mongo Shell (`mongosh.exe`) est une version modernisée du CLI installé par défaut avec MongoDB (`mongo.exe`).

Cette installation n'est pas obligatoire mais plutôt conseillée pour avoir des couleurs et des options supplémentaires pour manipuler vos bases de données.

[Installer Mongo Shell](https://www.mongodb.com/try/download/shell)

Une fois installé, ouvrez un terminal et tapez la commande :
```bash
mongosh
```

## Installation des données 

Placez vous dans le dossier où se trouve les données dossier Data

[restaurants](../../Data/restaurants.json)

```bash
# première solution
mongoimport --uri mongodb://localhost:27017/ny --collection restaurants --file restaurants.json --jsonArray

# deuxième solution
mongoimport --db ny \
            --collection restaurants \
            --drop \
            --file ./restaurants.json --jsonArray
```


## Dernière option pour tester Mongo

Vous avez eu un problème et n'avez pas réussi l'installation de MongoDB sur votre système ?
Vous pouvez toujours le faire plus tard et tester Mongo en ligne :

https://mongoplayground.net/
