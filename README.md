# 🏆 RéuniSport

**RéuniSport** est une application web de gestion et de réservation d'équipements sportifs. Elle permet aux clubs sportifs de consulter les équipements disponibles, d'effectuer des réservations et de gérer leurs demandes.

L'application dispose également d'un espace administrateur permettant de gérer les clubs, les utilisateurs et les équipements.

## 🎯 Objectifs du projet

- Centraliser la gestion des équipements sportifs.
- Permettre aux clubs de réserver du matériel et des infrastructures.
- Éviter les conflits entre les réservations.
- Faciliter le suivi et la gestion des réservations.
- Mettre en place une architecture basée sur des microservices.

## 👥 Les utilisateurs

L'application comporte deux types d'utilisateurs :

- **Administrateur :** gère les clubs, les utilisateurs, les équipements et l'ensemble des réservations.
- **Club sportif :** consulte le catalogue des équipements, effectue des réservations et consulte ses propres réservations.

## 🏗️ Architecture technique

RéuniSport repose sur une architecture composée de plusieurs microservices indépendants, communiquant entre eux via une API.

```text
                    UTILISATEUR
                         |
                         v
                  FRONTEND WEB
                (PHP / HTML / CSS / JS)
                         |
                         v
                  API GATEWAY
                     (Nginx)
                         |
          +--------------+--------------+
          |              |              |
          v              v              v
     AUTH SERVICE  CATALOGUE SERVICE  RESERVATION SERVICE
          |              |              |
          v              v              v
       AUTH DB       CATALOGUE DB    RESERVATION DB
        MySQL           MySQL           MySQL
```

### 🔐 1. Auth Service

Gère l'authentification et les utilisateurs de l'application.

**Fonctionnalités :**
- Connexion et déconnexion.
- Gestion des comptes utilisateurs.
- Gestion des rôles (administrateur et club).
- Authentification avec JWT.
- Association des utilisateurs à leurs clubs.

**Base de données : `reunisport_auth`**

| Colonne | Type | Description |
|---|---|---|
| `id` | INT | Identifiant du club ou de l'utilisateur (clé primaire) |
| `nom` | VARCHAR(150) | Nom du club |
| `ville` | VARCHAR(100) | Ville du club |
| `code_club` | VARCHAR(30) | Code unique du club |
| `actif` | BOOLEAN | État du club ou du compte |
| `created_at` | TIMESTAMP | Date de création |
| `updated_at` | TIMESTAMP | Date de dernière modification |
| `username` | VARCHAR(100) | Nom d'utilisateur |
| `email` | VARCHAR(190) | Adresse e-mail |
| `password_hash` | VARCHAR(255) | Mot de passe sous forme de hash |
| `role` | ENUM | Rôle : admin ou club |
| `club_id` | INT | Identifiant du club associé (clé étrangère) |
| `must_change_password` | BOOLEAN | Indique si le mot de passe doit être changé |
| `username_attempted` | VARCHAR(100) | Nom d'utilisateur saisi lors d'une tentative de connexion |
| `user_id` | INT | Identifiant de l'utilisateur concerné (clé étrangère, nullable) |
| `success` | BOOLEAN | Indique si la connexion a réussi |
| `ip_address` | VARCHAR(45) | Adresse IP |
| `attempted_at` | TIMESTAMP | Date et heure de la tentative |

### 🏟️ 2. Catalogue Service

Gère les équipements sportifs disponibles dans l'application.

**Fonctionnalités :**
- Consulter les équipements.
- Consulter les détails d'un équipement.
- Ajouter et modifier des équipements.
- Activer ou désactiver un équipement.

**Base de données : `reunisport_catalogue`**

| Colonne | Type | Description |
|---|---|---|
| `id` | INT | Identifiant de l'équipement (clé primaire) |
| `nom` | VARCHAR(150) | Nom de l'équipement |
| `type` | ENUM | Type : salle, terrain, minibus, sonorisation, chronometre ou autre |
| `lieu` | VARCHAR(200) | Localisation de l'équipement |
| `capacite` | INT | Capacité de l'équipement (nullable) |
| `description` | TEXT | Description de l'équipement (nullable) |
| `actif` | BOOLEAN | Indique si l'équipement est disponible à la réservation |
| `created_at` | TIMESTAMP | Date de création |
| `updated_at` | TIMESTAMP | Date de dernière modification |

### 📅 3. Reservation Service

Gère les réservations effectuées par les clubs.

**Fonctionnalités :**
- Créer une réservation.
- Consulter ses réservations.
- Consulter les réservations des équipements.
- Annuler une réservation.
- Vérifier les disponibilités.
- Empêcher les chevauchements de réservation.

**Base de données : `reunisport_reservation`**

| Colonne | Type | Description |
|---|---|---|
| `id` | INT | Identifiant de la réservation (clé primaire) |
| `user_id` | INT | Identifiant de l'utilisateur provenant de l'Auth Service |
| `equipement_id` | INT | Identifiant de l'équipement provenant du Catalogue Service |
| `reservation_date` | DATE | Date de la réservation |
| `heure_debut` | TIME | Heure de début |
| `heure_fin` | TIME | Heure de fin |
| `statut` | ENUM | Statut : confirmée ou annulée |
| `created_at` | TIMESTAMP | Date de création |
| `updated_at` | TIMESTAMP | Date de dernière modification |
| `cancelled_at` | TIMESTAMP | Date d'annulation (nullable) |

### 🌐 4. API Gateway

L'API Gateway constitue le point d'entrée unique vers les différents microservices.

**Rôle :**
- Recevoir les requêtes du frontend.
- Rediriger les requêtes vers le microservice correspondant.
- Centraliser l'accès aux API.
- Éviter que le frontend communique directement avec chaque service.

Technologie utilisée : **Nginx**.

## 🛠️ Technologies utilisées

| Technologie | Utilisation |
|---|---|
| PHP | Développement web |
| HTML / CSS | Structure et design |
| JavaScript | Interactions utilisateur |
| MySQL | Gestion des données |
| phpMyAdmin | Administration des bases de données |
| Docker / Docker Compose | Conteneurisation |
| Nginx | API Gateway |
| JWT | Authentification |

---

**RéuniSport — Une plateforme centralisée pour simplifier la gestion et la réservation des équipements sportifs.**
