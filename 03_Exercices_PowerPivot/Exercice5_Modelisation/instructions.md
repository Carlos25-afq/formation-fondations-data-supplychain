# Exercice 5 — Modélisation Power Pivot (Schéma en Étoile) & Analyse Supply Chain

## 🎯 Objectif
Construire un modèle de données professionnel pour analyser les ventes, les stocks et les performances logistiques à grande échelle.  
Vous apprendrez à créer :
- un schéma en étoile (Star Schema)
- des relations fact/dimension
- des mesures DAX propres
- un modèle analytique exploitable pour le Demand Planning et le S&OP

---

# 1. Contexte métier

Votre entreprise opère dans :
- 14 pays,
- 6 entrepôts,
- 12 000 SKU
- plusieurs canaux (Retail, Wholesale, E-commerce).

Les équipes utilisent aujourd’hui des fichiers Excel volumineux et fragmentés.  
Vous devez construire un **modèle unifié**, basé sur Power Pivot, permettant de répondre aux questions critiques :

### Questions métiers :
- Quel est le chiffre d’affaires par SKU, pays, catégorie, warehouse ?
- Quels sont les Top/Bottom SKU en volume et valeur ?
- Quel est le taux de rotation des stocks ?
- Quels pays surstockent ?
- Quels produits souffrent de ruptures fréquentes ?
- Comment évoluent les ventes mensuelles en tendance ?
- Quel est le “ABC ranking” des SKU ?

---

# 2. Datasets fournis

Vous trouverez les fichiers dans :

05_Datasets/Exercice5_Modelisation/


### 📁 1. Table des faits (massive)
**fact_sales.xlsx**  
✔️ 80 000 à 130 000 lignes  
✔️ Colonnes :
- Date
- SKU
- Warehouse
- Country
- Quantity
- UnitPrice
- SalesAmount
- Channel
- OnHand
- StockOutFlag

Poids : 15–20 Mo max

---

### 📁 2. Dimension Produits
**dim_product.xlsx**  
✔️ 12 000 SKU  
✔️ Colonnes :
- SKU
- Category
- SubCategory
- Brand
- UnitCost
- LifeCycleStage
- LaunchDate

---

### 📁 3. Dimension Date
**dim_date.xlsx**  
✔️ 10 ans de dates  
✔️ Colonnes :
- Date
- Year
- Quarter
- Month
- Week
- Day
- IsHoliday
- Season

---

### 📁 4. Dimension Pays / Marché
**dim_country.xlsx**
✔️ Colonnes :
- Country
- Region
- Currency
- FXRate (vs EUR)
- MarketType

---

### 📁 5. Dimension Entrepôts
**dim_warehouse.xlsx**
✔️ Colonnes :
- Warehouse
- Country
- Capacity
- Manager

---

# 3. Travail attendu

## Étape 1 — Nettoyage minimal
- convertir dates
- normaliser SKU
- convertir quantités en entier
- vérifier SalesAmount = Quantity × UnitPrice

## Étape 2 — Construction du Star Schema

Relations obligatoires :

dim_date[Date] → fact_sales[Date]
dim_product[SKU] → fact_sales[SKU]
dim_warehouse[Warehouse] → fact_sales[Warehouse]
dim_country[Country] → fact_sales[Country]


## Étape 3 — Création des mesures DAX

Mesures obligatoires :

### Ventes :
- `Total Sales := SUM(fact_sales[SalesAmount])`
- `Total Quantity := SUM(fact_sales[Quantity])`

### KPIs Supply Chain :
- `Avg Price := AVERAGE(fact_sales[UnitPrice])`
- `Stock Coverage (days) := ...`
- `Stockout Rate := DIVIDE(CALCULATE(COUNTROWS(fact_sales), fact_sales[StockOutFlag]=1), COUNTROWS(fact_sales))`

### ABC Ranking :
- `Sales Rank := RANKX(ALL(dim_product[SKU]), [Total Sales], , DESC)`
- `ABC Class := IF([Sales Rank] <= TotalSKU*0.2, "A", IF(...))`

### Rotation des stocks :
- `Stock Turnover := DIVIDE([Total Quantity], AVERAGE(fact_sales[OnHand]))`

## Étape 4 — Création du report final
Créer un dashboard comprenant :

- Ventes par catégorie / sous-catégorie  
- Top 15 SKU  
- Bottom 15 SKU  
- Ventilation par pays  
- ABC Ranking  
- Courbe Vente Mensuelle  
- Analyse Stock vs Vente  

---

# 4. Livrables

### 📌 1. Modèle Power Pivot :

PowerPivot_Model.xlsx


### 📌 2. Dashboard :


---

# 5. Objectifs pédagogiques

- Comprendre le rôle des dimensions et des faits  
- Construire un modèle analytique performant  
- Utiliser DAX de manière propre et professionnelle  
- Créer des indicateurs Supply Chain pertinents  
- Automatiser les analyses S&OP / Demand Planning  

---

# 6. Difficulté
⭐⭐⭐⭐⭐ — Expert
