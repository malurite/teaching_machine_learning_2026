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
- Réseau de neuronnes ()

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
