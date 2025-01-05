# Course Management System
Ce projet est une application backend pour la gestion des cours, incluant des fonctionnalités de création, mise à jour, suppression et récupération de données.

## Structure du Projet
- `config/`: Gestion des connexions (MongoDB, Redis) et variables d'environnement.
- `controllers/`: Logique métier des entités (exemple: `CourseController`).
- `routes/`: Points d'entrée API pour chaque ressource.
- `services/`: Utilitaires pour manipuler les données.
- `app.js`: Point d'entrée principal.

## Réponses aux Questions

### `config/db.js`
1. **Pourquoi créer un module séparé pour les connexions aux bases de données ?**
   - Pour centraliser la gestion des connexions, réduire la duplication de code, et faciliter la maintenance.
2. **Comment gérer proprement la fermeture des connexions ?**
   - En utilisant les événements système (`SIGTERM`) pour libérer les ressources avant l'arrêt de l'application.

### `config/env.js`
1. **Pourquoi est-il important de valider les variables d'environnement au démarrage ?**
- Pour s'assurer que l'application dispose de toutes les informations nécessaires à son bon fonctionnement, évitant ainsi des erreurs imprévues en cours d'exécution.

2. **Que se passe-t-il si une variable requise est manquante ?**
- L'application doit arrêter son exécution avec un message d'erreur explicite, pour éviter un comportement imprévisible.

### `controllers/courseController.js`
1. **Quelle est la différence entre un contrôleur et une route ?**
- Une route définit l'URL et le type de requête (GET, POST, etc.), tandis qu'un contrôleur contient la logique métier exécutée lorsque cette route est appelée.

2. **Pourquoi séparer la logique métier des routes ?**
- Pour améliorer la lisibilité, la maintenabilité et faciliter les tests unitaires des contrôleurs.

### `routes/courseRoutes.js`
1. **Pourquoi séparer les routes dans différents fichiers ?**
- Cela facilite la gestion et la lisibilité des routes, surtout dans les grandes applications avec de nombreux points d'entrée.

2. **Comment organiser les routes de manière cohérente ?**
- Grouper les routes par fonctionnalité ou module (par exemple, courseRoutes.js pour les cours).

### `services/mongoService.js`
1. **Pourquoi créer des services séparés ?**
- Cela permet de réutiliser la logique métier et de réduire la duplication dans les contrôleurs.

### `services/redisService.js`
1. **Comment gérer efficacement le cache avec Redis ?**
- En définissant des clés avec des durées d'expiration (TTL) et en invalidant les données obsolètes.

2. **Quelles sont les bonnes pratiques pour les clés Redis ?**
- Utiliser des noms de clés descriptifs et éviter les collisions en définissant un préfixe (ex. courses:<id>).

### `app.js`
1. **Comment organiser le point d'entrée de l'application ?**
- Initialiser les connexions, configurer les middlewares et monter les routes.

2. **Quelle est la meilleure façon de gérer le démarrage de l'application ?**
- En utilisant des blocs try-catch pour capturer et gérer les erreurs au démarrage.

### `.env`
1. **Quelles sont les informations sensibles à ne jamais commiter ?**
- Il ne faut jamais commettre de mots de passe, clés API, certificats, informations personnelles ou de configurations sensibles dans un dépôt Git. Utilisez un `.gitignore` et des gestionnaires de secrets pour sécuriser ces données.

2. **Pourquoi utiliser des variables d'environnement ?**
- Les variables d'environnement permettent de stocker des informations sensibles et des configurations spécifiques à l'environnement sans les inclure directement dans le code source. Cela améliore la sécurité, la portabilité et la flexibilité des applications en permettant de modifier ces valeurs sans toucher au code.


## Explication des Choix Techniques

### `src/app.js`

- **Initialiser les connexions aux bases de données** : Cette étape est cruciale pour s'assurer que l'application peut accéder aux données nécessaires. La connexion à MongoDB et Redis est initialisée avant de démarrer le serveur.
- **Configurer les middlewares Express** : Les middlewares permettent de gérer les requêtes et les réponses de manière modulaire. Leur configuration avant le démarrage du serveur garantit que toutes les requêtes passent par les middlewares.
- **Monter les routes** : Les routes définissent les points d'entrée de l'API. Les monter avant de démarrer le serveur assure que toutes les routes sont prêtes à traiter les requêtes dès que le serveur est en ligne.
- **Démarrer le serveur** : Cette étape démarre effectivement le serveur et le rend capable de recevoir des requêtes.
- **Implémenter la fermeture propre des connexions** : Gérer la fermeture propre des connexions aux bases de données garantit que les ressources sont correctement libérées et qu'il n'y a pas de fuites de ressources.

### `src/config/env.js`

- **Validation des variables d'environnement** : Valider les variables d'environnement au démarrage est essentiel pour s'assurer que toutes les configurations nécessaires sont présentes. Cela évite des erreurs à l'exécution dues à des configurations manquantes.
- **Lever une erreur explicative si une variable manque** : Cela permet de détecter rapidement les problèmes de configuration et de fournir des messages d'erreur clairs, facilitant ainsi le débogage.

### `src/services/redisService.js`

- **Fonction générique de cache** : Implémenter une fonction générique pour mettre en cache les données permet de réutiliser cette logique à différents endroits de l'application, améliorant ainsi la maintenabilité et la cohérence.
- **Exporter les fonctions utilitaires** : Exporter ces fonctions permet de les utiliser dans d'autres parties de l'application, favorisant la modularité et la réutilisation du code.

### `src/controllers/courseController.js`

- **Séparer la logique métier des routes** : Cela facilite la gestion du code en séparant les préoccupations. Les contrôleurs gèrent la logique métier tandis que les routes définissent les points d'entrée de l'API.
- **Implémenter la création d'un cours** : Utiliser les services pour la logique réutilisable garantit que la logique métier est centralisée et réutilisable à travers l'application.
- **Exporter les fonctions du contrôleur** : Exporter ces fonctions permet de les utiliser dans les routes, assurant ainsi une séparation claire entre la définition des routes et la logique métier.

### `src/services/mongoService.js`

- **Fonction générique de recherche par ID** : Implémenter une fonction générique pour rechercher par ID permet de réutiliser cette logique dans différentes parties de l'application, améliorant ainsi la maintenabilité et la cohérence.
- **Exporter les fonctions utilitaires** : Exporter ces fonctions permet de les utiliser dans d'autres parties de l'application, favorisant la modularité et la réutilisation du code.

### `src/config/db.js`

- **Créer un module séparé pour les connexions aux bases de données** : Cela centralise la logique de connexion, facilitant ainsi la gestion et la réutilisation des connexions à MongoDB et Redis.
- **Gérer proprement la fermeture des connexions** : Assurer une fermeture propre des connexions aux bases de données garantit que les ressources sont correctement libérées et qu'il n'y a pas de fuites de ressources.
- **Exporter les clients et fonctions utiles** : Exporter ces fonctions permet de les utiliser dans d'autres parties de l'application, assurant ainsi une utilisation cohérente des connexions aux bases de données.


