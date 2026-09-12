# 🔍 Détection de fraude électrique par analyse des données de consommation

Projet réalisé lors d'un stage de fin d'études (Master 2 Statistique, Université de
Strasbourg) chez **Électricité de Djibouti (EDD)**, avril–août 2026.

## ⚠️ À propos des données

Pour des raisons de **confidentialité contractuelle**, les données réelles d'EDD ne
peuvent pas être publiées. Ce dépôt utilise un **jeu de données synthétique**,
généré pour reproduire les propriétés statistiques du jeu de données réel :

- mêmes catégories tarifaires et proportions dans le portefeuille
- mêmes ordres de grandeur de consommation, puissance souscrite et asymétrie des distributions
- comportements frauduleux simulés (chutes brutales, surconsommation chronique, instabilité) pour illustrer la capacité de détection du pipeline

**La méthodologie et le code sont ceux effectivement développés et utilisés pendant le
stage.** Les résultats chiffrés obtenus sur les vraies données EDD sont indiqués
ci-dessous à titre de référence, mais ne sont pas reproductibles depuis ce dépôt.

## 🎯 Objectif

En l'absence de labels historiques de fraude, formuler la détection de comportements
frauduleux comme un problème d'**apprentissage non supervisé**, en combinant trois
algorithmes complémentaires :

| Méthode | Principe |
|---|---|
| **K-Means** | Distance au centroïde du cluster assigné + détection de micro-clusters isolés |
| **Isolation Forest** | Isolement algorithmique — une anomalie s'isole plus vite qu'un point normal |
| **Local Outlier Factor (LOF)** | Densité locale — un client isolé au sein de pairs statistiquement similaires |

Les trois scores normalisés sont combinés en un **score composite**, avec un critère
de consensus (détection par ≥ 2 méthodes sur 3) pour prioriser les investigations
terrain.

## 🧪 Pipeline

1. **Audit et nettoyage des données** (doublons, valeurs manquantes, incohérences chronologiques)
2. **Ingénierie des variables** : consommation journalière, ratio de cohérence technique (kWh/j/kVA), coefficient de variation, chute brutale entre cycles
3. **Détection exploratoire univariée** par la méthode IQR, appliquée par catégorie tarifaire
4. **Agrégation par client** (une observation = un client, pas un relevé)
5. **Segmentation du portefeuille** en 3 groupes homogènes (résidentiel, professionnel, forte puissance), validée par test de **Kruskal-Wallis**
6. **Application de K-Means, Isolation Forest et LOF** sur chaque segment
7. **Score composite** et classement par consensus inter-méthodes
8. **Validation externe** contre un registre de fraude

## 📊 Résultats obtenus sur les données réelles (rapport de stage)

- **97 427** relevés propres après nettoyage, **9 853** clients modélisés
- **256 contrats suspects** identifiés (dont **102** détectés par les 3 méthodes simultanément)
- Validation contre un registre externe de **812 cas de fraude confirmée** : 60 recoupent le jeu modélisé
- **Enrichissement d'un facteur ≈ 4,8** du taux de fraude confirmée pour le groupe de consensus à 3 méthodes, par rapport à un contrôle aléatoire du portefeuille

## 🛠️ Outils

Python · Pandas · NumPy · Scikit-learn (KMeans, IsolationForest, LocalOutlierFactor) ·
SciPy (test de Kruskal-Wallis) · Matplotlib · Seaborn · PostgreSQL (extraction des
données réelles, non applicable ici)

## 📁 Contenu du dépôt

- `detection_fraude_edd.ipynb` — notebook complet, exécuté, avec sorties et graphiques
- `donnees_synthetiques_edd.csv` — jeu de données synthétique
- `labels_simules.csv` — statut de fraude simulée (validation illustrative uniquement)

## 👤 Auteur

Souheib Omar Osman — [LinkedIn](https://linkedin.com/in/souheib-omar-osman) · [GitHub](https://github.com/souheibomar939-hue
