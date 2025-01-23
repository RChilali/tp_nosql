Pour réaliser ce TP, nous allons suivre les étapes suivantes :

1. **Introduction aux familles de systèmes de gestion de bases de données NoSQL** :
   - **Bases de données clé-valeur** : Ces bases de données stockent les données sous forme de paires clé-valeur. Elles sont très performantes pour les opérations de lecture et d'écriture simples. Exemples : Redis, Memcached.
   - **Bases de données orientées colonnes** : Ces bases de données stockent les données par colonnes plutôt que par lignes, ce qui les rend efficaces pour les requêtes analytiques. Exemples : Apache Cassandra, HBase.
   - **Bases de données orientées documents** : Ces bases de données stockent les données sous forme de documents (généralement JSON ou BSON). Elles sont flexibles et permettent de stocker des données semi-structurées. Exemples : MongoDB, CouchDB.
   - **Bases de données orientées graphes** : Ces bases de données sont conçues pour stocker et interroger des relations complexes entre les données. Elles sont idéales pour les applications nécessitant des connexions et des relations complexes. Exemples : Neo4j, ArangoDB.

    
2. **Présentation de Redis** :
   Redis est une base de données NoSQL de type clé-valeur, souvent utilisée comme cache, courtier de messages et file d'attente. Elle est connue pour sa performance élevée et sa flexibilité.


3. **Installation de Redis** :
   Voici les étapes pour installer Redis sur un système basé sur Debian (comme Ubuntu) :

   ```sh
   sudo apt update
   sudo apt install redis-server
   ```
   

4. **Utilisation de Redis** :
   - **Lancement du serveur Redis** :
     ```sh
     redis-server
     ```
   - **Connexion au client Redis** :
     ```sh
     redis-cli
     ```
   - **Exemples de commandes Redis** :
     ```sh
     SET demo "bonjour"
     GET demo
     ```
     
5. **Exemple avec Redis** :
- **Créer (Create)** :
  ```sh
  SET user:1 "Alice"
  ```
- **Lire (Read)** :
  ```sh
  GET user:1
  ```
- **Mettre à jour (Update)** :
  ```sh
  SET user:1 "Alice Updated"
  ```
- **Supprimer (Delete)** :
  ```sh
  DEL user:1
  ```
- **In/Décrémenter** :
  ```sh
  set 1mars 0
  incr 1mars
  decr 1mars
  ```   
- **TTl** :
  ```sh
    # Définir une clé avec une valeur
    set key "value"
    # Définir un délai d'expiration de 10 secondes pour la clé
    expire key 10
    # Vérifier le temps restant avant l'expiration de la clé
    ttl key
    # Supprimer la clé
    del key
    ```
- **List** :
  ```sh
  # Ajouter un élément à la liste 'mescours'
  RPUSH mescours "BDA"
  # Ajouter un autre élément à la liste 'mescours'
  RPUSH mescours "service web"
  # Obtenir tous les éléments de la liste 'mescours'
  LRANGE mescours 0 -1
  # Supprimer le premier élément de la liste 'mescours'
  LPOP mescours
  ```
- **Set** :
  ```sh
  # Ajouter un ou plusieurs membres à un ensemble
  SADD myset "membre1" "membre2"
  # Obtenir tous les membres d'un ensemble
  SMEMBERS myset
  # Supprimer un ou plusieurs membres d'un ensemble
  SREM myset "membre1"
  # Obtenir l'union de plusieurs ensembles
  SUNION myset otherset
  ```
  
- **Sorted Set** :
  ```sh
  # Ajouter un élément avec un score à un ensemble trié
  ZADD myzset 1 "membre1"
  # Obtenir les éléments d'un ensemble trié dans un intervalle de rang
  ZRANGE myzset 0 -1
  # Obtenir les éléments d'un ensemble trié dans un intervalle de rang en ordre décroissant
  ZREVRANGE myzset 0 -1
  # Obtenir le rang d'un élément dans un ensemble trié
  ZRANK myzset "membre1"
    ```
- **Hash** :   
    ```sh
    #Définir un champ et une valeur dans un hash** :
    HSET user:11 "nom" "Alice"
    #Obtenir tous les champs et valeurs d'un hash** :
    HGETALL user:11
    #Définir plusieurs champs et valeurs dans un hash** :
    HMSET user:4 "nom" "Bob" "age" "30"
     #Incrémenter la valeur d'un champ dans un hash** :
    HINCRBY user:4 "age" 1
     #Obtenir toutes les valeurs d'un hash** :
    HVALS user:4
    ```
- **Pub/Sub** :
    ```sh
    # S'abonner à un canal dans terminal 1
    subscribe mescours
    # Publier un message sur un canal dans terminal 2 ( devrait apparaitre sur terminal 1)
    publish mescours
    # S'abonner à des canaux correspondant à un motif
    psubscribe mes*
    ```

6.**Fonctionnalités supplémentaires de Redis** :
- **Transactions** : Redis permet d'exécuter une série de commandes de manière atomique.
  ```sh
  # Démarrer une transaction
  MULTI
  SET key1 "value1"
  SET key2 "value2"
  # Exécuter la transaction
  EXEC
  ```

- **Persistances** : Redis offre plusieurs options de persistance des données, comme les snapshots et les journaux d'appendices.
  ```sh
  # Sauvegarder la base de données dans un fichier RDB
  SAVE
  # Sauvegarder la base de données dans un fichier AOF
  BGREWRITEAOF
  ```

- **Scripts Lua** : Redis permet d'exécuter des scripts Lua pour des opérations complexes.
  ```sh
  # Exécuter un script Lua
  EVAL "return redis.call('SET', KEYS[1], ARGV[1])" 1 key "value"
  ```

- **Clustering** : Redis offre la possibilité de créer des clusters pour la mise à l'échelle horizontale.
  ```sh
  # Créer un cluster Redis
  redis-cli --cluster create 127.0.0.1:7000 127.0.0.1:7001 127.0.0.1:7002 --cluster-replicas 1
  ```

8.**Réponse à la question de la vidéo n°3** :

**Comment Redis gère-t-il les bases multiples :**

Redis utilise des bases de données numérotées (par défaut 16) accessibles via la commande `SELECT <numéro_de_base>`. Cependant, il est recommandé d'utiliser une seule base de données par instance pour des raisons de performance et de simplicité.

**Quelles sont les limites de la persistance :**

Redis offre deux mécanismes de persistance : RDB (snapshots) et AOF (journaux d'appendices). Les limites incluent la perte potentielle de données entre les snapshots en RDB et la taille croissante des fichiers AOF, nécessitant des réécritures périodiques pour maintenir les performances.
