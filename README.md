# InternTracker

**InternTracker** est une application web CRUD développée entièrement en Swift côté serveur. Elle permet de gérer et suivre ses candidatures de stage : ajout, consultation, modification, suppression, classement par catégorie, recherche, filtrage et tri.

Le projet est réalisé dans le cadre du cours de développement iOS (2026) à l'Université Paris 8, en utilisant le framework **Hummingbird 2** et **SQLite** pour la persistance des données.

---

## Fonctionnalités

- **Tableau de bord** avec statistiques en temps réel (total, en attente, entretien, acceptée, refusée) et barre de progression du taux de réponse.
- **CRUD complet** : créer, lire, modifier et supprimer des candidatures.
- **Page de détails** dédiée pour chaque candidature (`/detail/:id`), avec formulaire de modification intégré.
- **Deuxième modèle de données** : gestion des catégories (Développement Web, IA, Systèmes Embarqués, Cybersécurité, Data Science, DevOps, Mobile, Autre) avec CRUD indépendant et relation clé étrangère.
- **Recherche et filtrage** : barre de recherche (entreprise, poste) et filtre par statut via clause `WHERE`.
- **Tri** : par date, par entreprise (A–Z) ou par statut via paramètres de requête.
- **Validation côté serveur** : messages d'erreur affichés si les champs obligatoires sont vides.
- **Interface soignée** : design sombre personnalisé avec icônes Material Symbols, animations CSS, mise en page responsive.

---

## Modèle de données

### `InternshipApplication` (8 champs)

| Champ         | Type      | Description                                  |
|---------------|-----------|----------------------------------------------|
| `id`          | `Int64?`  | Clé primaire auto-incrémentée                |
| `company`     | `String`  | Nom de l'entreprise                          |
| `position`    | `String`  | Intitulé du poste                            |
| `categoryId`  | `Int64?`  | Clé étrangère vers la table `categories`     |
| `status`      | `String`  | Statut : En attente, Entretien, Acceptée, Refusée |
| `appliedDate` | `String`  | Date de candidature (YYYY-MM-DD)             |
| `notes`       | `String`  | Notes et commentaires libres                 |
| `url`         | `String`  | Lien vers l'offre d'emploi                   |

### `Category` (2 champs)

| Champ  | Type     | Description                       |
|--------|----------|-----------------------------------|
| `id`   | `Int64?` | Clé primaire auto-incrémentée     |
| `name` | `String` | Nom de la catégorie (unique)      |

Les deux structs sont conformes aux protocoles `Codable` et `Sendable`.

---

## Routes exposées

| Méthode | Route                    | Description                                      |
|---------|--------------------------|--------------------------------------------------|
| `GET`   | `/`                      | Tableau de bord — liste toutes les candidatures (supporte `?search=`, `?status=`, `?sort=`) |
| `GET`   | `/add`                   | Formulaire de création d'une nouvelle candidature |
| `POST`  | `/create`                | Créer une candidature (avec validation serveur)   |
| `GET`   | `/detail/:id`            | Page de détails d'une candidature                 |
| `POST`  | `/update/:id`            | Modifier une candidature existante                |
| `POST`  | `/delete/:id`            | Supprimer une candidature                         |
| `GET`   | `/categories`            | Page de gestion des catégories                    |
| `POST`  | `/categories/create`     | Ajouter une nouvelle catégorie                    |
| `POST`  | `/categories/delete/:id` | Supprimer une catégorie                           |

**Total : 9 routes** (3 GET + 6 POST).

---

## Structure du projet

```
Sources/App/
├── main.swift        # Point d'entrée — configuration du serveur et définition des routes
├── Models.swift      # Modèles de données : InternshipApplication, Category, ApplicationStatus
├── Database.swift    # Couche SQLite — création des tables et fonctions CRUD typées
└── Views.swift       # Génération HTML côté serveur (layout, dashboard, formulaires, détails, 404)
```

---

## Compilation et exécution

Ouvrir le projet dans GitHub Codespaces, puis dans le terminal :

```bash
./build.sh
./run.sh
```

L'application sera accessible sur le port **8080**. Codespaces proposera d'ouvrir le navigateur automatiquement.

Pour arrêter le serveur : `Ctrl + C`.

---

## Technologies

- **Swift 6.2** — Langage principal
- **Hummingbird 2** — Framework web côté serveur
- **SQLite.swift** — ORM typé pour SQLite (aucune chaîne SQL brute)
- **Material Symbols Rounded** — Icônes
- **CSS custom** — Design sombre, responsive, avec animations

---

*Aymen Raki — L3 ISEI, Université Paris 8, 2026*
