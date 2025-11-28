# Exercice 4 — Data Cleaning Avancé (Power Query)

## 🎯 Objectif
Nettoyer un dataset ERP “sale” contenant des erreurs réelles :
- dates impossibles
- types incohérents
- texte dans des colonnes numériques
- SKUs invalides
- doublons
- lignes incomplètes
- espaces parasites
- formatages corrompus

Cet exercice reproduit exactement les cas vécus par les équipes Supply Chain lorsqu’elles reçoivent des exports ERP / WMS de mauvaise qualité.

---

# 1. Contexte métier

Dans de nombreuses entreprises :
- Les équipes IT génèrent des extractions non homogènes.
- Les formats varient selon les utilisateurs.
- Les erreurs humaines créent des données partiellement corrompues.
- Les champs obligatoires ne sont pas toujours remplis.

Les impacts :
- KPI faux (Service Level, OTD…)
- Prévisions impossibles
- Mauvaise compréhension des volumes
- Décisions erronées en S&OP
- Perte de confiance dans la donnée

Votre objectif :  
👉 produire un dataset propre, fiable et exploitable.

---

# 2. Données fournies

Dans `05_Datasets/Exercice4_Cleaning/`, vous trouverez :

raw_orders_corrupted.xlsx


Ce fichier contient volontairement :

### 🟥 Erreurs de DATE
- valeurs impossibles : `2025-18-72`
- texte dans les dates : `"Janvier-2023"`
- formats multiples : `12/03/23`, `2023-03-12`, `3.12.2023`

### 🟥 Erreurs de QUANTITY
- quantité en texte : `"12 unités"`
- valeurs négatives
- valeurs vides
- outliers extrêmes (ex : 42 000)

### 🟥 Erreurs SKU
- `NULL`
- `""`
- `SKU-???`
- `"X-SKU"`
- espaces début/fin

### 🟥 Doublons
- lignes identiques répétées
- IDs répétés mais quantités différentes

### 🟥 Champs incohérents
- SupplierID mix texte / numéro
- colonnes mélangées

---

# 3. Travail attendu (Étapes)

## Étape 1 — Analyse structurelle
- Inspecter types
- Identifier colonnes corrompues
- Repérer patterns d’erreurs

## Étape 2 — Nettoyage du SKU
- trim
- mise en majuscule
- remplacement des SKU invalides par null
- filtrage des SKU corrompus

## Étape 3 — Nettoyage des dates
- détecter les dates impossibles
- convertir 100 % des dates dans un format standard
- corriger ou remplacer les erreurs :
  - si réparable → convertir
  - sinon → null

## Étape 4 — Quantités
- retirer les unités (“12 unités” → 12)
- remplacer les valeurs négatives par null
- filtrer les outliers extrêmes
- convertir en entier

## Étape 5 — Doublons
- repérer les doublons
- conserver la ligne avec la date la plus récente
- documenter votre règle métier

## Étape 6 — Structuration finale
- forcer les types corrects
- trier par OrderDate
- supprimer lignes invalides

---

# 4. Livrable attendu

Un fichier final nommé :

PQ_Cleaning_Output.xlsx


Onglets :
- `Raw_Cleaned`
- `Report_Errors_Detected` (optionnel mais recommandé)

---

# 5. Objectifs pédagogiques
- Nettoyage avancé de données Supply Chain
- Gestion des erreurs réelles ERP
- Détection et correction automatique
- Amélioration de la qualité pour le forecasting
- Préparation des données avant Power Pivot / Modélisation

---

# 6. Difficulté
⭐⭐⭐⭐⭐ — Avancé
