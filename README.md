# Scripts-HAL

Collection de scripts python utilisables seuls et modulables pour faciliter la curation et faire des analyses bibliométriques à partir de l'API.

## HAL Curation Helper — Script de flagging

Script Python de détection automatique de problèmes potentiels sur des notices HAL, conçu pour faciliter le travail de curation d'une collection.

### Fonctionnement

À partir d'un code de collection et d'une plage de dates, le script interroge l'API HAL et analyse les notices déposées sur la période. Pour chaque notice, il évalue une série de règles et signale les irrégularités détectées.

### Règles de flagging implémentées (7)

- **DOI manquant** — articles sans DOI, hors articles en cours de publication (in press) et hors droit
- **Article sans résumé** — articles disposant d'un DOI mais sans résumé, hors comptes rendus (BOOKREVIEW)
- **Éditeur scientifique absent** — chapitres d'ouvrage sans éditeur scientifique (dir.)
- **Revue invalide** — notices dont la revue est en statut INCOMING dans le référentiel HAL
- **Auteurs sans affiliation** — notices où au moins un auteur n'est rattaché à aucune structure
- **Doublons potentiels** — recherche par titre et/ou DOI dans l'ensemble de HAL
- **Langue suspecte** — détection automatique de la langue du titre comparée à la langue déclarée

Les règles sont modulaires : il est simple d'en ajouter, retirer ou modifier dans le code.

### Dépendances

```
requests
pandas
langdetect
```

### Déploiement

Une interface web est disponible via Streamlit, permettant de lancer l'analyse, visualiser les résultats et exporter un rapport CSV sans passer par le script directement : https://hal-curation.streamlit.app/ 


## score_interdisciplinarité.py
Script d'attribution d'un score d'interdisciplinarité aux revues dans lesquelles les auteurs d'une structure (actuellement configuré sur 7550) publient. Le script récupère les revues en question, puis attribue un score d'interdiscipliarité basé sur les domaines (domain_s) de l'ensemble des depôts HAL liés à cette revue. Un score d'interdisciplinarité entre 0 et 1 est attribué à la revue en fonction de l'hétérogénéité des domaines disciplinaires (indice de Shannon). Est ensuite calculé le pourcentage des publications dans des revues interdisciplinaires de la structure.
