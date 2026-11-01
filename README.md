# BLConvento — Gestion des conventions

![Java](https://img.shields.io/badge/Java-17-ED8B00?logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5-6DB33F?logo=springboot&logoColor=white)
![Thymeleaf](https://img.shields.io/badge/Thymeleaf-Templates-005F0F?logo=thymeleaf&logoColor=white)
![Spring Security](https://img.shields.io/badge/Spring%20Security-RBAC-6DB33F?logo=springsecurity&logoColor=white)
![SQL Server](https://img.shields.io/badge/SQL%20Server-JPA-CC2927?logo=microsoftsqlserver&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-Wrapper-C71A36?logo=apachemaven&logoColor=white)

Application web de **gestion des conventions** (partenariat et financement de projets), développée avec **Spring Boot** et **Thymeleaf**. Elle est issue de la migration d'une application **Laravel** : le design d'origine (thème clair/sombre, badges, panneaux) a été conservé, tandis que le backend a été réécrit en Java.

---

## Sommaire

- [Fonctionnalités](#fonctionnalités)
- [Stack technique](#stack-technique)
- [Rôles et droits](#rôles-et-droits)
- [Prérequis](#prérequis)
- [Installation](#installation)
- [Configuration](#configuration)
- [Pages de l'application](#pages-de-lapplication)
- [État d'avancement](#état-davancement)
- [Sécurité](#sécurité)
- [Auteur](#auteur)

---

## Fonctionnalités

- **Conventions** : création, consultation, modification, avec dates, secteurs, communes, maîtres d'ouvrage et partenaires associés
- **Pièces jointes PDF** : stockage local des documents de convention et consultation via un point d'accès dédié
- **Partenaires par convention** : sélection multiple (signature et montant investi prévus dans le modèle)
- **Archives** : consultation des conventions archivées
- **Historique** : journal des actions, avec suppression d'entrées
- **Référentiels** : maîtres d'ouvrage, communes, secteurs, partenaires
- **Gestion des utilisateurs** et **profil** personnel
- **Authentification** par formulaire et autorisations par rôle (Spring Security)
- **Interface** responsive avec thème clair/sombre

## Stack technique

| Couche | Technologie |
|---|---|
| Langage | Java 17 |
| Framework | Spring Boot 3.5 (Spring MVC) |
| Vues | Thymeleaf |
| Sécurité | Spring Security |
| Persistance | Spring Data JPA, validation Bean Validation |
| Base de données | Microsoft SQL Server (driver `mssql-jdbc`) |
| Build | Maven (wrapper `mvnw` inclus) |

## Rôles et droits

| Rôle | Droits principaux |
|---|---|
| `ADMIN` | Gestion des conventions, consultation des référentiels |
| `SUPER_ADMIN` | Tout ce qu'un `ADMIN` peut faire, plus la création et la modification des référentiels |

## Prérequis

- **JDK 17** ou supérieur
- **SQL Server** (local, Docker ou distant) avec une base dédiée
- Maven n'est pas nécessaire : le wrapper `mvnw` télécharge la bonne version

## Installation

```bash
# 1. Cloner le dépôt
git clone https://github.com/CharafAZIZI/conventions-management-spring-boot.git
cd conventions-management-spring-boot

# 2. Créer le fichier de configuration locale
cp .env.example .env        # Windows : copy .env.example .env
# puis renseigner DB_USER, DB_PASSWORD, etc.

# 3. Lancer l'application
./mvnw spring-boot:run      # Windows : mvnw.cmd spring-boot:run
```

L'application est ensuite disponible sur `http://localhost:8080` (modifiable avec `SERVER_PORT`).

Pour produire un JAR exécutable :

```bash
./mvnw clean package
java -jar target/blconvento-0.0.1-SNAPSHOT.jar
```

## Configuration

Les paramètres sensibles ne sont **jamais** écrits dans `application.properties` : ils sont lus depuis le fichier `.env` (non versionné) ou depuis les variables d'environnement du système.

| Variable | Description | Exemple |
|---|---|---|
| `DB_HOST` | Hôte SQL Server | `localhost` |
| `DB_PORT` | Port SQL Server | `1433` |
| `DB_NAME` | Nom de la base | `blconvento` |
| `DB_USER` | Utilisateur applicatif | |
| `DB_PASSWORD` | Mot de passe | |
| `DB_TRUST_SERVER_CERT` | Faire confiance au certificat serveur (`true` en local, `false` en production) | `true` |
| `SERVER_PORT` | Port HTTP | `8080` |
| `APP_UPLOAD_DIR` | Dossier de stockage des PDF (hors dépôt) | `./uploads/conventions` |
| `COOKIE_SECURE` | Cookies de session `Secure` (`true` derrière HTTPS) | `false` |
| `JPA_DDL_AUTO` | Stratégie de schéma Hibernate (`validate` en production) | `update` |

> **Production** : placez l'application derrière un reverse proxy HTTPS, définissez `COOKIE_SECURE=true`, `DB_TRUST_SERVER_CERT=false` et `JPA_DDL_AUTO=validate`.

## Pages de l'application

| URL | Page |
|---|---|
| `/login` | Connexion |
| `/` | Tableau de bord |
| `/conventions` | Liste des conventions |
| `/conventions/create` | Nouvelle convention |
| `/conventions/{id}` | Détail d'une convention |
| `/conventions/{id}/edit` | Modification |
| `/archives` | Archives |
| `/referentiels` | Référentiels |
| `/users` | Utilisateurs |
| `/historique` | Historique des actions |
| `/profile` | Profil |

## État d'avancement

Fonctionnalités de l'application Laravel d'origine encore à reproduire :

- [ ] Modification et suppression des maîtres d'ouvrage, communes, secteurs et partenaires (seule la création existe)
- [ ] Recherche, filtrage, tri et détection avancée de doublons sur les conventions
- [ ] Saisie complète des champs `a_signe` et `montant_investi` pour les partenaires d'une convention
- [ ] Tests automatisés et vérification complète du build (`./mvnw clean package`)

Le détail de la migration est dans [`VERIFICATION_MIGRATION.md`](./VERIFICATION_MIGRATION.md).

## Sécurité

- Aucun secret n'est versionné : utilisez `.env` (voir `.env.example`).
- Les documents téléversés (`uploads/`) ne sont pas versionnés.
- En production : HTTPS obligatoire, compte SQL à droits limités, `JPA_DDL_AUTO=validate`, cookies `Secure`.
- Pour signaler une faille, contactez l'auteur en privé plutôt que d'ouvrir une issue publique.

## Auteur

**Charaf AZIZI**
GitHub : [@CharafAZIZI](https://github.com/CharafAZIZI)
