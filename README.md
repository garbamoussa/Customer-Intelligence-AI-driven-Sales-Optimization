# Customer Intelligence & AI-driven Sales Optimization

Projet pédagogique de AI CONTENT & DATA

Pôle Léonard de Vinci : Campus de l’Arche

## Objectifs

- Explorer les sources PDF et Excel du cas « Le Grand Marché ».
- Nettoyer et enrichir les transactions.
- Construire une segmentation exploratoire des clients.
- Générer des contenus marketing contextualisés par segment.
- Contrôler la qualité et la fiabilité des contenus IA.
- Mesurer les KPI marketing et comparer des variantes de campagne.

## Données du cas

Le projet a été préparé à partir de deux documents fournis dans le cadre du TP :

- `P2_Rancane_rapport_mensuel_LGM.pdf`
- `P2_Rancane_Le_rapport_sur_les_clients_affiliés.xlsx`

**Important :** les fichiers sources ne sont volontairement pas inclus dans ce dépôt Git. Ils peuvent contenir des données commerciales ou clients. Placez-les localement dans `data/raw/` après vérification des droits de diffusion. Ne poussez jamais de données clients réelles ou confidentielles vers un dépôt public.

Le classeur comporte une feuille `DATA` avec 660 lignes et les colonnes observées :
`ID client`, `Temps d'achat`, `Montant`, `Categorie`.

Le champ `Temps d'achat` doit être interprété avec prudence : son unité et sa définition métier doivent être confirmées auprès de la source.

### Limites connues du jeu de données

- Aucune date de transaction : la récence RFM ne peut pas être calculée.
- Les transactions ne contiennent que deux catégories : elles ne permettent pas de reconstituer toutes les catégories évoquées dans le rapport.
- Pas de données de navigation par session, d'abandon de panier, de canal d'acquisition ni de données de campagne.
- Les KPI de campagne et les tests A/B ne peuvent être calculés sur les sources seules. Utiliser des données expérimentales ou simulées en les identifiant explicitement comme telles.

## Installation

Python 3.10+ recommandé.

```bash
python -m venv .venv
# Windows PowerShell
.venv\Scripts\Activate.ps1
# macOS / Linux
source .venv/bin/activate

pip install -r requirements.txt
```

## Mise en place des sources

Copiez les deux documents dans `data/raw/` en conservant les noms :

```text
data/raw/
├── P2_Rancane_rapport_mensuel_LGM.pdf
└── P2_Rancane_Le_rapport_sur_les_clients_affiliés.xlsx
```

Le fichier Excel est utilisé par le pipeline. Le PDF sert à l'analyse métier et à la confrontation des hypothèses.

## Exécution du pipeline

Depuis la racine du projet :

```bash
python -m src.pipeline
```

Le pipeline :
1. charge la feuille `DATA` du classeur ;
2. contrôle et nettoie les champs ;
3. agrège les transactions au niveau client ;
4. calcule des indicateurs descriptifs ;
5. produit une segmentation K-Means exploratoire lorsque les dépendances sont disponibles ;
6. enregistre les résultats dans `data/processed/`.

## Lancer l'application

```bash
streamlit run app/streamlit_app.py
```

L'application propose :
- un tableau de bord commercial ;
- une vue de segmentation ;
- un générateur de prompts marketing ;
- une grille de contrôle qualité ;
- un calculateur de KPI de campagne.

## Arborescence

```text
customer-intelligence-sales/
├── app/
│   └── streamlit_app.py
├── data/
│   ├── raw/                 # sources locales, ignorées par Git
│   ├── processed/           # résultats locaux, ignorés par Git
│   └── sample/              # données de démonstration synthétiques
├── notebooks/
│   └── 01_demarrage.ipynb
├── reports/
│   └── rapport_template.md
├── src/
│   ├── __init__.py
│   ├── data.py
│   ├── segmentation.py
│   ├── prompts.py
│   ├── quality.py
│   ├── metrics.py
│   └── pipeline.py
├── tests/
│   └── test_metrics.py
├── .gitignore
├── requirements.txt
└── README.md
```

## Missions pédagogiques

| Mission | Travail | Livrable |
|---|---|---|
| 1. Sourcing | Inventorier les sources et les limites | Dictionnaire de données |
| 2. Data preparation | Nettoyer et enrichir | Dataset client |
| 3. Customer Intelligence | Analyse descriptive et clustering | Segments documentés |
| 4. Stratégie éditoriale | Associer besoins, segments et contenus | Calendrier éditorial |
| 5. Prompt Engineering | Concevoir et itérer les prompts | Bibliothèque de prompts |
| 6. Adaptation éditoriale | Décliner par cible et canal | Charte et contenus |
| 7. Fiabilité IA | Vérifier les affirmations et la cohérence | Grille de contrôle |
| 8. Performance | KPI, A/B test, optimisation | Dashboard et recommandations |

## Méthode de segmentation

La segmentation proposée est exploratoire. Elle utilise des variables disponibles (nombre d'achats, chiffre d'affaires, panier moyen et temps moyen d'achat). Le nombre de clusters doit être justifié et les profils doivent être interprétés à partir des centroïdes et des distributions.

Ne pas présenter les clusters comme des catégories naturelles ou définitives. Évaluer leur stabilité, leur utilité métier et les biais potentiels.

## Génération IA

Le projet fournit un constructeur de prompt, pas une connexion automatique à un fournisseur LLM. L'étudiant peut copier le prompt dans un modèle autorisé ou ajouter un connecteur après avoir configuré une clé API via une variable d'environnement. Ne jamais stocker de clé dans le code ou Git.

Les contenus doivent être validés par une personne avant diffusion. Ne pas inférer de données sensibles ni exposer d'informations individuelles.

## Tests

```bash
pytest -q
```

## Initialiser un dépôt GitHub

```bash
git init
git add .
git commit -m "Initial commit - Customer Intelligence Sales TP"
git branch -M main
git remote add origin https://github.com/<utilisateur>/customer-intelligence-sales.git
git push -u origin main
```

Créer de préférence un dépôt privé pour le travail étudiant et ne publier que des données synthétiques ou explicitement autorisées.

## Barème indicatif

- Collecte et sourcing : 10 %
- Nettoyage et enrichissement : 15 %
- Analyse et segmentation : 20 %
- Prompt Engineering : 15 %
- Adaptation éditoriale : 10 %
- Fiabilité et validation : 10 %
- KPI et optimisation : 15 %
- Application et soutenance : 5 %

## Avertissement

Ce dépôt est un support pédagogique. Les données de démonstration sont synthétiques. Les résultats obtenus ne constituent pas des recommandations commerciales réelles.
