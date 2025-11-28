# Exercice 6 — SQL Basics (Supply Chain Essentials)

## 🎯 Objectif
Acquérir les bases SQL nécessaires pour :

- lire les tables
- filtrer les données Supply Chain
- réaliser les premières analyses
- comprendre la structure d’un entrepôt de données

Les exercices utilisent les tables du dossier `05_Datasets/SQL/`.

---

## 🔹 Exercice 1 — Lister les produits
Afficher les 20 premiers produits avec :

- sku  
- catégorie  
- sous-catégorie  
- brand  

```sql
SELECT sku, category, subcategory, brand
FROM dim_product
LIMIT 20;

🔹 Exercice 2 — Liste des pays distincts

Afficher la liste de tous les pays activés dans la Supply Chain.

SELECT DISTINCT country_name, region, market_type
FROM dim_country;

🔹 Exercice 3 — Nombre de clients par segment

SELECT segment, COUNT(*) AS customer_count
FROM dim_customer
GROUP BY segment
ORDER BY customer_count DESC;

🔹 Exercice 4 — Volume total des commandes (toutes périodes)

SELECT SUM(quantity_ordered) AS total_quantity
FROM fact_orders;

🔹 Exercice 5 — Produits stockés dans chaque entrepôt
SELECT warehouse_id,
       COUNT(DISTINCT product_id) AS product_count
FROM fact_inventory
GROUP BY warehouse_id;


---

# ✅ **2) Exercice 7 — SQL Joins (pro Supply Chain)**  
📁 `04_Exercices_SQL/Exercice7_SQLJoins/Exercice7_SQLJoins.md`


# Exercice 7 — SQL Joins (Supply Chain Operations)

## 🎯 Objectif
Apprendre à naviguer entre tables de fait/dimension via les JOINs :

- INNER JOIN
- LEFT JOIN
- RIGHT JOIN
- FULL JOIN

---

## 🔹 Exercice 1 — Commandes avec info produit

```sql
SELECT o.order_id, o.quantity_ordered, p.sku, p.category
FROM fact_orders o
INNER JOIN dim_product p
    ON o.product_id = p.product_id
LIMIT 50;

🔹 Exercice 2 — Inventaire avec entrepôts


SELECT i.snapshot_date_id,
       w.warehouse_code,
       i.product_id,
       i.stock_on_hand
FROM fact_inventory i
LEFT JOIN dim_warehouse w
       ON i.warehouse_id = w.warehouse_id;

🔹 Exercice 3 — Ventes avec pays

SELECT s.quantity_shipped,
       c.country_name,
       c.region
FROM fact_shipments s
INNER JOIN dim_warehouse w
       ON s.warehouse_id = w.warehouse_id
INNER JOIN dim_country c
       ON w.country_id = c.country_id;

🔹 Exercice 4 — Commandes sans livraison

SELECT o.order_id, o.quantity_ordered
FROM fact_orders o
LEFT JOIN fact_shipments s
       ON o.order_id = s.order_id
WHERE s.order_id IS NULL;


🔹 Exercice 5 — Produits jamais commandés

SELECT p.product_id, p.sku
FROM dim_product p
LEFT JOIN fact_orders o
       ON p.product_id = o.product_id
WHERE o.product_id IS NULL;



---

# ✅ **3) Exercice 8 — SQL Indicators (KPIs Supply Chain)**  
📁 `04_Exercices_SQL/Exercice8_SQLIndicators/Exercice8_SQLIndicators.md`


# Exercice 8 — SQL Indicators (KPIs Supply Chain)

## 🎯 Objectif
Construire les principaux KPI opérationnels utilisés par les Demand Planners :

- Lead Time
- Fill Rate
- Stockout Rate
- Turnover
- Capacity Utilization

---

## 🔹 Exercice 1 — Lead Time moyen par entrepôt

```sql
SELECT warehouse_id,
       AVG(lead_time_days) AS avg_lead_time
FROM fact_shipments
GROUP BY warehouse_id;

🔹 Exercice 2 — Fill Rate par client

SELECT o.customer_id,
       SUM(s.quantity_shipped) / SUM(o.quantity_ordered) AS fill_rate
FROM fact_orders o
INNER JOIN fact_shipments s
       ON o.order_id = s.order_id
GROUP BY o.customer_id;


🔹 Exercice 3 — Produits en rupture

SELECT product_id, warehouse_id
FROM fact_inventory
WHERE stock_on_hand = 0;


🔹 Exercice 4 — Rotation des stocks

SELECT product_id,
       SUM(quantity_shipped) /
       AVG(stock_on_hand) AS stock_turnover
FROM fact_shipments s
INNER JOIN fact_inventory i
       ON s.product_id = i.product_id
GROUP BY product_id;


🔹 Exercice 5 — Capacité entrepôt utilisée

SELECT w.warehouse_code,
       AVG(i.stock_on_hand) / w.capacity AS capacity_usage
FROM fact_inventory i
INNER JOIN dim_warehouse w
       ON i.warehouse_id = w.warehouse_id
GROUP BY w.warehouse_code;



---

# ✅ **4) Exercice 9 — SQL Forecast Prep (Data ready for forecasting)**  

📁 `04_Exercices_SQL/Exercice9_SQLForecastPrep/Exercice9_SQLForecastPrep.md`

```markdown
# Exercice 9 — SQL Forecast Prep (Demand Forecasting Ready)

## 🎯 Objectif
Créer une table propre, agrégée et prête à être utilisée pour des modèles de prévision.

On va produire :

- séries temporelles
- volumes mensuels
- nettoyage des outliers
- classification des produits

---

## 🔹 Exercice 1 — Ventes mensuelles par SKU

```sql
SELECT d.year, d.month, p.sku,
       SUM(s.quantity_shipped) AS qty_shipped
FROM fact_shipments s
INNER JOIN dim_date d
    ON s.shipment_date_id = d.date_id
INNER JOIN dim_product p
    ON s.product_id = p.product_id
GROUP BY d.year, d.month, p.sku;


🔹 Exercice 2 — Pondération Prix Moyen par SKU

SELECT p.sku,
       SUM(o.quantity_ordered * o.unit_price) / SUM(o.quantity_ordered)
           AS weighted_price
FROM fact_orders o
INNER JOIN dim_product p
    ON o.product_id = p.product_id
GROUP BY p.sku;


🔹 Exercice 3 — Coefficient de variation (CV)

SELECT p.sku,
       STDDEV(s.quantity_shipped) / AVG(s.quantity_shipped) AS cv
FROM fact_shipments s
INNER JOIN dim_product p
    ON s.product_id = p.product_id
GROUP BY p.sku;


🔹 Exercice 4 — Marquer les outliers (méthode IQR)

SELECT product_id,
       warehouse_id,
       stock_on_hand,
       CASE
            WHEN stock_on_hand > (q3 + 1.5 * iqr) THEN 'Outlier High'
            WHEN stock_on_hand < (q1 - 1.5 * iqr) THEN 'Outlier Low'
            ELSE 'Normal'
       END AS outlier_flag
FROM fact_inventory;


(Note : dans un vrai SQL, il faut utiliser une CTE pour calculer Q1, Q3 et IQR.)


🔹 Exercice 5 — Table Prévision Ready (finale)

Créer une table résumée :

SELECT d.year, d.month, p.sku,
       SUM(s.quantity_shipped) AS qty,
       SUM(s.quantity_shipped * o.unit_price) AS sales_value
FROM fact_shipments s
INNER JOIN fact_orders o
    ON s.order_id = o.order_id
INNER JOIN dim_product p
    ON s.product_id = p.product_id
INNER JOIN dim_date d
    ON s.shipment_date_id = d.date_id
GROUP BY d.year, d.month, p.sku;


Cette table servira dans Power BI ou Python.

