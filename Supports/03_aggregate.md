# Aggregate

Les exercices de cette partie se trouvent : 
1. Exercices [aggregate](./Exercices/02_exercices_aggregate.md) et utilise la base de données restaurants.json (restaurants de NY).

1. Exercices complémentaires [aggregate](./Exercices/03_exercices_aggregate.md) 

### Introduction à l'agrégation

L'opération d'agrégation `aggregate` de MongoDB permet d'effectuer des calculs avancés et des transformations de données sur une collection. Elle permet de réaliser des opérations telles que le regroupement, le tri, le calcul de moyennes, de totaux, etc., sur les données stockées.

### Syntaxe de base de l'agrégation

La syntaxe de base de l'opération d'agrégation `aggregate` est la suivante :

```javascript
db.collection.aggregate([pipeline])
```

- `db.collection` : spécifie la collection sur laquelle effectuer l'opération d'agrégation.
- `pipeline` : une séquence de plusieurs étapes (stages) qui spécifient les différentes opérations d'agrégation à appliquer aux données. Chaque étape peut inclure un ou plusieurs opérateurs d'agrégation.

## La notion de pipe avec le framework aggregate

La notion de pipe avec l'agrégation MongoDB est un concept clé qui permet de créer des pipelines de traitement de données flexibles et puissants. 

Un pipeline d'agrégation est une séquence d'opérations de traitement de données qui sont exécutées séquentiellement sur les documents dans une collection MongoDB. 

Chaque opération de traitement de données dans le pipeline d'agrégation est représentée par un stade (stage) distinct.

### Structure d'un Pipeline d'Agrégation

Un pipeline d'agrégation MongoDB est composé de plusieurs stades (stages) qui sont liés ensemble par des tuyaux (pipes), d'où le terme "pipe". Chaque stade du pipeline d'agrégation effectue une opération spécifique sur les documents qui lui sont fournis en entrée et transmet les résultats au stade suivant.

### Illustration avec un Schéma

Voici un exemple simple de pipeline d'agrégation avec trois stades :

```plaintext
[    Stage 1   ]   [    Stage 2   ]   [    Stage 3   ]
    $match          $group & $sort       $project
```

1. **Stage 1 - `$match` :**
   - Filtre les documents en fonction d'un critère spécifique.
   - Seuls les documents correspondant au critère sont transmis au stade suivant.

2. **Stage 2 - `$group` & `$sort` :**
   - Regroupe les documents en fonction d'une clé spécifique.
   - Effectue une agrégation sur les documents regroupés (par exemple, comptage, somme, moyenne).
   - Trie les résultats selon un ordre spécifié.

3. **Stage 3 - `$project` :**
   - Projette les champs spécifiques des documents en sortie du pipeline.
   - Permet de renommer, ajouter ou supprimer des champs dans les documents en sortie.

### Fonctionnement du Pipeline

Lorsque vous exécutez un pipeline d'agrégation, chaque document de la collection passe à travers chaque stade du pipeline séquentiellement, avec les résultats de chaque stade servant d'entrée pour le stade suivant. Cela permet une transformation progressive et flexible des données, en appliquant différentes opérations à chaque étape du processus.

### Avantages des Pipelines d'Agrégation

- **Flexibilité :** Vous pouvez combiner différents stades pour créer des pipelines d'agrégation complexes qui répondent à des besoins spécifiques d'analyse de données.
- **Puissance :** Les pipelines d'agrégation permettent d'effectuer des opérations avancées sur les données, telles que l'agrégation, le regroupement, le tri, la projection et bien plus encore.
- **Performance :** En utilisant des pipelines d'agrégation efficaces, vous pouvez optimiser les performances des requêtes en réduisant le nombre de documents à traiter à chaque étape du pipeline.

### Étapes (stages) de l'agrégation

1. **`$match`** : Filtre les documents en fonction de critères spécifiques.
1. **`$project`** : Permet de sélectionner, d'exclure ou de renommer des champs, ainsi que de créer de nouveaux champs calculés.
1. **`$group`** : Regroupe les documents en fonction de certaines clés et effectue des calculs d'agrégation sur les données groupées.
1. **`$sort`** : Trie les documents en fonction des valeurs de champs spécifiques.
1. **`$limit`** : Limite le nombre de documents renvoyés par l'agrégation.
1. **`$skip`** : Ignore un certain nombre de documents au début de la sortie.
2. **`$unwind`** : Décompose les tableaux dans les documents en un document par élément du tableau.


### Utilisation dans des cas pratiques

1. **Analyse de données** : Calcul de moyennes, totaux, nombres de documents, etc.
2. **Préparation de données** : Transformation et nettoyage des données avant analyse.
3. **Génération de rapports** : Création de rapports personnalisés à partir des données.

### L'attribut spécial `_id` dans MongoDB Agrégation

En MongoDB, l'attribut `_id` est un identifiant unique par défaut pour chaque document dans une collection. 

⚠️ Cependant, lors des opérations d'agrégation, l'attribut `_id` prend un rôle différent et essentiel, surtout avec l'opérateur `$group`.

#### Exemple de l'opération d'agrégation `$group`

```js
db.sales.aggregate([
    { $match: { date: { $gte: ISODate("2022-01-01"), $lt: ISODate("2023-01-01") } } }, // Filtre par date
    { $group: { _id: "$product", totalSales: { $sum: "$quantity" } } } // Regroupe par produit et calcule le total des ventes
])
```

#### Explication de l'attribut spécial `_id` avec `$group`

##### 1. **Filtrage initial avec `$match`**
La première étape dans le pipeline d'agrégation est le filtrage des documents basés sur une condition de date. Cela limite les documents aux ventes de l'année 2022.

```js
{ $match: { date: { $gte: ISODate("2022-01-01"), $lt: ISODate("2023-01-01") } } }
```

##### 2. **Grouper avec `$group`**
L'étape suivante utilise l'opérateur `$group` pour regrouper les documents selon un critère spécifique, qui dans ce cas est le champ `product`.

```js
{ $group: { _id: "$product", totalSales: { $sum: "$quantity" } } }
```

- **`_id`** : Dans l'opération `$group`, `_id` devient le champ sur lequel nous regroupons les documents. Chaque groupe formé aura une clé `_id` égale à la valeur du champ spécifié. Dans cet exemple, les documents sont regroupés par le champ `product`. Cela signifie que tous les documents ayant la même valeur pour `product` seront agrégés ensemble dans un groupe.
- **`totalSales`** : Ce champ est créé pour chaque groupe et utilise l'opérateur `$sum` pour additionner les valeurs du champ `quantity` de tous les documents appartenant à ce groupe.

#### Exemple de documents et résultat

##### Documents d'entrée (exemple simplifié)

```json
[
    { "_id": 1, "product": "Apple", "quantity": 10, "date": ISODate("2022-05-01") },
    { "_id": 2, "product": "Apple", "quantity": 20, "date": ISODate("2022-06-01") },
    { "_id": 3, "product": "Orange", "quantity": 15, "date": ISODate("2022-05-01") }
]
```

##### Résultat après agrégation

```json
[
    { "_id": "Apple", "totalSales": 30 },
    { "_id": "Orange", "totalSales": 15 }
]
```

🚀 Dans le contexte de l'opérateur `$group`, l'attribut spécial `_id` est utilisé pour définir le critère de regroupement. Chaque groupe formé aura une clé `_id` représentant la valeur du champ spécifié pour le regroupement, permettant ainsi des opérations d'agrégation comme `$sum`, `$avg`, `$min`, `$max`, etc., sur les documents appartenant à chaque groupe.

### Quelques exemples supplémentaires

#### 01 
Voici un exemple simple de l'opération d'agrégation `aggregate` :

```js
db.sales.aggregate([
    { $match: { date: { $gte: ISODate("2022-01-01"), $lt: ISODate("2023-01-01") } } }, // Filtre par date
    { $group: { _id: "$product", totalSales: { $sum: "$quantity" } } } // Regroupe par produit et calcule le total des ventes
])
```

Cet exemple filtre les ventes pour une année donnée, puis groupe les ventes par produit et calcule le total des ventes pour chaque produit.

#### 02 

1. Filtrer uniquement les restaurants qui ont été inspectés en 2014

```js
db.restaurants.aggregate([
  // Unwind pour dérouler le tableau des grades
  { $unwind: "$grades" },
  // Filtrer les grades qui ont une date en 2014
  { $match: { "grades.date": { $gte: ISODate("2014-01-01"), $lt: ISODate("2015-01-01") } } },
  // Regrouper par _id du restaurant pour rétablir la structure initiale
  { $group: { _id: "$_id", name: { $first: "$name" }, address: { $first: "$address" }, borough: { $first: "$borough" }, cuisine: { $first: "$cuisine" }, grades: { $push: "$grades" } } }
])

```

####  Utilisation de l'opération d'agrégation `$unwind` :
- L'opération `$unwind` est utilisée pour dérouler un tableau (array) contenu dans un document MongoDB.
- Dans notre cas, nous avons un tableau de grades dans chaque document de restaurant. Nous devons dérouler ce tableau pour pouvoir filtrer les dates d'inspection.

```js
const students = [
    {
    "name": "Alice",
    "subjects": ["Math", "Science", "History"]
    },
    {
    "name": "Bob",
    "subjects": ["Literature", "Math"]
    },
    {
    "name": "Charlie",
    "subjects": []
    }
]

db.students.inserMany(students);

db.students.aggregate([
  { $unwind: "$subjects" }
])
//
{ "_id": 1, "name": "Alice", "subjects": "Math" }
{ "_id": 1, "name": "Alice", "subjects": "Science" }
{ "_id": 1, "name": "Alice", "subjects": "History" }
{ "_id": 2, "name": "Bob", "subjects": "Literature" }
{ "_id": 2, "name": "Bob", "subjects": "Math" }
//
```

- L'objectif est de décomposer les tableaux subjects pour que chaque matière soit dans un document distinct.

Si un document n'a pas de subject on peut quand crée une ligne sans subject :

```js
db.students.aggregate([
    { 
        $unwind: { 
            path: "$subjects", 
            preserveNullAndEmptyArrays: true 
        }
    }
])
```

#### Filtrage des dates d'inspection avec `$match` :
- Une fois que nous avons déroulé le tableau des grades, nous utilisons l'opération `$match` pour filtrer les éléments en fonction de critères spécifiques.
- Nous utilisons `$match` pour sélectionner uniquement les documents où la date d'inspection est comprise entre le 1er janvier 2014 et le 1er janvier 2015.

#### Regroupement des résultats avec `$group` :
- Après avoir filtré les documents en fonction des dates d'inspection, nous utilisons l'opération `$group` pour regrouper les résultats.
- Dans notre cas, nous voulons regrouper les résultats par `_id` du restaurant et rétablir la structure initiale du document de restaurant en conservant les informations telles que le nom, l'adresse, le quartier, la cuisine, et les grades.
