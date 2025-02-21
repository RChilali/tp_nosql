# Cours sur MongoDB basé sur le TP2

## 1. Introduction à MongoDB

MongoDB est une base de données NoSQL orientée documents. Contrairement aux bases relationnelles, elle stocke les données sous forme de documents JSON (BSON en interne). MongoDB est très adapté aux applications modernes qui ont besoin de flexibilité et de scalabilité.

### Pourquoi utiliser MongoDB ?

- Stockage flexible et sans schéma rigide.
- Haute performance pour les requêtes complexes.
- Facile à mettre à l'échelle.

### Installation et Configuration

1. **Installer MongoDB** :
   - Linux/macOS : via `brew` ou `apt`.
   - Windows : via le site officiel.
2. **Lancer MongoDB** :
   ```bash
   mongod --dbpath /chemin/vers/dossier
   ```
3. **Accéder au shell MongoDB** :
   ```bash
   mongo
   ```

## 2. Importation et vérification des données

### Importer une collection

Pour importer une collection JSON dans MongoDB :

```bash
mongoimport --db lesfilms --collection films --file films.json --jsonArray
```

### Vérifier l'importation

Vérifier que les données sont bien importées en comptant le nombre de documents :

```javascript
db.films.count()
```

## 3. Comprendre la structure d'un document

MongoDB stocke les données sous forme de documents JSON. Pour voir un exemple de document :

```javascript
db.films.findOne()
```

Cela affiche un film avec ses attributs (titre, genre, année, acteurs, etc.).

## 4. Interroger la base de données

### Requêtes simples

Afficher tous les documents :

```javascript
db.films.find()
```

Afficher la liste des films d'action :

```javascript
db.films.find({ genre: "Action" })
```

### Filtres et projections

Afficher uniquement les titres des films d'action :

```javascript
db.films.find({ genre: "Action" }, { titre: 1, _id: 0 })
```

## 5. Requêtes avancées

### Rechercher selon plusieurs critères

- Films d'action produits en France :
  ```javascript
  db.films.find({ genre: "Action", pays: "France" })
  ```
- Films d'action produits en France en 1963 :
  ```javascript
  db.films.find({ genre: "Action", pays: "France", annee: 1963 })
  ```

### Conditions complexes

- Films d'action avec une note supérieure à 10 :

  ```javascript
  db.films.find({ genre: "Action", note: { $gt: 10 } })
  ```

- Films contenant certains artistes :

  ```javascript
  db.films.find({ acteurs: { $in: ["artist:4", "artist:18", "artist:11"] } })
  ```

- Films sans résumé :

  ```javascript
  db.films.find({ resume: { $exists: false } })
  ```

## 6. Manipulation des données

### Ajouter un document

```javascript
db.films.insertOne({ titre: "Nouveau Film", genre: "Drame", annee: 2024 })
```

### Mettre à jour un document

```javascript
db.films.updateOne({ titre: "Nouveau Film" }, { $set: { genre: "Comédie" } })
```

### Supprimer un document

```javascript
db.films.deleteOne({ titre: "Nouveau Film" })
```

## 7. Conclusion

MongoDB est un outil puissant et flexible pour stocker et manipuler des données. Ce cours vous permet d'avoir une base solide pour exploiter ses capacités.


