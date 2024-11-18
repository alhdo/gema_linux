
# Système de Gestion de Bibliothèque Spatiale

**Date de remise :** Mercredi 20 Novembre 23:59:59

## Contexte

La Grande Bibliothèque Interplanétaire a besoin d'un système de gestion pour cataloguer et gérer sa collection de livres rares. Vous êtes chargé(e) de développer un ensemble d'outils en Bash pour répondre à ce besoin.

## Prérequis Techniques

- Système Linux/Unix
- Bash
- Droits d'écriture/lecture sur le système de fichiers
- Éditeur de texte

## Structure des Fichiers

Copy

```
library/ 
├── books.txt       # Base de données des livres 
├── users.txt       # Base de données des utilisateurs 
├── logs/          # Dossier des journaux 
└── scripts/       # Vos scripts
```

## Partie 1 : Configuration Initiale (20 points)

### 1.1 Création de l'Environnement (8 points)

Créez un script `setup.sh` qui :

1. Crée la structure de répertoires ci-dessus
2. Initialise les fichiers avec les en-têtes appropriés
3. Configure les permissions correctes
4. Documente chaque étape dans un fichier `setup.log`

### 1.2 Validation de l'Environnement (12 points)

Créez un script `check_environment.sh` qui vérifie :

1. L'existence de tous les fichiers/dossiers requis
2. Les permissions correctes
3. La capacité à lire/écrire dans les fichiers
4. Affiche un rapport détaillé

## Partie 2 : Gestion des Livres (40 points)

### 2.1 Ajout de Livres (15 points)

Créez `add_book.sh` qui :

- Demande les informations du livre :
    - Titre
    - Auteur
    - Année de publication
    - Planète d'origine
    - ISBN Galactique
- Valide les données entrées
- Ajoute le livre à `books.txt`
- Enregistre l'opération dans les logs

### 2.2 Recherche de Livres (15 points)

Créez `search_book.sh` qui :

- Permet la recherche par :
    - Titre
    - Auteur
    - ISBN
- Affiche les résultats de manière formatée
- Gère les cas où aucun livre n'est trouvé

__Documentez vous sur la commande grep__

### 2.3 Gestion de l'Inventaire (10 points)

Créez `inventory.sh` qui :

- Liste tous les livres
- Produit des statistiques :
    - Nombre total de livres
    - Livres par planète
    - Livres par siècle

## Partie 3 : Journal et Maintenance (40 points)

### 3.1 Système de Journalisation (15 points)

Créez `logger.sh` qui :

- Enregistre toutes les opérations
- Format : `[DATE] [HEURE] [OPÉRATION] [DÉTAILS]`

### 3.2 Sauvegarde (15 points)

Créez `backup.sh` qui :

- Sauvegarde la base de données des livres (command zip, tar a regarder)
- Compresse les archives
- Maintient un historique des 3 dernières sauvegardes
- Vérifie l'intégrité des sauvegardes

### 3.3 Rapport d'Activité (10 points)

Créez `report.sh` qui génère un rapport contenant :

- Les opérations du jour
- Les nouvelles acquisitions
- Les statistiques d'utilisation
- Les erreurs éventuelles
```bash
# Format attendu dans le rapport
=== STATISTIQUES DU [DATE] ===
- Nombre total d'opérations : [X]
- Nouvelles entrées : [X]
- Recherches effectuées : [X]
```

## Critères d'Évaluation

### Fonctionnalité (40%)

- Scripts fonctionnels
- Gestion des erreurs
- Respect des spécifications

### Code (30%)

- Structure claire
- Documentation
- Conventions de nommage
- Commentaires pertinents

### Robustesse (20%)

- Gestion des cas limites
- Validation des entrées
- Protection contre les erreurs utilisateur

### Bonus (10%)

- Interface utilisateur améliorée
- Fonctionnalités supplémentaires utiles
- Documentation exceptionnelle

## Format de Rendu

### Documents à Rendre

1. Archive contenant :
    - Tous les scripts
    - Fichier README détaillé
    - Documentation d'installation
    - Documentation utilisateur
    - Exemples d'utilisation
2. Rapport technique expliquant :
    - Architecture choisie
    - Choix techniques
    - Difficultés rencontrées
    - Solutions implémentées

### Modalités de Rendu

- Archive nommée : `NOM_Prenom_BashExam.tar.gz`
- Date limite : 20/11/2024 à 23:59
## NOUVEAU : Bonus GitHub (15 points supplémentaires)

### Configuration du Dépôt (5 points)

- Structure claire du repository
- README.md complet
- .gitignore approprié

### Documentation GitHub (5 points)

1. README.md professionnel contenant :
    
    
```markdown
# Système de Gestion de Bibliothèque Spatiale

## Installation
[Instructions détaillées]

## Configuration
[Étapes de configuration]

## Utilisation
[Exemples avec captures d'écran]

## Architecture
[Schémas et explications]

## Tests
[Instructions pour les tests]
```
    
2. Wiki GitHub avec :
    - Guide utilisateur
    - Guide administrateur

### Critères d'Évaluation du Bonus GitHub

- Qualité de la documentation
- Organisation du code
- Utilisation des fonctionnalités GitHub
- Historique des commits
- Clarté du README
- Utilisation des issues/projets (optionnel)

Note : Le bonus GitHub n'est accordé que si le projet de base atteint au moins 60% des points.

## Règles

1. Travail strictement individuel
2. Utilisation exclusive de Bash
3. Pas de scripts externes
4. Documentation obligatoire

## Conseils

- Commencez par la structure générale
- Testez chaque script individuellement
- Documentez au fur et à mesure
- Gérez les erreurs dès le début
- Prévoyez du temps pour les tests

---

**Note importante :** Ce TP est un examen individuel. Tout partage de code sera considéré comme de la triche et sanctionné en conséquence.

---

L'attribution des points se fera selon la grille suivante :

- A (90-100%) : Excellent, tout fonctionne parfaitement
- B (80-89%) : Très bien, quelques détails à améliorer
- C (70-79%) : Bien, fonctionnalités principales présentes
- D (60-69%) : Passable, plusieurs manques
- E (50-59%) : Minimum atteint
- F (<50%) : Insuffisant

# Reference
- [Mission Interstellaire (Dernier TD)](td/mission_interstellaires.md)
