# Introduction aux Microservices

## LES MICROSERVICES

### • De quoi parle-t-on ?
Les microservices sont une manière moderne de concevoir des applications. L'idée est de découper une grosse application en **petites briques autonomes**, spécialisées sur un seul domaine métier. Chaque brique peut être **développée, testée et déployée indépendamment**. Ensemble, elles forment une application cohérente.

Une bonne analogie : une entreprise avec plusieurs pôles (RH, logistique, compta...) où chaque équipe gère sa mission, en autonomie, dans un cadre organisé.

---

### • Les caractéristiques des microservices

- ✅ **Autonome** : déploiement, tests, mises à jour sans impacter les autres services  
  ⚒ Ex : Le microservice "Paiement" peut être mis à jour sans toucher au microservice "Panier".

- ✅ **Responsabilité unique** : chaque service est centré sur un domaine fonctionnel  
  📎 Ex : un service "Facturation", un autre "Commandes", un autre "Utilisateurs".

- ✅ **Communication via API** (REST, RabbitMQ, Kafka...)  
  ⟳ Ex : "Commande" notifie "Livraison" lorsqu'une commande est validée.

- ✅ **Scalabilité indépendante** : chaque service peut être répliqué selon sa charge  
  📈 Ex : pendant une promo, le service "Produits" passe à 5 instances.

- ✅ **Résilience** : si un service tombe, les autres continuent à fonctionner  
  💥 Ex : "Avis clients" plante, "Commande" continue normalement.

- ✅ **Base de données dédiée (souvent)** : évite les conflits  
  💣 Ex : "Utilisateurs" → PostgreSQL, "Paiement" → MongoDB.

> Cette architecture suit le principe "**High Cohesion / Loose Coupling**".

---

### • La différence entre un microservice et un Webservice

#### WebService :
- Interface technique exposée sur le réseau (REST ou SOAP)
- Utilisé pour fournir des données ou actions à un client (navigateur, appli mobile)

🧠 **Exemple :** 
> Une méthode `GET /produits` dans un monolithe Java EE → C'est un WebService, **pas** un microservice.

#### Microservice :
- Mini-application à part entière (logique, base, API)
- Autonome : déploiement, redémarrage, scalabilité isolés

🧠 **Exemple :** 
> Un service `ProduitService` avec Spring Boot :
> - Contrôleur REST `GET /produits`
> - Service métier `ProduitService`
> - Repository JPA + base PostgreSQL
> → C'est un microservice.

> **Tous les microservices exposent des WebServices, mais tous les WebServices ne sont pas des microservices.**

---

### • Le Cloud et les microservices

#### Le Cloud, c'est quoi ?
- Des serveurs accessibles à distance, hébergés par AWS, Azure, GCP...
- Vos applis tournent sur Internet, pas en local

#### Les 3 modèles Cloud :
- **IaaS** : serveurs à configurer soi-même (ex : EC2)
- **PaaS** : plateforme prête à l'emploi (ex : Heroku)
- **SaaS** : application clé-en-main (ex : Gmail)

#### Pourquoi le Cloud est idéal pour les microservices ?
- ♻ Déploiement rapide via CI/CD
- 📊 Scalabilité automatique selon le trafic
- ⚡️ Lancer/arrêter des instances à la volée
- 🔍 Monitoring, logs et supervision native

🧠 **Exemple :**
> Un service "Paiement" surchargé pendant les soldes → on passe de 2 à 10 instances en quelques secondes

---

## IMPLÉMENTER UN MICROSERVICE AVEC SPRING BOOT

- **Les étapes de conception** d'un microservice : identifier le domaine métier, créer l'entité, réfléchir aux endpoints REST, créer le repository et le service associé.
- **Création de projet avec Spring Initializr** : choisir les dépendances essentielles (Spring Web, Spring Data JPA, PostgreSQL Driver, Lombok...).
- **Implémentation d'un contrôleur REST** : créer une API REST (`@RestController`) pour gérer les opérations CRUD.
- **Configurer son environnement de développement** : utiliser IntelliJ ou VS Code, Postman pour tester les endpoints, et Docker pour la base de données si besoin.
- **Tester localement avec `localhost:8080`** : s'assurer que le projet tourne et que les endpoints REST fonctionnent.
- **Déployer avec Spring Boot + Spring Cloud** : pour gérer les microservices dans une architecture distribuée.
- **Activer CORS** si besoin avec `@CrossOrigin` pour permettre les appels depuis une appli front comme Angular.
- **Spring Boot CLI (optionnel)** : permet de générer rapidement un projet ou de lancer une appli Spring sans IDE.

---

## SPRING CLOUD ET L'ÉQUILIBRAGE DE CHARGE

- **Équilibrage de charge** : stratégie permettant de répartir les requêtes sur plusieurs instances d'un service (scalabilité).
- **Spring Cloud LoadBalancer** : outil moderne pour le load balancing interne à Spring Cloud.
- **Spring Cloud Gateway** : routeur d’API moderne, remplace souvent Zuul, point d’entrée unique.
- **Ribbon** : ancien outil désormais déprécié mais encore mentionné dans certains projets.
- **API Gateway = point d’accès unique** à tout le système, centralise la sécurité, le routage et le monitoring.

---

## LE SERVICE DISCOVERY DE SPRING CLOUD

- **Pourquoi multi-instancier ?** Pour améliorer la **disponibilité**, la **scalabilité**, et la **tolérance aux pannes**.
- **Problème** : comment savoir sur quelle instance appeler un service ?
- **Solution : Eureka**
  - Service de **répertoire dynamique** (naming server)
  - Chaque microservice **s’enregistre automatiquement** dans Eureka
  - Les autres services peuvent ensuite **le découvrir dynamiquement** via son nom
- **Spring Cloud Eureka Client** : facilite l'enregistrement automatique des microservices
- **Avec Eureka + Gateway**, on obtient une architecture dynamique, scalable, et adaptée au Cloud.

