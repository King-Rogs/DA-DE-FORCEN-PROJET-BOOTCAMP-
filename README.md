# Pipeline de Données de Mobilité Urbaine et Pollution

## Description du Projet

Ce projet implémente un pipeline de données complet pour analyser les données de mobilité urbaine et de pollution. Le pipeline traite des données brutes issues d'un fichier Excel, effectue un nettoyage et des transformations, puis exporte les données nettoyées. Une analyse exploratoire des données (EDA) est également incluse pour visualiser les tendances et corrélations.

Le pipeline est conçu pour être modulaire, réutilisable et facile à comprendre, avec une documentation claire pour faciliter la maintenance et l'extension.

## Structure du Projet

```
bootcamp_data_pipeline/
├── data/
│   ├── raw/
│   │   └── mobility_urban_pollution_300.xlsx  # Données brutes d'entrée
│   └── processed/
│       └── mobility_clean.csv                  # Données nettoyées en sortie
├── notebooks/
│   └── pipeline.ipynb                          # Notebook principal contenant le pipeline et l'EDA
└── README.md                                   # Cette documentation
```

## Prérequis

- Python 3.8 ou supérieur
- Bibliothèques Python :
  - pandas
  - matplotlib
  - seaborn
  - openpyxl (pour lire les fichiers Excel)

## Installation

1. Clonez ce repository ou téléchargez les fichiers dans votre environnement local.

2. Installez les dépendances Python :
   ```bash
   pip install pandas matplotlib seaborn openpyxl
   ```

3. Assurez-vous que le fichier de données brutes `mobility_urban_pollution_300.xlsx` est placé dans le dossier `data/raw/`.

## Utilisation

### Exécution du Pipeline

Le pipeline est entièrement contenu dans le notebook `notebooks/pipeline.ipynb`. Pour l'exécuter :

1. Ouvrez le notebook dans Jupyter Notebook, VS Code ou tout éditeur compatible.
2. Exécutez les cellules dans l'ordre séquentiel.

#### Contenu du Notebook (Cellule par Cellule)

- **Cellule 1 (Python)** : Importation de pandas et définition de la fonction `data_pipeline(input_path, output_path)`.
- **Cellule 2 (Markdown)** : Titre "Exécution du pipeline".
- **Cellule 3 (Python)** : Appel de la fonction `data_pipeline` avec les chemins :
  - Entrée : `"../data/raw/mobility_urban_pollution_300.xlsx"`
  - Sortie : `"../data/processed/mobility_clean.csv"`

Le pipeline traite le fichier Excel et génère le CSV nettoyé.

### Fonction `data_pipeline(input_path, output_path)`

Basée sur la Cellule 1, cette fonction exécute :

1. **Ingestion** : `df = pd.read_excel(input_path)`
2. **Normalisation des colonnes** : `df.columns = df.columns.str.lower().str.replace(" ", "_")`
3. **Suppression des NaN** : `df = df.dropna()`
4. **Transformation date** (si colonne 'date' existe) :
   - `df['date'] = pd.to_datetime(df['date'])`
   - `df['hour'] = df['date'].dt.hour`
   - `df['day'] = df['date'].dt.day_name()`
5. **Export** : `df.to_csv(output_path, index=False)`
6. Affichage : `"Pipeline exécuté avec succès ✅"`
7. Retour : `df`

### Analyse Exploratoire des Données (EDA)

Les cellules suivantes (à partir de la Cellule 4) contiennent l'EDA complète :

#### Analyse Univariée
- **Cellule 5 (Markdown)** : "### ANALYSE UNIVARIÉE"
- **Cellule 6 (Markdown)** : "#### Mesures descriptives (variables numériques)"
- **Cellule 7 (Python)** : `df.describe()` - Statistiques descriptives.
- **Cellule 8 (Python)** : Histogramme de `traffic_density` avec matplotlib.
- **Cellule 9 (Python)** : Histogramme de `speed_kmh`.
- **Cellule 10 (Python)** : Histogramme de `traffic_density` (dupliqué dans le notebook).
- **Cellule 11 (Python)** : Histogramme de `air_quality_index`.
- **Cellule 12 (Python)** : Diagramme en barres de `weather.value_counts()`.

#### Agrégation des Variables
- **Cellule 13 (Markdown)** : "### Agrégation des variables"
- **Cellule 14 (Python)** : Vitesse moyenne par météo : `df.groupby("weather")["speed_kmh"].mean()` avec bar plot.
- **Cellule 15 (Python)** : AQI moyen par route : `df.groupby("route_id")["air_quality_index"].mean()` avec bar plot.

#### Analyse Bivariée
- **Cellule 16 (Markdown)** : "## Analyse Bivariée"
- **Cellule 17 (Python)** : Matrice de corrélation pour `["speed_kmh", "traffic_density", "air_quality_index", "latitude", "longitude"]`.
- **Cellule 18 (Markdown)** : "### Nuage de points"
- **Cellule 19 (Python)** : Scatter plot : `traffic_density` vs `speed_kmh`.
- **Cellule 20 (Python)** : Scatter plot : `air_quality_index` vs `speed_kmh`.
- **Cellule 21 (Python)** : Scatter plot : `traffic_density` vs `air_quality_index`.
- **Cellule 22 (Python)** : Scatter plot : `timestamp` vs `traffic_density` (note : le code utilise `timestamp`, mais la colonne est probablement `date` transformée).

#### Visualisation Finale
- **Cellule 23 (Python)** : Calcul de `traffic_mean = df.groupby("route_id")["traffic_density"].mean().reset_index()` et renommage.
- **Cellule 24 (Python)** : Bar plot avec Seaborn pour la densité moyenne par route.

## Données

### Format d'Entrée
- Fichier Excel (.xlsx) contenant les colonnes suivantes (exemples) :
  - `date` : Date et heure des observations
  - `speed_kmh` : Vitesse en km/h
  - `traffic_density` : Densité du trafic
  - `air_quality_index` : Indice de qualité de l'air
  - `weather` : Conditions météorologiques
  - `route_id` : Identifiant de la route
  - `latitude`, `longitude` : Coordonnées géographiques

### Format de Sortie
- Fichier CSV nettoyé avec les colonnes normalisées et les transformations appliquées.

## Personnalisation

- **Chemins de fichiers** : Modifiez les chemins `input_path` et `output_path` dans l'appel de fonction pour pointer vers vos propres fichiers.
- **Étapes du pipeline** : La fonction `data_pipeline` peut être étendue pour inclure des transformations supplémentaires (filtrage, agrégations, etc.).
- **Visualisations** : Les graphiques de l'EDA peuvent être personnalisés ou étendus selon les besoins d'analyse.

## Dépannage

- **Erreur de fichier non trouvé** : Vérifiez que les chemins relatifs sont corrects par rapport au répertoire de travail du notebook.
- **Problèmes de dépendances** : Assurez-vous que toutes les bibliothèques sont installées avec les versions compatibles.
- **Données manquantes** : Si les données contiennent beaucoup de valeurs nulles, ajustez la stratégie de nettoyage dans la fonction.


