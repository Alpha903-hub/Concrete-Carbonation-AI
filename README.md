# Concrete Carbonation AI
### Open scientific engineering project | Projet ouvert de recherche et d'ingénierie scientifique

**A traceable data-to-model workflow for studying concrete carbonation depth.**  
**Une chaîne traçable, des données expérimentales à la prédiction de la profondeur de carbonatation.**

**Status / Statut:** research prototype — active development / prototype de recherche — développement actif  
**Languages / Langues:** English and French

---

# ENGLISH

## 1. Project purpose

Concrete carbonation is a coupled physicochemical process involving CO₂ transport through the pore network, dissolution in pore water, and reactions with alkaline constituents of cementitious materials. Its progression depends on material composition, curing and preconditioning, exposure conditions, moisture state, temperature, and time.

**Concrete Carbonation AI** investigates how experimental information reported in scientific literature can be organized, audited, and used to train machine-learning models for estimating carbonation depth.

The project is deliberately open to inspection and criticism. It publishes not only model scores, but also data provenance, processing artifacts, validation design, prediction errors, and known limitations.

This is a research prototype. It is not a validated engineering design tool, a substitute for laboratory measurements, or a service-life certification method.

## 2. Research questions

1. What is the exact observational unit in the source dataset: mix, specimen, exposure-time measurement, or another entity?
2. How does the processed analytical dataset relate to the original RILEM supplementary material?
3. Which variables, unit conversions, exclusions, and harmonizations were applied, and why?
4. Does the model generalize to source studies excluded from training?
5. Which studies and conditions account for the largest errors?
6. Can a language model extract explicitly reported experimental inputs from scientific text without inventing missing values?

## 3. Scientific source and data provenance

Vollpracht, A. et al. (2024). *Report of RILEM TC 281-CCC: insights into factors affecting the carbonation rate of concrete with SCMs revealed from data mining and machine learning approaches*. **Materials and Structures**, 57, 206.  
DOI: [10.1617/s11527-024-02465-0](https://doi.org/10.1617/s11527-024-02465-0)

The supplied archive includes the article PDF and supplementary workbook `RILEM_TC281_CCC_supplementary_workbook.xlsx`. The workbook has four sheets: `gerneral information`, `chem. analysis`, `phys. charakterisation`, and `references`. Original sheet spelling is retained.

The current analytical file `carbonation_cleaned_for_eda.csv` contains 9,359 rows × 72 columns and 88 distinct `source_reference` values. The related `carbonation_longitudinal.csv` contains 9,359 rows × 56 columns. These are processed project artifacts, not the untouched RILEM workbook.

**Important:** 9,359 rows must not be described as 9,359 independent concrete mixes. The exact row-level observational unit and the relationship between source records, mixes, and repeated exposure-time measurements must be established in `docs/data/OBSERVATIONAL_UNIT.md`.

## 4. Data transformation: known and pending

The repository preserves the supplied source and processed artifacts. However, the exact chronological transformation history from the RILEM workbook to `carbonation_cleaned_for_eda.csv` is not yet fully reconstructed from a single deterministic script.

Therefore, this project does **not** claim that every difference between the original and processed files has already been justified. The transformation log is explicitly marked as pending reconstruction.

The audit must establish:
- source and processed row/column counts;
- what each row represents;
- key fields and joins between sheets;
- variable mapping and renamed fields;
- unit conversions;
- missing-value handling;
- duplicate/repeated-measurement handling;
- exclusions and their reasons;
- derived variables;
- before/after distributions and missingness.

An unreported value is unknown, not zero. Zero is valid only when the source explicitly reports zero or an absence with a defined numerical meaning.

## 5. Modeling and evaluation

The supplied experiment record reports five-fold `GroupKFold` grouped by `source_reference`, with 9,359 out-of-fold observations and 88 studies. No source study was shared across folds.

| Metric | Baseline | Random Forest |
|---|---:|---:|
| MAE (mm) | 3.3756 | **2.1198** |
| RMSE (mm) | 5.1540 | **3.9012** |
| R² | -0.0909 | **0.3750** |

Reported pooled MAE reduction: **37.20%** relative to the baseline.

### Study-specific failure: FEL15

The supplied study-level evaluation reports 1,021 FEL15 observations:
- Random Forest MAE: 5.4642 mm
- Random Forest RMSE: 7.3447 mm
- Random Forest R²: -7.7135
- Bias (prediction − observed): +5.3699 mm

This is a material failure, not a footnote. It shows that pooled performance does not guarantee transferability to each source study. The all-study result remains the headline result. Any analysis excluding FEL15 must be labeled sensitivity analysis and must not replace the all-study metrics.

Prediction-level and study-level CSV files are included for inspection. Exact model configuration, baseline definition, preprocessing fitted within each fold, and fold-generation code must be checked against the notebook before claiming complete independent reproducibility.

## 6. Language-model extraction

The supplied notebook contains a Qwen3-8B extraction workflow followed by Python normalization and a Random Forest prediction step.

The intended contract is:
- extract only values explicitly stated in the source text;
- preserve missing/unreported values as `null`;
- attach literal evidence excerpts to extracted fields;
- perform unit conversion deterministically in Python;
- never infer or generate the measured carbonation-depth target;
- keep observed targets and model predictions separate.

A successful integration example is a smoke test, not scientific validation. The extraction module requires evaluation against manually annotated paragraphs from held-out studies, including field-level extraction metrics and evidence checks.

## 7. Repository contents

- `data/source/`: supplied RILEM article and supplementary workbook.
- `data/processed/`: supplied cleaned and restructured datasets.
- `data/evaluation/`: supplied out-of-fold predictions and per-study metrics.
- `notebooks/`: working Colab notebook snapshot, with outputs removed.
- `docs/science/`: scientific writing plan.
- `docs/data/`: provenance and data-audit protocol.
- `docs/methodology/`: model and validation protocol.
- `docs/extraction/`: Qwen extraction protocol.
- `src/`: starter validation utilities.
- `tests/`: basic tests for data contracts.

## 8. Reproduction

Start with `notebooks/00_project_working_notebook.ipynb` in Google Colab. Upload or mount the supplied data files from `data/processed/` when prompted. This is a working research notebook, not yet a fully refactored package; run it top-to-bottom in a fresh runtime and record missing dependencies or stateful-cell dependencies.

For the data audit, read `docs/data/RECONSTRUCTION_PLAN.md` first. Do not claim the raw-to-processed transformation is reproducible until that work is complete.

## 9. Licence and attribution

The original code developed for this project is distributed under the MIT License. See the `LICENSE` file for the terms governing its use, modification, and redistribution.

The reference scientific article is published under the Creative Commons Attribution 4.0 International (CC BY 4.0) license. The supplementary workbook and derived datasets are subject to a separate review of their redistribution terms.

The MIT License covering the project's original code does not automatically apply to third-party articles, datasets, or other external resources.

## 10. Contribution

Contributions are welcome in scientific documentation, data auditing, variable mapping, unit validation, missingness and repeated-measurement audits, study-aware validation, residual analysis, and extraction annotation. Read `CONTRIBUTING.md` before proposing changes.

---

# FRANÇAIS

## 1. Objectif du projet

La carbonatation du béton est un phénomène physico-chimique couplé impliquant le transport du CO₂ dans le réseau poreux, sa dissolution dans l'eau interstitielle et des réactions avec les constituants alcalins des matériaux cimentaires. Sa progression dépend notamment de la composition du matériau, de la cure, du préconditionnement, des conditions d'exposition, de l'humidité, de la température et du temps.

**Concrete Carbonation AI** étudie comment les informations expérimentales de la littérature scientifique peuvent être organisées, auditées et utilisées pour entraîner des modèles d'apprentissage automatique estimant la profondeur de carbonatation.

Le dépôt est ouvert à l'inspection et à la critique. Il documente les données, les transformations, la validation, les erreurs et les limites connues, et pas uniquement les scores du modèle.

Il s'agit d'un prototype de recherche, pas d'un outil de dimensionnement validé, d'un substitut aux mesures de laboratoire ou d'une méthode de certification de durée de vie.

## 2. Questions de recherche

1. Quelle est l'unité exacte d'observation : formulation, éprouvette, mesure à un âge d'exposition ou autre entité ?
2. Comment la base analytique préparée se rattache-t-elle au supplément RILEM original ?
3. Quelles variables, conversions d'unités, exclusions et harmonisations ont été appliquées, et pourquoi ?
4. Le modèle généralise-t-il à des études sources absentes de l'entraînement ?
5. Quelles études et conditions expliquent les erreurs les plus importantes ?
6. Un modèle de langage peut-il extraire des paramètres explicitement rapportés sans inventer les valeurs absentes ?

## 3. Source scientifique et provenance

Vollpracht, A. et al. (2024). *Report of RILEM TC 281-CCC: insights into factors affecting the carbonation rate of concrete with SCMs revealed from data mining and machine learning approaches*. **Materials and Structures**, 57, 206.  
DOI : [10.1617/s11527-024-02465-0](https://doi.org/10.1617/s11527-024-02465-0)

L'archive fournie contient le PDF de l'article et le classeur supplémentaire `RILEM_TC281_CCC_supplementary_workbook.xlsx`. Celui-ci comporte quatre feuilles : `gerneral information`, `chem. analysis`, `phys. charakterisation` et `references`. Les graphies originales sont conservées.

Le fichier analytique `carbonation_cleaned_for_eda.csv` contient 9 359 lignes × 72 colonnes et 88 valeurs distinctes de `source_reference`. Le fichier longitudinal associé contient 9 359 lignes × 56 colonnes. Ce sont des artefacts préparés du projet, et non le classeur RILEM brut.

**Attention :** 9 359 lignes ne signifient pas 9 359 formulations indépendantes. L'unité d'observation et la relation entre enregistrements, formulations et mesures répétées doivent être établies dans `docs/data/OBSERVATIONAL_UNIT.md`.

## 4. Transformations : éléments connus et travail restant

Le dépôt conserve les fichiers source et les fichiers analytiques transmis. Cependant, l'historique chronologique exact des transformations entre le classeur RILEM et `carbonation_cleaned_for_eda.csv` n'est pas encore entièrement reconstruit à partir d'un script déterministe unique.

Le projet ne prétend donc pas que toutes les différences entre les fichiers originaux et préparés ont déjà été justifiées. Le journal de transformation est marqué comme incomplet.

L'audit doit établir les dimensions avant/après, l'unité d'observation, les clés et jointures, la correspondance des variables, les conversions, les valeurs manquantes, les doublons/mesures répétées, les exclusions, les variables dérivées et les distributions comparées.

Une valeur non rapportée est inconnue, et non égale à zéro. Zéro n'est valide que si la source le rapporte explicitement ou indique une absence ayant une signification numérique définie.

## 5. Modélisation et évaluation

Les résultats transmis rapportent une validation croisée à cinq plis `GroupKFold`, groupée par `source_reference`, avec 9 359 prédictions hors pli et 88 études. Aucune étude source n'est partagée entre les plis.

| Indicateur | Baseline | Random Forest |
|---|---:|---:|
| MAE (mm) | 3,3756 | **2,1198** |
| RMSE (mm) | 5,1540 | **3,9012** |
| R² | -0,0909 | **0,3750** |

Réduction de MAE rapportée : **37,20 %** par rapport à la baseline.

### Échec spécifique à l'étude FEL15

L'évaluation fournie rapporte 1 021 observations FEL15 :
- MAE Random Forest : 5,4642 mm ;
- RMSE Random Forest : 7,3447 mm ;
- R² Random Forest : -7,7135 ;
- biais (prédiction − observation) : +5,3699 mm.

Cet échec est important. Il montre qu'une performance globale ne garantit pas la transférabilité à chaque étude. Le résultat toutes études confondues reste le résultat principal. Toute analyse excluant FEL15 doit être présentée comme une analyse de sensibilité.

Les fichiers de prédictions et de métriques par étude sont fournis pour inspection. La configuration exacte du modèle, la baseline, le prétraitement ajusté à l'intérieur de chaque pli et le code de génération des plis doivent être vérifiés avant de revendiquer une reproductibilité indépendante complète.

## 6. Extraction par modèle de langage

Le notebook transmis contient un flux d'extraction Qwen3-8B, suivi d'une normalisation Python et d'une prédiction Random Forest.

Le contrat visé est d'extraire uniquement les valeurs explicitement présentes, de conserver les valeurs non rapportées sous `null`, d'associer des extraits littéraux, de convertir les unités de façon déterministe en Python, de ne jamais générer la profondeur mesurée et de séparer observations et prédictions.

Un exemple réussi est un test d'intégration, pas une validation scientifique. Il faut un ensemble de paragraphes annotés manuellement provenant d'études tenues à l'écart, avec métriques par champ et contrôle des preuves.

## 7. Contenu du dépôt

- `data/source/` : article RILEM et classeur supplémentaire fournis.
- `data/processed/` : jeux de données nettoyés et restructurés.
- `data/evaluation/` : prédictions hors pli et métriques par étude.
- `notebooks/` : instantané du notebook de travail Google Colab, sans les sorties.
- `docs/science/` : plan de rédaction scientifique.
- `docs/data/` : protocole de provenance et d'audit des données.
- `docs/methodology/` : protocole de modélisation et de validation.
- `docs/extraction/` : protocole d'extraction Qwen.
- `src/` : utilitaires de validation.
- `tests/` : tests des contrats de données.

## 8. Reproduction

Commencer par `notebooks/00_project_working_notebook.ipynb` dans Google Colab. Téléverser ou monter les données de `data/processed/` lorsque le notebook le demande. Ce notebook est un support de travail, pas encore un package entièrement refactorisé : l'exécuter de haut en bas dans un runtime vierge et noter les dépendances et cellules dépendantes de l'état du runtime.

Lire `docs/data/RECONSTRUCTION_PLAN.md` avant de prétendre que la transformation du fichier brut vers la base préparée est reproductible.

## 9. Licence et attribution

Le code original développé pour ce projet est distribué sous licence MIT. Consulter le fichier `LICENSE` pour connaître les conditions d'utilisation, de modification et de redistribution.

L'article scientifique de référence est publié sous licence Creative Commons Attribution 4.0 International (CC BY 4.0). Le classeur supplémentaire et les jeux de données dérivés font l'objet d'une vérification distincte de leurs conditions de redistribution.

La licence MIT couvrant le code original du projet ne s'applique pas automatiquement aux articles, jeux de données ou autres ressources externes.

## 10. Contribution

Les contributions sont les bienvenues dans les domaines de la documentation scientifique, de l'audit des données, de la correspondance des variables, de la validation des unités, de l'analyse des valeurs manquantes et des mesures répétées, de la validation par étude source, de l'analyse des résidus et de l'annotation des données d'extraction.

Lire `CONTRIBUTING.md` avant de proposer des modifications.
