# Analyse scRNA-seq : Embryogenèse précoce de *Paracentrotus lividus*

Ce dépôt contient les pipelines bioinformatiques développés pour l'analyse de données de séquençage d'ARN en cellule unique (scRNA-seq) aux stades 8 et 16 cellules de l'embryon d'oursin méditerranéen (*Paracentrotus lividus*). 

Ce projet a été réalisé à l'Institut de Biologie Valrose (iBV) au sein de l'équipe "Réseaux génétiques, axes embryonnaires et morphogenèse".

## Outils et Prérequis

Le traitement primaire des données brutes et l'analyse en aval reposent sur les environnements et packages suivants :
* **Cell Ranger (v10.0.0)** : Alignement sur le génome de référence personnalisé et quantification des transcrits (UMIs).
* **R (v4.5.2) & RStudio** : Environnement de traitement des matrices.
* **Seurat (v5.4.0)** : Package principal pour le contrôle qualité, la normalisation et le clustering.
* **DoubletFinder** : Identification et suppression des doublets techniques.

## Pipeline d'Analyse (Seurat)

Le flux de travail détaillé dans les scripts R suit des étapes rigoureuses de filtrage et de réduction de dimensionnalité afin de maximiser le rapport signal/bruit :

1. **Contrôle Qualité (QC) et Filtrage personnalisé :**
   * Rétention des cellules présentant un minimum de gènes détectés (`nFeature_RNA` > 2000).
   * Tolérance stricte pour les transcrits mitochondriaux (`percent.mt` < 1%).
   * Seuil minimal adaptatif de transcrits totaux (`nCount_RNA` > 500 initialement, puis adapté aux stades).
2. **Normalisation :** Application de `SCTransform` pour stabiliser la variance, corriger les biais liés à la profondeur de séquençage et isoler les 3000 gènes les plus variables.
3. **Nettoyage :** Retrait des doublets via la simulation de cellules artificielles (DoubletFinder).
4. **Réduction de dimension et Clustering :** 
   * Analyse en Composantes Principales (ACP) : sélection des 10 premières composantes (CP) validées par l'Elbow plot.
   * La résolution de clustering a été fixée à `SCT_snn_res.0.5` pour garantir la stabilité de la partition finale des données.
   * Visualisation non linéaire via UMAP pour identifier la ségrégation des blastomères (micromères, lignage neural, région dorsale, etc.).


**Geena Djebli**  
Master 1 Bioinformatique et Biologie Computationnelle
