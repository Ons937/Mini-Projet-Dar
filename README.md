# 🎓 Gestion PFE - Plateforme Collaborative Distribuée

![Java](https://img.shields.io/badge/Java-17%2B-ed8b00?style=for-the-badge&logo=java&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-6db33f?style=for-the-badge&logo=spring&logoColor=white)
![ActiveMQ](https://img.shields.io/badge/JMS-ActiveMQ-CC2927?style=for-the-badge&logo=apache&logoColor=white)
![SQL Server](https://img.shields.io/badge/SQL_Server-2022-CC2927?style=for-the-badge&logo=microsoft-sql-server&logoColor=white)
![Hibernate](https://img.shields.io/badge/Hibernate-ORM-59666C?style=for-the-badge&logo=hibernate&logoColor=white)

Une solution **hybride et distribuée** pour gérer le cycle de vie complet des Projets de Fin d'Études (PFE).  
Elle connecte en temps réel les trois acteurs clés : **Étudiants**, **Enseignants** et **Sociétés**.

---

## 🏗️ Architecture Technique

Ce projet démontre une maîtrise des **systèmes répartis** en combinant des technologies Java standard et une interface Web moderne.

### 1. Middleware & Communication
*   **📡 Java RMI (Remote Method Invocation)** : Assure la communication synchrone entre la passerelle Web (Spring Boot) et le serveur Backend. Utilise **JNDI** pour l'annuaire de services.
*   **📨 JMS (Java Message Service)** : Assure la communication asynchrone pour les **Notifications Push** en temps réel (via Apache ActiveMQ).

### 2. Backend & Persistance
*   **Logique Métier** : Hébergée sur un serveur Java autonome.
*   **Données** : Stockées sur **Microsoft SQL Server**.
*   **ORM** : Utilisation de **JPA / Hibernate** pour la gestion transparente des données.

### 3. Clients (Approche Hybride)
*   **🌐 Client Léger (Web)** : Interface principale en **HTML5/JS** propulsée par **Spring Boot**. Utilise **WebSocket** pour relayer les notifications JMS aux utilisateurs web.
*   **🖥️ Client Lourd (Desktop)** : Application **Java Swing** utilisée spécifiquement pour démontrer la réception native des messages JMS.

---

## 🔄 Workflow Métier (Cycle de vie du Projet)

Le système implémente un workflow séquentiel strict avec gestion d'états :

1.  **Proposition** : L'étudiant propose un sujet (`ATTENTE_SOC`).
2.  **Validation Société** : L'entreprise accepte le sujet (`ATTENTE_PROF`) ou le refuse (`REFUSE_SOC`).
3.  **Validation Professeur** : L'enseignant valide le sujet pédagogiquement (`AFFECTE`).
4.  **Gestion des Refus** :
    *   Si la société refuse : Le projet est annulé.
    *   Si le prof refuse : L'accord de la société est conservé, l'étudiant doit changer d'encadrant (`REFUSE_PROF`).

> **Règle d'or** : Un étudiant ne peut avoir qu'un seul projet actif ou validé à la fois.

---

## 📦 Structure du Projet (Maven)

Le projet est divisé en modules pour respecter la séparation des préoccupations :

| Module | Description |
| :--- | :--- |
| `pfe-common` | **Contrat** : Interfaces RMI (`IPfeService`), Modèles JPA (`User`, `Project`). Partagé par tous. |
| `pfe-server` | **Backend** : Implémentation RMI, Connexion BDD (Hibernate), Producteur JMS. |
| `pfe-web` | **Gateway** : Application Spring Boot, Contrôleur REST, WebSocket, Interface HTML/JS. |
| `pfe-client` | **Client Swing** : Démonstrateur technique pour l'écoute JMS native. |

---

## 🚀 Installation et Démarrage

### Prérequis
*   **Java JDK 17** ou supérieur.
*   **Apache ActiveMQ** (Classic) lancé sur le port `61616`.
*   **SQL Server** configuré sur le port `55555`.

### Étapes de lancement

1.  **Démarrer l'infrastructure** :
    ```cmd
    activemq start
    ```
2.  **Lancer le Serveur Backend** (`ServerApp`) :
    *   Initialise le registre RMI sur le port `1099`.
    *   Connecte la base de données.
3.  **Lancer l'Application Web** (`PfeWebApplication`) :
    *   Démarre Spring Boot sur le port `8080`.
4.  **Accéder à l'application** :
    *   Ouvrir le navigateur : `http://localhost:8080`

## 📸 Aperçu

*L'interface s'adapte dynamiquement selon le rôle connecté.*

*   **Étudiant** : Tableau de bord avec statut coloré (Jaune/Bleu/Vert/Rouge) selon l'avancement.
*   **Société** : Gestion des offres et validation des propositions entrantes.
*   **Professeur** : Centre de validation et liste des encadrements.

---
