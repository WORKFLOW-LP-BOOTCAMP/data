# Requête spéciale comprendre

```js
db.restaurants.find({
  "grades": {
    $not: {
      $elemMatch: {
        grade: { $ne: "A" }
      }
    }
  }
}, { "grades.grade": 1 })
```

- Explication

1. **$elemMatch** :
   ```js
   {
     grade: { $ne: "A" }
   }
   ```

L'opérateur `$elemMatch` est utilisé pour faire correspondre au moins un élément dans un tableau.

Ici, il est utilisé pour chercher dans le tableau `grades` un élément où `grade` n'est pas égal à "A" (`$ne` signifie "not equal" ou "n'est pas égal à").

Si nous utilisions simplement cette condition, cela trouverait tous les documents où au moins un grade n'est pas "A".

2. **$not** :
   ```js
   $not: {
     $elemMatch: {
       grade: { $ne: "A" }
     }
   }
   ```
L'opérateur `$not` est appliqué à la condition `$elemMatch`. Il inverse le résultat de cette condition. Donc, au lieu de trouver des documents où il y a un grade qui n'est pas "A", il trouve des documents où il n'y a **pas** de grade qui n'est pas "A".

En d'autres termes, la condition complète `{
    $not: {
    $elemMatch: {
        grade: { $ne: "A" }
    }
    }
}` signifie "trouver des documents où il n'y a **pas** d'élément dans `grades` dont `grade` n'est pas "A"". Cela revient à dire que tous les éléments dans le tableau `grades` doivent avoir `grade` égal à "A".

Cette requête recherche donc tous les documents où le tableau `grades` ne contient que des éléments dont le champ `grade` est "A".
