## Lancer un Docker avec CouchDB

Pour lancer un conteneur Docker avec CouchDB, suivez les étapes ci-dessous :

1. Exécutez la commande suivante pour lancer un conteneur Docker avec CouchDB :
    ```sh
    sudo docker run -d --name couchdb1 -e COUCHDB_USER=admin -e COUCHDB_PASSWORD=admin -p 5985:5984 couchdb
    ```

2. Vérifiez que CouchDB est en cours d'exécution en utilisant la commande `curl` :
    ```sh
    curl -X GET http://{username}:{password}@localhost:5984
    ```
3. couchDB propose un client graphique
    ```
    http://localhost:{port}/_utils
    ```

4. Pour arrêter le conteneur Docker CouchDB, exécutez la commande suivante :
    ```sh
    sudo docker stop couchdb1
    ```

5. Pour supprimer le conteneur Docker CouchDB, exécutez la commande suivante :
    ```sh
    sudo docker rm couchdb1
    ```

6. Pour créer une base de données \`films\` dans CouchDB, exécutez la commande suivante :
   \```sh
    curl -X PUT http://admin:admin@localhost:5984/films
    \```

7. Exécutez la commande suivante pour vérifier la base de données \`films\` :
      ```sh
      curl -X GET http://admin:admin@localhost:5984/films
      ```

8. Insertion d'un nouveau document dans la base de données films a partir du fichier "
   films_couchdb.json"
    ```sh
    curl -X POST http://admin:admin@localhost:5984/films -H "Content-Type: application/json" -d @films_couchdb.json
    ```

9. Exécutez la commande suivante pour vérifier que les données on bien été ajouté à la base de
   données \`films\` :
      ```sh
      curl -X GET http://admin:admin@localhost:5984/films
      ```

10. Exécutez la commande suivante pour obtenir les détails d'un film spécifique :
    ```sh
    curl -X GET http://admin:admin@localhost:5984/films/movie:10098
    ```

11. Créer une view sur l'interface graphique et
    Index name:
    new-view
    Map function:
    ```js
function (doc) {
    if (doc.docs && Array.isArray(doc.docs)) {
        doc.docs.forEach(function(movie) {
        emit(movie.year, movie.title);
      });
    }
}
```

vous devriez avoir la liste des films triés par année