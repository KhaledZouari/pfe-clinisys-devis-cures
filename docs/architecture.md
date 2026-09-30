# Architecture du module

## Portée de cette description

Cette page décrit uniquement ce qui peut être déduit des éléments techniques disponibles. Elle ne révèle aucun endpoint, hôte, serveur, client ou composant interne de l'entreprise.

Le module est un front-end multipage intégré à une application hospitalière existante. Il consomme des services métier au format JSON. Le backend, l'authentification serveur, la base et l'infrastructure de déploiement ne sont pas inclus dans ce portfolio.

## Vue contextuelle

```mermaid
flowchart LR
    A[Agent habilité] --> M[Module interne de devis]
    M --> S[Services métier externes]
    S --> P[(Données patient — système externe)]
    S --> R[(Référentiels métier)]
    S --> D[(Devis et cures)]
```

Les types de stockage, les frontières exactes des services et leur hébergement sont **À CONFIRMER**.

## Composants front-end

```mermaid
flowchart TB
    NAV[Navigation existante] --> PAT[Parcours patient]
    PAT --> DEV[Écran de devis]
    DEV --> CAL[Calcul du calendrier]
    DEV --> CAT[Sélection des éléments]
    DEV --> TOT[Calcul des montants]
    DEV --> HTTP[Client HTTP]
    HTTP --> API[API métier externe]
    API --> VIEW[Consultation du devis]
```

- **Parcours patient** : sélection ou création d'un dossier dans l'application interne.
- **Écran de devis** : informations générales, médecins et paramètres des cures.
- **Calcul du calendrier** : répétition des jours sélectionnés selon la durée et l'intervalle.
- **Catalogue** : catégories d'éléments facturables chargées depuis des services externes.
- **Montants** : quantité, prix et total calculés côté interface, à valider côté serveur.
- **Consultation** : affichage synthétique d'un devis identifié.

## Modèle logique déduit

Ce modèle est conceptuel. Il ne représente pas le schéma physique de l'entreprise.

```mermaid
erDiagram
    PATIENT ||--o{ DEVIS : concerne
    MEDECIN ||--o{ DEVIS : associe
    DEVIS ||--o{ CURE : planifie
    DEVIS ||--o{ LIGNE_DEVIS : contient
    CURE ||--o{ PRODUIT_CURE : utilise
    CATALOGUE ||--o{ LIGNE_DEVIS : reference
    CATALOGUE ||--o{ PRODUIT_CURE : reference
```

Cardinalités, clés, champs et contraintes : **À CONFIRMER** avec une documentation autorisée du backend.

## Flux de préparation

```mermaid
sequenceDiagram
    actor Agent
    participant UI as Interface web
    participant API as Services métier

    Agent->>UI: Sélectionne un patient
    UI->>API: Demande les informations autorisées
    API-->>UI: Dossier patient
    Agent->>UI: Configure le devis et les cures
    UI->>UI: Génère les dates prévisionnelles
    Agent->>UI: Ajoute les éléments et quantités
    UI->>UI: Calcule les montants affichés
    Agent->>UI: Valide
    UI->>API: Transmet le devis
    API-->>UI: Résultat ou erreur
```

## Choix techniques observés

L'utilisation de JavaScript, Handlebars, Bootstrap, jQuery et Gulp est vérifiable dans le socle analysé. Elle s'explique principalement par l'intégration dans une application existante. La décision personnelle de choisir ces technologies est **À CONFIRMER** ; elle ne doit pas être revendiquée sans preuve.

## Limites architecturales

- logique d'interface, calcul et accès réseau fortement couplés ;
- plusieurs opérations nécessaires pour enregistrer un même devis ;
- contrat et validation serveur non disponibles ;
- état transféré entre pages côté navigateur ;
- tests automatisés absents des éléments fournis.

## Architecture cible proposée

Cette cible constitue une recommandation, pas une réalisation du PFE :

```mermaid
flowchart LR
    UI[Interface] --> V[Validation locale]
    V --> C[Client HTTP centralisé]
    C --> A[Endpoint agrégat du devis]
    A --> S[Validation et autorisation serveur]
    S --> T[Transaction métier]
    T --> DB[(Stockage)]
```

Le serveur devrait rester la source de vérité pour les autorisations, tarifs, montants finaux et règles de cohérence.
