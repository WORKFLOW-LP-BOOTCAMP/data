# Exercices aggregate

1. Calculez le nombre total de restaurants dans la collection.

```js

```

1. Trouvez le nombre de restaurants dans chaque quartier (borough).

2. Calculez la moyenne des scores des restaurants par quartier. Utilisez l'opérateur `$unwind` pour dépiler les grades pour effectuer les calculs.

```js

// juste pour comprendre ce qu'il fait avec  $unwind regarder le nombre de doc créés en fonction du nombre de grades.


db.restaurants.aggregate([
    { $unwind : "$grades"},
    {$match : { name :"Brunos On The Boulevard"} },
    { $project : { "grades" : 1 }}
])

db.restaurants.aggregate([
    { $unwind : "$grades"},
    { $group : {_id : "$borough" , avgScore : { $avg : "$grades.score" } }}
])

```

3. Calculez le nombre moyen de grades par restaurant. 

4. Trouvez le nombre de restaurants ayant obtenu une note "A" dans chaque quartier.

5. Calculez le score moyen de chaque restaurant.

6. Trouvez les cinq restaurants ayant le plus grand nombre de grades.

7. Calculez le nombre de restaurants par type de cuisine (cuisine).

8.  Trouvez le restaurant le mieux noté dans chaque quartier.

9.  Calculez le score moyen de chaque type de cuisine (cuisine).

10. Trouvez le quartier avec le plus grand nombre total de grades "A".

11. Calculez le nombre moyen de grades "A" par restaurant dans chaque quartier.

12. Trouvez les restaurants ayant un score moyen supérieur à 8.

13. Calculez le nombre moyen de grades pour chaque type de cuisine (cuisine).

15. Trouvez le nombre de restaurants ayant plus de 3 grades "B" dans chaque quartier.

16. Calculez le score moyen de chaque quartier pour chaque type de cuisine (cuisine).
