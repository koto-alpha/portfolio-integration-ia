# Projet d'intégration IA — Automatisation du service client

## 1. Présentation du projet

Ce projet consiste à concevoir un workflow d'automatisation avec n8n et un modèle d'intelligence artificielle exécuté localement avec Ollama.

L'objectif est de traiter des demandes clients, de les classer par catégorie et par priorité, de vérifier les réponses générées, puis d'enregistrer les résultats dans un fichier CSV.

Le projet est réalisé dans un environnement local à des fins d'apprentissage et de portfolio.

## 2. Objectifs

- Automatiser le traitement de demandes clients.
- Utiliser un modèle d'IA local pour produire une classification structurée.
- Exploiter des données JSON dans n8n.
- Valider techniquement les réponses de l'IA.
- Comparer les décisions de l'IA à des règles métier définies.
- Gérer les erreurs et conserver les résultats dans un fichier CSV.

## 3. Technologies utilisées

- **n8n** : orchestration du workflow d'automatisation.
- **Ollama** : exécution locale du modèle d'IA.
- **Qwen3:4b** : modèle utilisé pour classifier les demandes.
- **JavaScript** : transformation des données et validation des résultats.
- **CSV** : stockage et consultation des résultats.
- **Docker Desktop** : exécution du conteneur n8n.

## 4. Architecture du workflow

Le workflow suit les étapes suivantes :

1. Déclenchement manuel du workflow.
2. Création de demandes clients de test.
3. Envoi des demandes à l'API locale d'Ollama.
4. Génération d'une catégorie et d'une priorité au format JSON.
5. Validation technique de la réponse.
6. Comparaison de la classification avec des règles métier.
7. Conversion des résultats en CSV.
8. Enregistrement du fichier dans le dossier `exports`.

Une branche distincte est également configurée pour traiter les erreurs de la requête HTTP et enregistrer les informations d'échec.

## 5. Règles métier du jeu de test

Les règles utilisées pour les quatre demandes de démonstration sont les suivantes :

| Demande | Catégorie attendue | Priorité attendue |
|---|---|---|
| Remboursement courant | Remboursement | Priorité Normale |
| Colis en retard depuis cinq jours | Livraison | Priorité Normale |
| Produit défectueux | Réclamation | Priorité Haute |
| Double débit | Remboursement | Priorité Haute |

Ces règles sont définies pour le jeu de test pédagogique. Elles ne constituent pas une politique de priorité validée par une entreprise.

## 6. Validation technique et métier

Le workflow distingue deux contrôles.

### Validation technique

Elle vérifie notamment :
- que la réponse reçue peut être analysée comme du JSON ;
- que la catégorie appartient à la liste autorisée ;
- que la priorité appartient à la liste autorisée.

### Validation métier

Elle compare la catégorie et la priorité produites par l'IA aux règles définies pour les demandes de test.

Une réponse peut donc être techniquement valide, mais non conforme aux règles métier.

## 7. Résultats des tests

Quatre demandes ont été traitées lors du test documenté.

| ID | Catégorie produite | Priorité produite | Statut technique | Statut métier |
|---:|---|---|---|---|
| 1 | Remboursement | Priorité Haute | Validé | Non conforme |
| 2 | Livraison | Priorité Haute | Validé | Non conforme |
| 3 | Réclamation | Priorité Haute | Validé | Conforme |
| 4 | Remboursement | Priorité Haute | Validé | Conforme |

Résultats observés :
- 4 réponses sur 4 étaient techniquement valides.
- 2 demandes sur 4 étaient conformes aux règles métier.
- Les deux écarts de priorité ont été détectés et expliqués dans le fichier CSV.

Ces résultats concernent uniquement les quatre demandes de test et ne mesurent pas les performances générales du modèle.

## 8. Gestion des erreurs

Les tests réalisés ont notamment porté sur :
- une catégorie non autorisée ;
- une réponse JSON invalide ;
- une URL incorrecte pour provoquer une erreur HTTP ;
- l'utilisation de la branche d'erreur du nœud HTTP Request ;
- la configuration de nouvelles tentatives en cas d'échec.

Le workflow utilise également des champs de statut et d'erreur afin de faciliter l'analyse des résultats.

## 9. Fichiers du projet

- `README.md` : documentation du projet.
- `exports/` : fichiers CSV produits par les workflows.
- `n8n-config/` : configuration Docker Compose de n8n.
- `n8n-config-backup.json` : sauvegarde de configuration.
- `n8n_data_backup.tar.gz` : sauvegarde des données persistantes de n8n.

## 10. Limites identifiées

- Le modèle peut produire une classification techniquement valide mais métier incorrecte.
- Les règles métier utilisées sont simples et adaptées aux demandes de démonstration.
- Le jeu de test ne comporte que quatre demandes.
- Une validation humaine reste nécessaire pour les cas ambigus ou les décisions importantes.
- Les résultats obtenus ne suffisent pas à conclure à la fiabilité générale du système.

## 11. Améliorations possibles

- Élargir le jeu de test avec des demandes variées.
- Ajouter des cas ambigus et des scénarios limites.
- Mesurer les erreurs de classification par catégorie et par priorité.
- Améliorer les règles métier avec un responsable métier.
- Ajouter une procédure de revue humaine des demandes non conformes.
- Étudier une persistance des résultats dans une base de données SQL.

## 12. Sécurité

Le modèle Ollama est exécuté localement dans l'environnement de démonstration. L'accès à n8n est publié sur l'adresse locale `127.0.0.1:5678`.

Les données de test sont fictives. Avant une utilisation en entreprise, il faudrait notamment définir les droits d'accès, la gestion des secrets, la politique de conservation des données et les règles autorisant le traitement des données personnelles.

---

**Statut du projet :** prototype pédagogique fonctionnel, avec validation technique, contrôle métier et export CSV.
