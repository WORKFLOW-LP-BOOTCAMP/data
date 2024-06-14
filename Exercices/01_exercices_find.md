# Exercices find

1. Trouver tous les restaurants à Brooklyn

```js
db.restaurants.find({ borough : "Brooklyn" })
db.restaurants.find({ borough : "Brooklyn" }, {_id : 0, name : 1})
```

1. Trouver tous les restaurants qui servent de la cuisine américaine :

```js
db.restaurants.find({ cuisine : "American" }, {_id : 0, name : 1})
```

2. Trouver tous les restaurants qui ont reçu une note "A" :

```js
db.restaurants.find({ "grades.grade" : "A" }, {_id : 0, name : 1})


db.restaurants.find({"grades.grade" :{ $not : { $nin : ["A"]} } }, {_id : 0, name : 1, "grades.grade" : 1 })
```

La question que des notes grades.grade A est plus technique...

```js

db.restaurants.find({
  "grades": {
      $elemMatch: {
        grade: { $ne: "A" }
      }
  }
}, { "grades.grade" : 1})

db.restaurants.find({
  "grades": {
    $not: {
      $elemMatch: {
        grade: { $ne: "A" }
      }
    }
  }
}, { "grades.garde" : 1})
```

1. Trouver tous les restaurants qui ont obtenu un score supérieur à 10 :

```js
db.restaurants.find({ "grades.score" : {$gt : 10 } }, {_id : 0, name : 1})
```

4. Trouver tous les restaurants qui ont reçu une note "A" avec un score de 5 :
   
```js
db.restaurants.find({ 
    $and : [
        {"grades.grade" : "A"},
        {"grades.score" : 5 },
    ]
 }, {_id : 0, name : 1, "grades.grade" : 1 , "grades.score" : 1})
```

5. Trouver tous les restaurants qui ont une adresse dans la rue "Stillwell Avenue" :

```js
db.restaurants.find({ "address.street" : "Stillwell Avenue" }, {_id : 0, name : 1})

```

6. Trouver tous les restaurants qui ont été inspectés en 2014 (nous verrons une requête avec l'agrégation pour obtenir uniquement les restaurants inspectés en 2014)

```js
db.restaurants.find({ "grades.date" : { $gte : ISODate('2014-01-01') , $lt: ISODate('2015-01-01') } },
{_id : 0, name : 1, "grades.date" : 1}
)
```

7. Trouver tous les restaurants qui ont obtenu une note "A" après 2010 :

```js
db.restaurants.find({ "grades.grade" :  "A" , "grades.date" : { $gte : ISODate('2011-01-01' )} },
{_id : 0, name : 1, "grades.date" : 1})
```

1. Trouver tous les restaurants qui ont un code postal (zipcode) égal à "11224" :

```js
db.restaurants.find({ "address.zipcode" :  "11224" },
{_id : 0, name : 1, "address" : 1})
```

1.  Trouver tous les restaurants dont le nom contient le mot "Riviera" :

```js
db.restaurants.find({ "name" :  { $regex : /Riviera/} },{_id : 0, name : 1 } )

// dont les noms des restaurants commencent par Riviera : ^
db.restaurants.find({ "name" :  { $regex : /^Riviera/} },{_id : 0, name : 1 } )

// insensible à la casse avec l'option i 
db.restaurants.find({ "name" :  { $regex : /^Riviera/i} },{_id : 0, name : 1 } )

```

## Deuxième liste

1. Trouvez tous les restaurants qui ont reçu une note "A" pour au moins un de leurs grades et une note "B" pour au moins un autre grade.

2. Trouvez tous les restaurants qui n'ont jamais reçu une note "C" dans leurs grades.

3. Trouvez tous les restaurants qui ont reçu une note "A" pour leur dernier grade.

4. Trouvez tous les restaurants qui ont reçu une note "A" dans leur premier grade et une note "B" dans leur dernier grade.

5. Trouvez tous les restaurants qui ont reçu une note "A" pour leur premier grade et une note "B" pour leur deuxième grade, mais pas pour leur troisième grade.

6. Trouvez tous les restaurants qui ont reçu une note "A" pour tous leurs grades et dont la somme des scores des grades est supérieure à 30.

7. Trouvez tous les restaurants qui ont reçu une note "A" pour au moins un de leurs grades et dont la moyenne des scores des grades est supérieure à 8.

8. Trouvez tous les restaurants qui ont reçu une note "A" pour tous leurs grades et dont la différence entre le score de leur premier et dernier grade est supérieure à 5.

9. Trouvez tous les restaurants qui ont reçu une note "A" pour tous leurs grades et dont la moyenne des scores des grades est supérieure à la moyenne des scores de tous les restaurants.
   
## Exercices sur la partie approndissement

1. Trouvez tous les restaurants dont le nom commence par "Pizza" en utilisant une expression régulière. rmq : filtrage avec `$regex`

1. Trouvez tous les restaurants dans un rayon de 5 km d'un point géographique donné (latitude, longitude) en utilisant un index géospatial sur les coordonnées.

1. Trouvez tous les restaurants et incluez uniquement les champs `name`, `borough` et `address` dans les résultats.

1. Trouvez tous les restaurants avec un nom contenant "Sushi" et dont le score moyen est supérieur à 7, en utilisant un index textuel pour accélérer la recherche du nom et une opération d'agrégation `$group` suivie d'une opération de filtrage avec `find` pour le calcul du score moyen.
