# Sécurité, confidentialité et limites

## Positionnement

Cette documentation n'affirme ni conformité RGPD, ni conformité à la législation tunisienne, ni sécurité complète, ni mise en production. Une telle affirmation nécessiterait des preuves organisationnelles et techniques qui ne sont pas disponibles ici.

## Données concernées

Le parcours manipule potentiellement des données d'identité et des informations liées à une prise en charge. Ces données doivent être considérées comme sensibles. Ce dépôt public n'en contient aucune.

## Mesures observables

- échanges applicatifs structurés en JSON ;
- présence d'un mécanisme de session dans le socle existant ;
- utilisation de transport chiffré dans l'environnement de développement observé ;
- suppression de certaines données temporaires après leur utilisation ;
- utilisation ponctuelle de méthodes de rendu limitant l'interprétation de HTML.

Ces éléments ne prouvent pas le contrôle d'accès serveur, le chiffrement en production, la journalisation, la conservation ou la résistance aux attaques.

## Limites connues

| Niveau | Limite | Traitement recommandé |
|---|---|---|
| Critique | Enregistrement en plusieurs opérations, avec risque de résultat partiel | Endpoint agrégat et transaction serveur |
| Élevé | Données patient temporairement présentes dans le navigateur | Transmettre un identifiant minimal et recharger après autorisation |
| Élevé | Autorisations serveur non auditables | Contrôles systématiques côté API et tests de rôles |
| Élevé | Validation incomplète | Validation structurée côté client et serveur |
| Élevé | Absence de tests automatisés démontrés | Tests unitaires, contrats, intégration et parcours complets |
| Moyen | Dépendances historiques | Inventaire, audit et mises à jour progressives |
| Moyen | Gestion d'erreurs partielle | Erreurs structurées et journalisation sans données sensibles |
| À vérifier | XSS, CSRF, injections et gestion des secrets | Audit du système complet |

## Principes de publication

- aucune donnée réelle ou pseudonymisée ;
- aucune capture issue d'un environnement d'entreprise ;
- aucune URL, adresse IP, nom de serveur, client ou collaborateur ;
- aucun jeton, identifiant interne ou extrait de configuration ;
- illustrations entièrement recréées et marquées « DONNÉES FICTIVES » ;
- suppression des métadonnées avant ajout d'une image ;
- revue manuelle avant chaque publication.

## Travaux requis avant une affirmation de conformité

À titre indicatif : gouvernance et registre des traitements, base juridique, information des personnes, minimisation, durées de conservation, gestion des droits, habilitations, journalisation, sauvegardes, gestion des incidents, contrats avec les sous-traitants et analyse d'impact lorsque requise.

Le cadre juridique applicable et la responsabilité de chaque acteur sont **À CONFIRMER** avec des personnes compétentes. Ce document n'est pas un avis juridique.

## Frontière clinique

Le module prépare un devis et un calendrier administratif prévisionnel. Il ne constitue pas une prescription, une validation de protocole ou une décision clinique. Toute évolution vers le suivi des effets indésirables ou l'aide à la décision nécessiterait un cadrage médical, réglementaire, éthique et de sécurité distinct.
