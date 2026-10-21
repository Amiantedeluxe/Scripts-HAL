# Scripts-HAL

Collection de scripts python utilisables seuls et modulables pour faciliter la curation et faire des analyses bibliométriques à partir de l'API.

## scipt_curation_v3.py

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

Script d'analyse bibliométrique qui propose une mesure de degré d'interdisciplinarité des revues dans lesquelles publient les membres d'une structure HAL.

### Principe

Pour une structure donnée (identifiée par son `structId` HAL), le script :

1. Récupère toutes les revues dans lesquelles la structure a publié, au-dessus d'un seuil minimal d'articles
2. Pour chaque revue, interroge HAL global pour obtenir la distribution des domaines disciplinaires (`level0_domain_s`) de l'ensemble des dépôts liés à cette revue
3. Calcule un **score de Shannon normalisé** (∈ [0, 1]) mesurant l'hétérogénéité disciplinaire de la revue
4. Calcule le pourcentage des publications de la structure réalisées dans des revues considérées comme interdisciplinaires

### Score de Shannon normalisé

- **0** — revue mono-disciplinaire pure (tous les articles dans un seul domaine)
- **1** — revue parfaitement interdisciplinaire (distribution uniforme entre tous les domaines)

Une revue est considérée comme interdisciplinaire si son score dépasse le seuil `INTER_THRESHOLD` (0.4 par défaut). Ce seuil ne correspond à rien de précis et peut-être modifié. Le score permet surtout d'obtenir un aperçu à gros trait du caractère interdisciplinaire ou non de certaines revues.

### Paramètres configurables

| Paramètre | Valeur par défaut | Description |
|---|---|---|
| `STRUCT_ID` | `7550` | Identifiant de la structure dans HAL |
| `MIN_ARTICLES` | `50` | Nombre minimum d'articles pour qu'une revue soit analysée |
| `INTER_THRESHOLD` | `0.4` | Seuil Shannon au-dessus duquel une revue est dite interdisciplinaire |
| `PAUSE` | `0.2` | Délai entre les requêtes API (en secondes) |

### Sortie

Le script affiche la progression en console et exporte les résultats dans un fichier `hal_results.json` contenant :

- Les paramètres utilisés
- Un résumé (nombre de revues analysées, pourcentage interdisciplinaire)
- Le détail par revue : titre, nombre d'articles local et HAL global, distribution des domaines, score Shannon

