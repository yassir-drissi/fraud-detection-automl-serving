# Modele manuel ou modele AutoML ? (TP05)

| Critere                    | Manuel (rf 500/16)          | AutoML (FLAML 90 s)                  |
|----------------------------|-----------------------------|--------------------------------------|
| Famille de modele          | foret aleatoire             | xgboost (XGBClassifier)              |
| F1 sur le jeu de test      | 0.8669                      | 0.8582                               |
| Temps de recherche machine | 1.5 s (un seul entrainement)| 90.6 s                               |
| Temps humain (estimation)  | ~1 h (reglage manuel TP2)   | ~5 min (lancer le script)            |
| Latence mediane (ms)       | 56.9 / 55.8                 | 25.4 / 8.4                           |
| Latence p95 (ms)           | 100.3 / 220.8               | 32.6 / 33.4                          |
| Taille du fichier (Mo)     | 13.37                       | 0.29                                 |
| Explicabilite              | importance des variables ; 500 arbres de profondeur 16, illisibles un par un | importance des variables (gain) ; reglages cfg_ visibles dans MLflow ; boosting moins intuitif |

Latences : deux mesures de 200 appels chacune, en local.

## Decision (5 lignes) : quel modele mettre en production, et pourquoi ?

Je mets en production le modele AutoML (fraude-automl, xgboost).
Il perd 0.0087 de F1 (environ 1 %), un ecart faible face a ses autres atouts.
Il est 46 fois plus leger (0.29 Mo contre 13.37 Mo), ce qui compte pour l'image Docker.
Il repond 2 a 7 fois plus vite, avec un p95 stable autour de 33 ms contre 100 a 220 ms.
Le modele manuel reste dans le registre : un deplacement d'alias suffit pour revenir en arriere.
