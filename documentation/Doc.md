# Documentation

## Nutriscore

![bb15644219fa-calcule-du-nutriscore-3.png](img%2Fbb15644219fa-calcule-du-nutriscore-3.png)

ont as un range de -17 à 59 où :
- A est entre -17 et 0
- B est entre 1 et 2
- C est entre 3 et 10
- D est entre 11 et 18
- E est entre 19 et 59

![66faae5aa9a42ce05a1ee42c_667be297df2a03e1eb0a6360_11.png](img%2F66faae5aa9a42ce05a1ee42c_667be297df2a03e1eb0a6360_11.png)

## Modèles choisi pour les tests

On liste les modèles suivants pour être tester sur nos données :
- LightGBM / XGBoost
- CatBoost
- Random Forest
- Logistic Regression
- Réseau de neuronnes

## Traitement apporté au jeu de données

On garde les colonnes suivantes :

Cibles et identification :
    "code", "product_name", "main_category", "nutriscore_grade", "nutriscore_score",

Points négatifs du Nutri-Score :
    "energy_100g", 
    "sugars_100g", 
    "saturated-fat_100g", 
    "sodium_100g", 

Points positifs du Nutri-Score :
    "proteins_100g", 
    "fiber_100g", 
    "fruits-vegetables-legumes_100g",

Variables globales utiles au modèle :
    "fat_100g",
    "carbohydrates_100g"

On retire ensuite les lignes où nutriscore_grade, nutriscore_score et energy_100g 
valent NaN, unknown ou not-applicable ce qui supprime 4148649 lignes


## Models, entrainement et choix

### Random Forest
Après avoir nettoyé et rendu le dataset un maximum utilisable et le plus clair possible, on utilise le modèle RandomForest pour le tester et observer les résultats sur ce dataset. 

On cherche à déterminer ici le nutriscore_grade, la lette qui correspond donc au nutriscore. 

On utilise 100 itérations sur le modèle et on regarde ce que les métriques donnent.

Pour chaque lettre allant de A à E, on retrouve une precision plutôt similaire et stable, aux alentours de 0.90 (avec une allonge allant de 0.84 jusqu'à 0.96),
avec le recall et le f1-score ayant les mêmes valeurs à peu près.

Maintenant on va observer la learning curve de notre modèle, et on peut voir que notre score de validation avoisine les 0.90.

Score :
![RF_CA_score.png](img/RF_CA_score.png)

Grade :
![RF_CA_grade.png](img/RF_CA.png)
Ensuite on va chercher à prédire le nutriscore_score, le score tout simplement.
On obtient un score RMSE de 1713 et un score de R² de 0.969.
Pour la learning curve on obtient un score de validation autour de 0.965.

Globalement, pour les 2 tests, on obtient des scores convaincants et élevés mais le modèle étant un modèle plutôt simple, on pourrai obtenir de meilleurs scores avec des modèles plus complexes. 

### Nutriscore Score

### Nutriscore Grade 
Catboost est le meilleur model
avec comme hyperparamètres :
- iterations=500
- learning_rate=0.05
- depth=6

en 466 itérations ont obtient un test de 0.9951291612 ce qui nous
donne un classification report tel quel :

```
Accuracy : 0.995129161207968Accuracy : 0.995129161207968
              precision    recall  f1-score   support

           a       0.98      0.99      0.99      5285
           b       0.99      0.98      0.99      3612
           c       1.00      1.00      1.00      7905
           d       1.00      1.00      1.00      6719
           e       1.00      1.00      1.00      6248

    accuracy                           1.00     29769
   macro avg       0.99      0.99      0.99     29769
weighted avg       1.00      1.00      1.00     29769
```
Erreurs notables

- Classe 1 est la plus difficile à classer : 80 exemples de la classe 1 sont prédits comme classe 0. C'est l'erreur la plus significative du modèle, suggérant que ces deux classes partagent des caractéristiques proches.
- Classe 0 génère également 27 confusions vers la classe 1, confirmant que la frontière 0/1 est la plus ambiguë.
- Les classes 2, 3 et 4 sont très bien séparées, avec seulement quelques confusions entre voisines (3↔4 notamment : 14 cas).

![catboost_matrix.png](img/catboost_matrix.png)

Le modèle as d'excellentes performances globales, avec une accuracy 
qui atteint 99,3–99,4% aussi bien sur le train que sur le test à 500 itérations.
Avec une convergence rapide en une 100aine d'itérations et une absence de sur apprentissage.

![catboost_learncurve.png](img/catboost_learncurve.png)