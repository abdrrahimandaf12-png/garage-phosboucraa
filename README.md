# Garage OCP Phosboucraa

Application de gestion des missions, véhicules, consommations et interventions
pour le garage OCP Phosboucraa — Projet de stage (DUT CDL — EST Dakhla).

## Stack technique

| Couche | Technologie |
|---|---|
| Back-end | **ASP.NET Core 8.0** (Web API / C#) |
| Base de données | **SQLite** (dev) / **SQL Server** (prod) avec **Entity Framework Core** |
| Authentification | **JWT** (BCrypt pour les mots de passe) |
| Front-end | HTML / CSS / JavaScript vanilla (statique) |
| Documentation API | **Swagger / OpenAPI** |

## Fonctionnalités

- Gestion du parc automobile : fiches véhicule, statuts, kilométrage
- Gestion des missions : création, affectation, suivi des statuts
- Gestion des consommations : carburant, lubrifiants, coûts
- Gestion des interventions : entretiens, réparations, historiques
- Gestion des utilisateurs : CRUD, rôles (admin, mécanicien, chauffeur, user)
- Authentification JWT avec contrôle d'accès par rôle
- Demandes de véhicule avec workflow d'approbation/rejet
- Reporting : KPIs, coûts par véhicule, consommations mensuelles

## Structure

```
garage-phosboucraa/
├── backend-aspnet/          ← API ASP.NET Core
│   ├── Controllers/         ← 8 contrôleurs (auth, vehicules, missions...)
│   ├── Models/              ← Entités (Vehicule, Mission, Consommation...)
│   ├── Data/                ← DbContext + DbSeeder
│   ├── Program.cs           ← Point d'entrée
│   └── appsettings.json     ← Configuration (JWT, DB)
├── public/                  ← Front-end statique
│   ├── index.html
│   ├── css/
│   ├── js/
│   └── img/
└── README.md
```

## Prérequis

- [.NET SDK 8.0](https://dotnet.microsoft.com/download/dotnet/8.0)
- SQL Server (optionnel, pour la production)

## Installation et lancement

```bash
cd backend-aspnet
dotnet run
```

Accès navigateur : **http://localhost:5000**

## API - Swagger

Documentation interactive : **http://localhost:5000/swagger**

### Endpoints

| Route | Auth | Rôle | Description |
|---|---|---|---|
| `POST /api/auth/login` | Non | — | Connexion |
| `POST /api/auth/register` | Non | — | Inscription |
| `GET/POST /api/users` | JWT | admin | Lister / créer utilisateurs |
| `GET/PUT/DELETE /api/users/{id}` | JWT | admin | Détail / modifier / supprimer |
| `GET/POST /api/vehicules` | JWT | * | Lister / créer véhicules |
| `GET/PUT/DELETE /api/vehicules/{id}` | JWT | admin/chauffeur | Détail / modifier / supprimer |
| `GET/POST /api/missions` | JWT | admin/chauffeur | Lister / créer missions |
| `PUT/DELETE /api/missions/{id}` | JWT | admin/chauffeur | Modifier / supprimer |
| `GET/POST /api/consommations` | JWT | admin | Lister / créer consommations |
| `PUT/DELETE /api/consommations/{id}` | JWT | admin | Modifier / supprimer |
| `GET/POST /api/interventions` | JWT | admin/mecanicien | Lister / créer interventions |
| `PUT/DELETE /api/interventions/{id}` | JWT | admin/mecanicien | Modifier / supprimer |
| `GET /api/interventions/echeances/proches` | JWT | admin/mecanicien | Échéances dans 30 jours |
| `GET/POST /api/demandes-vehicule` | JWT | * | Lister / créer demandes |
| `PUT/DELETE /api/demandes-vehicule/{id}` | JWT | admin/chauffeur | Traiter / supprimer |
| `GET /api/reporting/kpis` | JWT | * | Indicateurs par rôle |
| `GET /api/reporting/couts-par-vehicule` | JWT | admin | Coûts par véhicule |
| `GET /api/reporting/consommation-mensuelle` | JWT | admin | Consommations mensuelles |
| `GET /api/reporting/missions-par-statut` | JWT | admin | Missions par statut |
| `GET /api/reporting/interventions-par-type` | JWT | admin/mecanicien | Interventions par type |

## Comptes de démonstration

| Identifiant | Mot de passe | Rôle |
|---|---|---|
| `admin` | `admin123` | Administrateur |
| `mecanicien` | `meca123` | Mécanicien |
| `user` | `user123` | Utilisateur |
| `chauffeur` | `chauffeur123` | Chauffeur |

## Base de données

Par défaut, SQLite est utilisé (fichier `garage.db` créé automatiquement).
Les données de démonstration sont insérées au premier démarrage.

### Basculer vers SQL Server

Dans `appsettings.json` :

```json
"DatabaseProvider": "SqlServer",
"ConnectionStrings": {
    "SqlServer": "Server=localhost;Database=GaragePhosboucraa;Trusted_Connection=True;TrustServerCertificate=True;"
}
```

## Licence

MIT
