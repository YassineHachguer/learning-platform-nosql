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
