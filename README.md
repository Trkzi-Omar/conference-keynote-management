# **Rapport de Projet**:  conference-keynote-management

## **1. Introduction**
Ce rapport présente le développement d'une application de gestion des conférences et des keynotes, basée sur une architecture microservices. Le but de ce projet est de fournir un système modulaire, résilient et extensible pour la gestion des conférences, des keynotes et des avis associés.

---

## **2. Objectifs du Projet**
- **Modularité** : Chaque service est autonome et gérable indépendamment.
- **Scalabilité** : Capacité à gérer de nombreuses conférences, keynotes et avis.
- **Sécurité** : Mise en place d'une authentification sécurisée à l'aide de Keycloak (OAuth2 et OIDC).
- **Résilience** : Utilisation de Resilience4J pour la tolérance aux pannes.
- **Documentation** : Génération d’une documentation des API REST avec Swagger (OpenAPI).

---

## **3. Architecture Technique**

### **3.1. Diagramme d'Architecture**
![Diagramme d'Architecture](./architecture_diagram/architecture_diagram.png)

### **3.2. Composants Principaux**

| **Service**            | **Rôle**                                                |
|-----------------------|-------------------------------------------------------|
| **Keynote Service**    | Gestion des keynotes (CRUD)                             |
| **Conference Service** | Gestion des conférences et des avis (CRUD)             |
| **Gateway Service**    | Point d'entrée des requêtes, routage des API           |
| **Discovery Service**  | Enregistrement et découverte des services (Eureka)    |
| **Config Service**     | Gestion des configurations centralisées (Spring Cloud Config) |
| **Keycloak**           | Authentification et autorisation avec OAuth2 et OIDC  |
| **Base de Données**    | Stockage des données (PostgreSQL)                      |

---

## **4. Technologies Utilisées**

| **Technologie**         | **Rôle**                          |
|------------------------|-------------------------------------|
| **Spring Boot**         | Création de microservices         |
| **Spring Cloud**        | Eureka, Config Server, Gateway    |
| **PostgreSQL**          | Base de données relationnelle    |
| **Angular**             | Interface utilisateur (Frontend) |
| **Keycloak**            | Gestion de la sécurité           |
| **Docker & Docker Compose** | Conteneurisation et orchestration |
| **Maven**               | Gestion des dépendances          |
| **Swagger**             | Documentation des API REST      |

---

## **5. Développement des Services**

### **5.1. Keynote Service**
- **Entités** : Keynote (id, nom, prénom, email, fonction)
- **Endpoints** :
  - `GET /keynotes` : Récupération de tous les keynotes
  - `POST /keynotes` : Création d'un nouveau keynote
  - `PUT /keynotes/{id}` : Mise à jour d'un keynote existant
  - `DELETE /keynotes/{id}` : Suppression d'un keynote

### **5.2. Conference Service**
- **Entités** : Conference (id, titre, type, date, durée, inscrits, score), Review (id, date, texte, stars)
- **Endpoints** :
  - `GET /conferences` : Récupération de toutes les conférences
  - `POST /conferences` : Création d'une nouvelle conférence
  - `POST /conferences/{id}/reviews` : Ajout d'un avis sur une conférence

### **5.3. Gateway Service**
- **Fonctionnalités** :
  - Point d'accès unique à l'ensemble des microservices
  - Routage des requêtes HTTP vers les services appropriés
  - Circuit breaker (Resilience4J) pour la tolérance aux pannes

### **5.4. Discovery Service**
- **Fonctionnalités** :
  - Registre des services (Eureka) 
  - Permet la découverte automatique des microservices actifs

### **5.5. Config Service**
- **Fonctionnalités** :
  - Fourniture centralisée des fichiers de configuration des services
  - Synchronisation des propriétés entre les microservices

---

## **6. Sécurité**

- **Authentification** : Keycloak est utilisé comme fournisseur d'identité (OAuth2 et OIDC).
- **Autorisation** : Les rôles et les permissions sont définis dans Keycloak.
- **Sécurisation des Endpoints** : Les requêtes au niveau des API sont protégées par des jetons d'accès (JWT).

---

## **7. Tests**

### **7.1. Tests Unitaires**
- Tests unitaires écrits pour les contrôleurs, les services et les DAO.
- Utilisation de **JUnit** et **Mockito**.

### **7.2. Tests d'Intégration**
- Les tests d'intégration vérifient la communication entre les services.
- Tests automatisés avec des bases de données H2.

### **7.3. Tests de Performance**
- Tests de montée en charge des services.

---

## **8. Déploiement**

### **8.1. Docker et Docker Compose**
- Chaque service a son propre Dockerfile.
- Un fichier **docker-compose.yml** orchestre l'exécution des conteneurs pour :
  - Keynote Service
  - Conference Service
  - Discovery Service
  - Gateway Service
  - Config Service
  - Keycloak
  - PostgreSQL

### **8.2. Commandes de Déploiement**
```bash
# Construction des images Docker
mvn clean package -DskipTests

docker-compose up --build
```

---

## **9. Documentation des API**
- **Swagger** est activé sur les services `keynote-service` et `conference-service`.
- Les endpoints sont documentés automatiquement à l'adresse :
  - `http://localhost:{port}/swagger-ui.html`

---

## **10. Améliorations Futures**
- **Améliorer la gestion des erreurs** : Ajouter des gestionnaires d'erreurs globaux pour les exceptions personnalisées.
- **Mise en cache** : Implémenter un cache Redis pour les requêtes fréquemment utilisées.
- **Notifications** : Ajouter un service de notifications pour informer les utilisateurs des mises à jour de la conférence.

---

## **11. Conclusion**
Le projet de gestion de conférences et keynotes a été conçu pour être modulaire, sécurisé et résilient. Grâce à une architecture basée sur les microservices, il permet une évolutivité et une flexibilité accrues. Chaque service est isolé, facile à maintenir et extensible, tout en garantissant une communication fluide à travers le Gateway et le Discovery Service. L'utilisation de Docker permet un déploiement efficace et reproductible dans n'importe quel environnement.

---

## **12. Annexes**
- **Lien du dépôt GitHub** : [Lien du projet](https://github.com/Trkzi-Omar/conference-keynote-management)
- **Exemples de commandes utiles** :
  ```bash
  # Lancer les conteneurs Docker
  docker-compose up --build
  
  # Voir les logs d'un service spécifique
  docker-compose logs -f keynote-service
  ```
