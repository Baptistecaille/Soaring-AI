# Soaring AI - Prédicteur de Conditions de Vol à Voile

## 📋 Description du projet

Soaring AI est un système d'intelligence artificielle qui prédit les conditions favorables au vol à voile en combinant :
- **Images satellites météorologiques** (données visuelles des conditions atmosphériques)
- **Données SYNOP** (observations météorologiques numériques)
- **Données de vols réels** (historique des vols effectués)

Le modèle utilise un réseau de neurones hybride combinant CNN (Convolutional Neural Network) pour l'analyse d'images et MLP (Multi-Layer Perceptron) pour les données météorologiques numériques.

## 🏗️ Architecture du projet

```
.
├── data/                           # Données d'entrée
│   ├── airports.csv               # Liste des aérodromes
│   ├── flights.csv               # Données de vols (étiquettes)
│   ├── flights_analysis.csv      # Analyse des vols par jour
│   └── pilots.csv                # Base de données des pilotes
├── cropped_images/                # Images satellites prétraitées
├── synop_data/                    # Données météorologiques SYNOP
│   └── synop_final.csv           # Données SYNOP consolidées
├── synop_files/                   # Fichiers de configuration SYNOP
│   └── indexHeaders.csv          # Description des variables SYNOP
├── model/                         # Modèles entraînés
├── data_validation/               # Images de validation
├── __pycache__/                   # Cache Python
└── .vscode/                       # Configuration VS Code
```

## 📊 Fichiers principaux

### Scripts Python
- **[`train_model.py`](train_model.py)** : Script principal d'entraînement du modèle hybride
- **[`test_single_day.py`](test_single_day.py)** : Test du modèle sur une date spécifique
- **[`data_check.py`](data_check.py)** : Vérification de la cohérence des données

### Notebooks Jupyter
- **[`fetchNotbook.ipynb`](fetchNotbook.ipynb)** : Récupération et prétraitement des images satellites
- **[`Fetch_data.ipynb`](Fetch_data.ipynb)** : Récupération des données de vols
- **[`Fetch_synop.ipynb`](Fetch_synop.ipynb)** : Récupération des données météorologiques SYNOP
- **[`Data_visualization.ipynb`](Data_visualization.ipynb)** : Visualisation et analyse des données

## 🔧 Installation et configuration

### Prérequis
```bash
pip install tensorflow
pip install pandas
pip install numpy
pip install matplotlib
pip install seaborn
pip install scikit-learn
pip install pillow
pip install requests
pip install beautifulsoup4
```

### Structure des données requises

1. **Images satellites** : Format JPG, nommées `YYYY-MM-DD_HH:MM:SS.jpg`
2. **Données SYNOP** : CSV avec colonnes météorologiques standardisées
3. **Données de vols** : CSV avec colonnes `Date` et `Flyable` (0/1)

## 🚀 Utilisation

### Entraînement du modèle
```bash
python train_model.py
```

### Test sur une date spécifique
```bash
python test_single_day.py
```

### Vérification des données
```bash
python data_check.py
```

## 🧠 Architecture du modèle

Le modèle hybride combine deux branches :

### Branche CNN (Images satellites)
- **Base** : EfficientNetB0 préentraîné sur ImageNet
- **Prétraitement** : Redimensionnement vers 224x224, normalisation
- **Couches** : GlobalAveragePooling2D → Dense(256) → Dense(128)

### Branche MLP (Données SYNOP)
- **Entrée** : Caractéristiques météorologiques numériques
- **Architecture** : Dense(64) → Dense(32) → Dense(16)
- **Normalisation** : RobustScaler pour gérer les valeurs aberrantes

### Fusion et sortie
- **Fusion** : Concaténation des sorties CNN et MLP
- **Couches finales** : Dense(64) → Dense(1, sigmoid)
- **Sortie** : Probabilité binaire (volable/non volable)

## 📈 Variables SYNOP utilisées

Le modèle utilise plusieurs variables météorologiques :
- **Pression** (`pres`) : Pression atmosphérique
- **Vent** (`ff`, `dd`) : Force et direction du vent
- **Température** (`t`, `td`) : Température et point de rosée
- **Humidité** (`u`) : Humidité relative
- **Nébulosité** (`nbas`, `hbas`, `nnuage1`) : Couverture nuageuse
- **Précipitations** (`rr12`) : Précipitations sur 12h
- **Température du sol** (`tminsol`) : Température minimale du sol

## 📊 Évaluation du modèle

Le modèle est évalué avec les métriques suivantes :
- **Accuracy** : Précision globale
- **Precision** : Précision pour la classe positive
- **Recall** : Rappel pour la classe positive
- **F1-Score** : Moyenne harmonique precision/recall
- **AUC** : Aire sous la courbe ROC

## 🎯 Fonctionnalités

### Prédiction des conditions
- **Entrée** : Date spécifique
- **Sortie** : 
  - Probabilité de conditions favorables (0-1)
  - Décision binaire (favorable/défavorable)
  - Niveau de confiance (%)
  - Description textuelle

### Gestion des données
- **Images** : Prétraitement automatique et recadrage
- **SYNOP** : Nettoyage et normalisation des données météo
- **Équilibrage** : Gestion du déséquilibre des classes

## 📝 Format des données

### Images satellites
```
Nom de fichier : YYYY-MM-DD_HH:MM:SS.jpg
Heures requises : 09:00, 12:00, 15:00
```

### Données SYNOP
```csv
date,numer_sta,pres,ff,t,td,u,nbas,hbas,tminsol,nnuage1,rr12
2024-06-01_09:00:00,7027,101620.0,8.0,287.25,281.25,73.0,5.0,800.0,285.95,8,0.0
```

### Données de vols
```csv
Date,Flyable
2024-06-01,1
2024-06-02,0
```

## 🔮 Améliorations futures

- [ ] Utilisation des 3 images (9h, 12h, 15h) au lieu d'une seule
- [ ] Intégration de données météorologiques supplémentaires
- [ ] Optimisation des hyperparamètres
- [ ] Prédiction de la qualité des conditions (échelle 1-5)
- [ ] Interface web pour les utilisateurs finaux

## 👥 Contributeurs

- Baptiste Caillerie - Développeur principal

## 📄 Licence

Ce projet est sous licence [MIT](LICENSE).

---

**Note** : Ce système est conçu comme aide à la décision. Les pilotes doivent toujours consulter les bulletins météorologiques officiels avant tout vol.
