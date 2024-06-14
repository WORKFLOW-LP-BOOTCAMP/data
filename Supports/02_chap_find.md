# Cours sur la méthode `find` de MongoDB

Les exercices de cette partie se trouve : 
Exercices [find](./Exercices/01_exercices_find.md) et utilise la base de données restaurants.json (restaurants de NY).

### Introduction à la méthode `find`

La méthode `find` est l'une des méthodes les plus couramment utilisées dans MongoDB pour interroger une collection et récupérer des documents qui correspondent à certains critères. Elle est utilisée pour effectuer des opérations de lecture dans une base de données MongoDB.

### Syntaxe de base de la méthode `find`

La syntaxe de base de la méthode `find` est la suivante :

```js
db.collection.find(<filtre>, <projection>)
```

- `db.collection` : spécifie la collection sur laquelle effectuer la recherche.
- `<filtre>` : spécifie les critères de filtrage pour sélectionner les documents à retourner. Il utilise la syntaxe des expressions de requête MongoDB.
- `<projection>` (optionnel) : spécifie les champs à inclure ou exclure des résultats de la recherche. Il utilise la syntaxe des projections de champs MongoDB.

### Fonctionnalités importantes de la méthode `find`

1. **Filtrage des documents** :
   - Vous pouvez filtrer les documents en utilisant divers opérateurs de comparaison (`$eq`, `$ne`, `$gt`, `$lt`, etc.) pour comparer les valeurs des champs.
   - Vous pouvez utiliser des opérateurs logiques (`$and`, `$or`, `$not`, `$nor`) pour combiner plusieurs conditions de filtrage.


   1. **`$eq` (Equal) :**
   Trouver tous les users (collections) dont l'âge est égal à 30.

   ```js
   db.users.find({"age": {$eq: 30}})
   ```

   2. **`$ne` (Not Equal) :**
   Trouver tous les users dont l'âge n'est pas égal à 30.

   ```js
   db.users.find({"age": {$ne: 30}})
   ```

   3. **`$gt` (Greater Than) :**
   Trouver tous les users dont l'âge est supérieur à 30.

   ```js
   db.users.find({"age": {$gt: 30}})
   ```

   4. **`$lt` (Less Than) :**
   Trouver tous les users dont l'âge est inférieur à 30.

   ```js
   db.users.find({"age": {$lt: 30}})
   ```

   5. **`$gte` (Greater Than or Equal) :**
   Trouver tous les users dont l'âge est supérieur ou égal à 30.

   ```js
   db.users.find({"age": {$gte: 30}})
   ```

   6. **`$lte` (Less Than or Equal) :**
   Trouver tous les users dont l'âge est inférieur ou égal à 30.

   ```js
   db.users.find({"age": {$lte: 30}})
   ```

   7. **`$in` :**
   Trouver tous les users dont le nom est soit "Alice Dupont" soit "Bob Martin".

   ```js
   db.users.find({"nom": {$in: ["Alice Dupont", "Bob Martin"]}})
   ```

   8. **`$nin` :**
   Trouver tous les users dont le nom n'est ni "Alice Dupont" ni "Bob Martin".

   ```js
   db.users.find({"nom": {$nin: ["Alice Dupont", "Bob Martin"]}})
   ```

   9. **`$and` :**
   Trouver tous les users dont l'âge est supérieur à 25 et dont la ville est "Paris".

   ```js
   db.users.find({
       $and: [
           {"age": {$gt: 25}},
           {"adresse.ville": "Paris"}
       ]
   })
   ```

   10. **`$or` :**
   Trouver tous les users dont l'âge est supérieur à 30 ou dont la ville est "Paris".

   ```js
   db.users.find({
       $or: [
           {"age": {$gt: 30}},
           {"adresse.ville": "Paris"}
       ]
   })
   ```

   11. **`$not` :**
   Trouver tous les users dont l'âge n'est pas supérieur à 30.

   ```js
   db.users.find({"age": {$not: {$gt: 30}}})
   ```

   12. **`$nor` :**
   Trouver tous les users dont l'âge n'est pas supérieur à 30 et dont la ville n'est pas "Paris".

   ```js
   db.users.find({
       $nor: [
           {"age": {$gt: 30}},
           {"adresse.ville": "Paris"}
       ]
   })
   ```

### Utilisation dans les exercices du cours

Pour résoudre les exercices proposés nous allons utilisé plusieurs fonctionnalités de la méthode `find` :

- Filtrage des documents en fonction des valeurs de champs spécifiques.
- Utilisation d'opérateurs logiques pour combiner plusieurs conditions de filtrage.
- Utilisation de projections de champs pour spécifier les champs à inclure dans les résultats.


### Exemples d'utilisation

1. **Filtrage simple** :
```js
// Trouver tous les restaurants à Brooklyn
db.restaurant.find({ borough: "Brooklyn" })
```

2. **Filtrage avec opérateurs** :
```js
// Trouver tous les restaurants avec un score supérieur à 20
db.restaurant.find({ "grades.score": { $gt: 20 } })
```

3. **Projection de champs** :
```js
// Trouver tous les restaurants et inclure seulement le nom et l'adresse
db.restaurant.find({}, { name: 1, "address.street": 1, _id: 0 })
```

### Filtrage avancé avec les opérateurs

Outre les opérateurs de comparaison de base, MongoDB offre une gamme d'opérateurs puissants pour effectuer un filtrage avancé :

- `$regex` : Permet d'effectuer une recherche par expression régulière.
- `$elemMatch` : Utilisé pour filtrer les documents contenant au moins un élément de tableau qui satisfait toutes les conditions spécifiées.
- `$exists` : Vérifie si un champ existe dans le document.
- `$type` : Vérifie le type d'un champ dans le document.

###  Indexation pour l'optimisation des requêtes `find`

L'indexation est cruciale pour optimiser les performances des requêtes `find` dans MongoDB. Voici quelques points importants à retenir :

- Les index peuvent être créés sur un ou plusieurs champs pour accélérer la recherche.
- Les index peuvent être simples (un champ) ou composés (plusieurs champs).
- Les index peuvent être uniques pour garantir l'unicité des valeurs d'un champ.
- Les index peuvent être géospatiaux pour prendre en charge les données de localisation.

### Utilisation de l'agrégation avec `find`

Bien que nous n'en ayons pas parlé dans le cours initial, l'utilisation de l'agrégation avec la méthode `find` est une approche avancée mais puissante pour manipuler les données :

- L'opération `$lookup` permet de joindre des données provenant de plusieurs collections.
- Les opérations de pipeline d'agrégation (`$match`, `$group`, `$project`, etc.) peuvent être utilisées avec `find` pour effectuer des calculs avancés et des transformations de données.

###  Performance et optimisation des requêtes `find`

Pour assurer des performances optimales de vos requêtes `find`, voici quelques bonnes pratiques à suivre :

- Utilisez les index appropriés pour les champs souvent utilisés dans les requêtes.
- Limitez la taille des résultats en utilisant la projection de champs pour exclure les champs inutiles.
- Évitez les opérations coûteuses, comme les expressions régulières, chaque fois que possible.
- Surveillez et analysez les performances des requêtes à l'aide des outils de surveillance de MongoDB.