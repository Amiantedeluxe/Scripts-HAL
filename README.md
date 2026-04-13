# Scripts-HAL

Collection de scripts python utilisables seuls et modulables pour faciliter la curation et faire des analyses bibliométriques à partir de l'API.

## script_curation_v1.py
Script de flagging automatique de notices hal problématiques. A partir d'un .txt en entrée contenant une liste de notices, pensé pour être extrait des mails de notification de nouveaux dépôts, le script requête les notices et repères de potentielles irrégularités (doublons potentiels, affiliations auteurs manquantes, doi manquants...). 6 règles de flagging sont implémentées mais possibilité d'en ajouter / retirer. 

## score_interdisciplinarité.py
Script d'attribution d'un score d'interdisciplinarité aux revues dans lesquelles les auteurs d'une structure (actuellement configuré sur 7550) publient. Le script récupère les revues en question, puis attribue un score d'interdiscipliarité basé sur les domaines (domain_s) de l'ensemble des depôts HAL liés à cette revue. Un score d'interdisciplinarité entre 0 et 1 est attribué à la revue en fonction de l'hétérogénéité des domaines disciplinaires (indice de Shannon). Est ensuite calculé le pourcentage des publications dans des revues interdisciplinaires de la structure.
