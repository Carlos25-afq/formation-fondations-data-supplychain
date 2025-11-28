# Exercice 1 — Analyse des Lead Times Fournisseurs (Excel)

## 🎯 Objectif
Analyser la performance des fournisseurs sur 12 mois : lead time réel, écart par rapport au lead time attendu, retards, fiabilité, et %OTD (On Time Delivery).

Cet exercice simule le travail d’un(e) Supply Chain Analyst chargé(e) de mesurer la qualité du service fournisseur.

---

# 1. Données fournies

Vous trouverez ou générerez les fichiers suivants :

### 📁 A. Fichier `fournisseurs.csv`
Colonnes :
- `SupplierID`
- `SupplierName`
- `Country`
- `Category` (Raw Materials, Packaging, Others)
- `ExpectedLeadTime_Days`

### 📁 B. 12 fichiers de commandes :  
Dans `05_Datasets/Exercice1_LeadTime/` :


Colonnes :
- `OrderID`
- `OrderDate`
- `DeliveryDate`
- `SupplierID`
- `SKU`
- `Quantity`

---

# 2. Tâches à réaliser

## Étape 1 — Consolider les données
- Importer les 12 fichiers mensuels dans un seul tableau Excel.
- Ajouter une colonne `Month` (1 à 12 ou Jan–Déc).
- Vérifier les types de données.

## Étape 2 — Calculer le Lead Time réel
Créer la colonne :
LeadTime_Real_Days = DeliveryDate - OrderDate

Attention : format date obligatoire.

## Étape 3 — Joindre les données fournisseurs
À partir de `SupplierID`, récupérer :
- `SupplierName`
- `ExpectedLeadTime_Days`

## Étape 4 — Déterminer si la commande est dans les délais
Créer une colonne :
OnTime = IF(LeadTime_Real_Days <= ExpectedLeadTime_Days; 1; 0)


## Étape 5 — Construire les KPI par fournisseur
Créer un tableau ou un TCD contenant :
- Lead Time moyen
- Lead Time max et min
- %OTD = SUM(OnTime) / COUNT(Orders)
- Nombre de commandes
- Écart moyen entre LT réel et LT attendu

## Étape 6 — Classer les fournisseurs
- Identifier les 10 pires fournisseurs (%OTD le plus bas).
- Identifier les 10 meilleurs.
- Mettre une mise en forme conditionnelle.

## Étape 7 — Créer un mini-dashboard
Onglet `Dashboard` :
- Graphique %OTD par fournisseur
- Top 3 meilleurs fournisseurs
- Top 3 moins fiables
- Carte KPI : lead time moyen global

---

# 3. Livrables attendus

Créer un fichier :

### 📁 `LeadTime_Analysis.xlsx`

Feuilles obligatoires :
- `Raw_Data` : données consolidées
- `KPI_Fournisseurs` : TCD ou tableau final
- `Dashboard` : graphiques + KPI

Déposer les livrables dans :  
`06_Solutions/Exercice1_LeadTime/`

---

# 4. Difficulté estimée
⭐⭐⭐✩  
Objectifs : consolidation, nettoyage, KPI, mini-dashboard.

---

# 5. Objectifs pédagogiques
Cet exercice vous apprend à :
- Consolider plusieurs fichiers
- Calculer un lead time réel
- Évaluer des fournisseurs
- Construire des indicateurs logistiques
- Réaliser un mini-dashboard d’analyse
