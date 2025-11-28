# Exercice 6 — SQL End-to-End (Supply Chain & Demand Planning)

## 🎯 Objectif général

Construire, explorer et analyser une base de données SQL complète pour la Supply Chain, depuis les commandes jusqu’aux ventes, en passant par les stocks et les entrepôts.

Vous allez :

- Créer le schéma de base de données (tables fact & dimension)
- Charger des données réalistes (datasets fournis)
- Écrire des requêtes de complexité croissante (niveau 1 à 5)
- Répondre à un vrai “Business Case” SQL Supply Chain

Cet exercice simule ce que fait un Data / Demand / S&OP Analyst dans une entreprise FMCG / Agroalimentaire.

---

## 1️⃣ Contexte métier

Vous travaillez pour une entreprise internationale qui :

- livre des produits dans plusieurs pays
- gère plusieurs entrepôts
- suit les commandes clients, les niveaux de stocks et les ventes
- doit sécuriser la disponibilité produit tout en limitant le surstock

L’équipe Data vous fournit trois grandes tables de faits + des dimensions :

- `fact_orders` : commandes clients (demand)
- `fact_inventory` : stocks journaliers (on hand)
- `fact_shipments` : expéditions / livraisons
- `dim_product`, `dim_warehouse`, `dim_customer`, `dim_country`, `dim_date`

Votre mission :  
👉 utiliser SQL pour répondre aux questions clés de performance Supply Chain.

---

## 2️⃣ Fichiers fournis dans cet exercice

Dans `05_Datasets/Exercice6_SQL_EndToEnd/` (à créer plus tard), vous aurez :

- `fact_orders.csv`
- `fact_inventory.csv`
- `fact_shipments.csv`
- `dim_product.csv`
- `dim_warehouse.csv`
- `dim_customer.csv`
- `dim_country.csv`
- `dim_date.csv`

Dans ce dossier (SQL) :

- `schema.sql` : script de création des tables
- `queries_level1_basic.sql`
- `queries_level2_intermediate.sql`
- `queries_level3_advanced_kpi.sql`
- `queries_level4_window_functions.sql`
- `queries_level5_business_case.sql`

---

## 3️⃣ Schéma logique (à implémenter en SQL)

### Tables de dimensions

- `dim_product(product_id, sku, category, subcategory, brand, unit_cost)`
- `dim_warehouse(warehouse_id, warehouse_code, country_id, capacity)`
- `dim_customer(customer_id, customer_name, segment, country_id)`
- `dim_country(country_id, country_name, region, market_type)`
- `dim_date(date_id, date, year, month, quarter, week, is_holiday)`

### Tables de faits

- `fact_orders(order_id, order_date_id, customer_id, product_id, warehouse_id, quantity_ordered, unit_price, order_status)`
- `fact_inventory(snapshot_date_id, product_id, warehouse_id, stock_on_hand)`
- `fact_shipments(shipment_id, shipment_date_id, order_id, product_id, warehouse_id, quantity_shipped, lead_time_days)`

Toutes les jointures se font via les clés id (FK) vers les dimensions.

---

## 4️⃣ Organisation des requêtes SQL

Les requêtes sont organisées par niveaux, dans des fichiers séparés.

### 🔹 Niveau 1 — Requêtes de base (fichier : `queries_level1_basic.sql`)

Objectif : se familiariser avec le schéma.

Exemples de requêtes demandées :

1. Lister les **10 premiers produits** avec leur catégorie et marque.
2. Extraire la liste distincte des **pays** et leur type de marché (`market_type`).
3. Compter le nombre total de **clients** par segment.
4. Calculer, pour une période donnée, le **volume total commandé**.
5. Obtenir, pour chaque entrepôt, le **nombre de produits distincts stockés**.

---

### 🔹 Niveau 2 — Jointures & Agrégations (fichier : `queries_level2_intermediate.sql`)

Objectif : naviguer entre les tables et faire des agrégations métier.

Exemples :

6. Volume total vendu par **pays & année**.
7. Top 10 **SKU par chiffre d’affaires**.
8. **Stock moyen** par entrepôt & catégorie produit.
9. Volume commandé par **channel client** (si segment ≈ channel).
10. Nombre de commandes par **statut** (`order_status`).

---

### 🔹 Niveau 3 — KPIs Supply Chain (fichier : `queries_level3_advanced_kpi.sql`)

Objectif : construire des indicateurs métiers directement en SQL.

Exemples :

11. Calculer le **Lead Time moyen** par entrepôt (à partir de `fact_shipments`).
12. Estimer le **Stockout potentiel** : lignes où `stock_on_hand = 0`.
13. Calculer le **fill rate** (quantité livrée / quantité commandée) par client.
14. Identifier les **produits à faible rotation** (stock élevé & ventes faibles).
15. Calculer le **taux de service** par pays et mois.

---

### 🔹 Niveau 4 — Fenêtres & Analyses avancées (fichier : `queries_level4_window_functions.sql`)

Objectif : utiliser les fonctions analytiques (window functions).

Exemples :

16. Calculer un **running total** des ventes par SKU et par mois.
17. Calculer le **classement des produits** par volume dans chaque catégorie.
18. Calculer la **variation mensuelle** des ventes par pays (%).
19. Identifier les **clients TOP 20 par CA** et leur part dans le total.
20. Calculer l’**âge moyen du stock** par entrepôt.

---

### 🔹 Niveau 5 — Business Case final (fichier : `queries_level5_business_case.sql`)

Objectif : répondre à une vraie question stratégique Supply Chain.

**Cas métier :**  
Le directeur Supply Chain veut savoir :

- Quels **pays** souffrent le plus de ruptures ?
- Dans quels **entrepôts** le stock est surdimensionné ?
- Quels sont les **Top 20 SKU** responsables de 80 % du CA (loi de Pareto) ?
- Quels clients ont un profil **“haut CA mais souvent en rupture”** ?

Les requêtes finales doivent produire :

21. Un tableau `Pays / Mois / Taux de rupture`
22. Un tableau `Entrepôt / Taux d’utilisation capacité (approx. via stock moyen / capacity)`
23. Un tableau `SKU / CA / Rang / Classe ABC`
24. Un tableau `Client / CA / Nombre de ruptures subies`
25. Une table de synthèse que vous utiliserez pour créer un dashboard (hors SQL).

---

## 5️⃣ Livrables attendus

- `schema.sql` : script CREATE TABLE complet.
- `queries_level1_basic.sql` → 5 requêtes de base.
- `queries_level2_intermediate.sql` → 5 requêtes intermédiaires.
- `queries_level3_advanced_kpi.sql` → 5 requêtes KPI Supply Chain.
- `queries_level4_window_functions.sql` → 5 requêtes avec fonctions analytiques.
- `queries_level5_business_case.sql` → 5 requêtes orientées décisionnel.

---

## 6️⃣ Outils recommandés

- PostgreSQL, MySQL, SQL Server ou SQLite.
- Outil client : DBeaver, DBeaver Lite, Azure Data Studio, Beekeeper, etc.

---

## 7️⃣ Règles de qualité attendues

- Requêtes lisibles (indentation, alias clairs).
- Pas de `SELECT *` dans les livrables finalisés.
- Alias explicites (`total_qty`, `avg_lead_time`, etc.).
- Commentaires SQL (`--`) pour documenter les KPI difficiles.
- Scripts réutilisables dans un contexte pro.

---

Ce module SQL est conçu pour simuler un vrai environnement d’Analyste Supply Chain / Demand Planner.


# Module SQL — Supply Chain & Demand Planning (End-to-End)

Ce module regroupe 4 exercices progressifs (06 → 09) + un Business Case final (10).  
Il couvre l’ensemble des compétences SQL nécessaires dans une Supply Chain moderne :

- extraction de données (SQL Basics)
- jointures opérationnelles (SQL Joins)
- calcul d’indicateurs métier (SQL Indicators)
- préparation des données pour les prévisions (Forecast Prep)
- analyse complète et décisions stratégiques (Business Case final)

## Structure du module

| Exercice | Compétence | Description |
|---------|------------|-------------|
| 06 | SQL Basics | Sélections, filtres, ordres, DISTINCT, COUNT |
| 07 | SQL Joins | JOINS complexes : INNER/LEFT/RIGHT/FULL |
| 08 | SQL Indicators | KPIs Supply Chain : Lead Time, Fill Rate, Stockout Rate |
| 09 | Forecast Prep | Préparation des données pour la prévision |
| 10 | Business Case SQL | Étude complète sur commandes/stock/ventes |

---

## Jeux de données utilisés

Les exercices SQL utilisent les datasets dans :

05_Datasets/SQL/


Contenu prévu :
- fact_orders.csv
- fact_inventory.csv
- fact_shipments.csv
- dim_product.csv
- dim_warehouse.csv
- dim_customer.csv
- dim_country.csv
- dim_date.csv

---

## Objectifs pédagogiques

À la fin de ce module, l’apprenant saura :

1. Écrire des requêtes SQL professionnelles
2. Construire des indicateurs Supply Chain :  
   - Lead Time  
   - Fill Rate  
   - Stockout Rate  
   - Turnover  
3. Préparer les données pour la prévision (Forecasting-ready)
4. Naviguer dans un schéma dimensionnel (star schema)
5. Produire un cas d’usage complet d’analyse Supply Chain

---

## Pour aller plus loin

Ce module peut être connecté ensuite à :

- Power BI (modèle tabulaire)
- Python (feature engineering)
- Spark (gros volumes)
- Data Warehouse (Snowflake, BigQuery…)


