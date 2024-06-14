# MongoDB 

## Flexibilité des schéma

Imaginez que vous développiez une application pour gérer un catalogue de produits. Avec une base de données relationnelle traditionnelle, vous devez définir un schéma fixe pour vos tables avant d'insérer des données. Par exemple, une table "produits" pourrait ressembler à ceci :

| id | nom         | prix | description    |
|----|-------------|------|----------------|
| 1  | T-shirt     | 20   | Un t-shirt...  |
| 2  | Chaussures  | 50   | Des chaussures |

Si vous voulez ajouter un champ "couleur" pour certains produits, vous devez modifier le schéma de la table pour ajouter cette nouvelle colonne.

Avec MongoDB, ce processus est beaucoup plus flexible. Vous pouvez stocker des documents JSON dans une collection sans schéma fixe. Voici un exemple de documents dans une collection "produits" :

```json
{
  "_id": 1,
  "nom": "T-shirt",
  "prix": 20,
  "description": "Un t-shirt confortable"
}
```

```json
{
  "_id": 2,
  "nom": "Chaussures",
  "prix": 50,
  "description": "Des chaussures de sport",
  "couleur": "rouge"
}
```

Dans cet exemple, le premier document ne contient pas de champ "couleur", tandis que le second document en contient un. MongoDB permet cette flexibilité sans nécessiter de modification du schéma ou de migration des données. Vous pouvez facilement ajouter de nouveaux champs à certains documents sans affecter les autres, ce qui permet une adaptation rapide aux besoins changeants de l'application.

### Ajout d'un Nouveau Produit

Pour illustrer encore cette flexibilité, ajoutons un nouveau produit qui a encore plus de champs spécifiques :

```json
{
  "_id": 3,
  "nom": "Casquette",
  "prix": 15,
  "description": "Une casquette ajustable",
  "couleur": "bleu",
  "taille": "M",
  "stock": 100
}
```

Ici, le nouveau produit "Casquette" inclut les champs "couleur", "taille" et "stock" que les autres produits n'ont pas nécessairement. Vous pouvez ajouter autant de champs que nécessaire pour chaque produit sans aucune contrainte de schéma.

### Schéma SQL Relationnel

Dans un schéma SQL relationnel, les données sont organisées en tables avec des lignes et des colonnes pré-définies. Chaque table représente une entité, et chaque colonne représente un attribut de cette entité. Les relations entre les entités sont établies à l'aide de clés étrangères.

Exemple de schéma SQL relationnel pour une application de blog :

**Table "Articles" :**

| id | titre        | contenu       | auteur_id |
|----|--------------|---------------|-----------|
| 1  | Premier Post | Lorem ipsum.. | 123       |
| 2  | Deuxième Post| Dolor sit amet| 456       |

**Table "Auteurs" :**

| id   | nom      | email           |
|------|----------|-----------------|
| 123  | John Doe | john@example.com|
| 456  | Jane Doe | jane@example.com|

Dans cet exemple, la table "Articles" a une colonne "auteur_id" qui est une clé étrangère faisant référence à la table "Auteurs".

### Documents Imbriqués en MongoDB

En MongoDB, les données sont stockées sous forme de documents JSON dans des collections. Les documents peuvent contenir des champs imbriqués et être structurés de manière plus complexe qu'une simple table.

Exemple de documents imbriqués pour une application de blog en MongoDB :

**Collection "Articles" :**

```json
{
  "_id": 1,
  "titre": "Premier Post",
  "contenu": "Lorem ipsum...",
  "auteur": {
    "id": 123,
    "nom": "John Doe",
    "email": "john@example.com"
  }
}
```

```json
{
  "_id": 2,
  "titre": "Deuxième Post",
  "contenu": "Dolor sit amet",
  "auteur": {
    "id": 456,
    "nom": "Jane Doe",
    "email": "jane@example.com"
  }
}
```

Dans ces exemples, chaque document dans la collection "Articles" contient un champ "auteur" qui est lui-même un document imbriqué contenant les détails de l'auteur. Il n'y a pas de schéma fixe pour les documents, et chaque document peut avoir une structure différente selon les besoins de l'application.

### Comparaison

La principale différence entre les deux approches réside dans la flexibilité du schéma. En SQL relationnel, le schéma est rigide et pré-défini, ce qui signifie que toute modification nécessite une altération de la structure de la table. En revanche, en utilisant des documents imbriqués en MongoDB, le schéma est plus flexible et peut évoluer avec les besoins de l'application sans nécessiter de modifications de la structure de la collection. Cela permet une plus grande adaptabilité aux changements et une conception de données plus intuitive dans MongoDB.