# CRUD - Exemple pratique

## 1. Create (Créer)

Créons une collection appelée `professors` et insérons quelques documents dans school si cela n'est pas déjà fait.

```js
// Connexion à la base de données
use school;

db.createCollection('professors')

// Insertion d'un document dans la collection professors
db.professors.insertOne({
    name: "John Doe",
    age: 21,
    courses: ["Math", "English", "Science"]
});

// Insertion de plusieurs documents dans la collection professors
db.professors.insertMany([
    {
        name: "Jane Smith",
        age: 22,
        courses: ["Math", "History", "Biology"]
    },
    {
        name: "Alice Brown",
        age: 23,
        courses: ["English", "Philosophy", "Art"]
    }
]);
```

## 2. Read (Lire)

Lecture de documents dans la collection `professors`.

```js
// Trouver un document avec un nom spécifique
db.professors.findOne({ name: "John Doe" });

// Trouver tous les documents dans la collection professors
db.professors.find().pretty();

// Trouver tous les étudiants âgés de plus de 21 ans
db.professors.find({ age: { $gt: 21 } }).pretty();
```

## 3. Update (Mettre à jour)

Mettons à jour des documents dans la collection `professors`.

```js
// Mettre à jour un document en ajoutant un nouveau cours
db.professors.updateOne(
    { name: "John Doe" },
    { $push: { courses: "Physics" } }
);

// Mettre à jour plusieurs documents pour augmenter l'âge de tous les étudiants de 1 an
db.professors.updateMany(
    {},
    { $inc: { age: 1 } }
);

// Remplacer un document entier
db.professors.replaceOne(
    { name: "Alice Brown" },
    {
        name: "Alice Brown",
        age: 24,
        courses: ["Literature", "Art"]
    }
);
```

## 4. Delete (Supprimer)

Suppression de documents dans la collection `professors`.

```js
// Supprimer un document avec un nom spécifique
db.professors.deleteOne({ name: "Jane Smith" });

// Supprimer tous les documents où l'âge est supérieur à 22
db.professors.deleteMany({ age: { $gt: 22 } });

// Supprimer tous les documents de la collection professors
db.professors.deleteMany({});
```

## En résumé

1. **Create** : Utilise `insertOne` pour insérer un seul document et `insertMany` pour insérer plusieurs documents.
2. **Read** : Utilise `findOne` pour trouver un seul document, `find` pour trouver plusieurs documents, avec des critères de recherche.
3. **Update** : Utilise `updateOne` pour mettre à jour un seul document, `updateMany` pour mettre à jour plusieurs documents, et `replaceOne` pour remplacer entièrement un document.
4. **Delete** : Utilise `deleteOne` pour supprimer un seul document et `deleteMany` pour supprimer plusieurs documents.

