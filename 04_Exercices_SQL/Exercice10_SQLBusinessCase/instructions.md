# Exercice 10 — Business Case SQL (Supply Chain & S&OP)

## 🎯 Objectif

Cet exercice est le **projet final** du module SQL.  
Vous devez analyser une Supply Chain complète (demandes, stocks, livraisons, pays, entrepôts) à partir d’une base SQL réaliste.

Le but est de produire :

- 25 requêtes SQL structurées (niveau 1 → niveau 5)  
- 5 fichiers `.sql` thématiques  
- 1 synthèse écrite finale

Ces résultats serviront à alimenter un dashboard Power BI ou un modèle de prévision.

---

# 1. Tables utilisées

Toutes les données se trouvent dans :

05_Datasets/SQL/


Tables fournies :

### Dimensions
- **dim_product**
- **dim_warehouse**
- **dim_customer**
- **dim_country**
- **dim_date**

### Faits
- **fact_orders**
- **fact_inventory**
- **fact_shipments**

---

# 2. Structure du Business Case

L’exercice est divisé en 5 niveaux, chacun dans un fichier `.sql` :

businesscase_level1_exploration.sql
businesscase_level2_kpi_core.sql
businesscase_level3_advanced.sql
businesscase_level4_segments.sql
businesscase_level5_final_views.sql


Chaque niveau contient **5 requêtes**, soit **25 requêtes** au total.

---

# 3. Détails des niveaux et des requêtes attendues

## 🟦 Niveau 1 — Exploration de la Supply Chain (5 requêtes)

1. Nombre total de commandes par pays  
2. Top 20 clients par chiffre d’affaires  
3. Top 20 SKU par volume commandé  
4. Distribution des segments clients (Retail / Wholesale / Autre)  
5. Analyse des lead times (MIN / MAX / AVG) par entrepôt  

---

## 🟩 Niveau 2 — KPI Supply Chain Core (5 requêtes)

6. Taux de rupture par pays et SKU  
7. Fill rate global (quantité livrée / commandée)  
8. Fill rate par client (tri décroissant)  
9. Stock Turnover par SKU / entrepôt  
10. Value of Stockouts (CA perdu sur ruptures)  

---

## 🟧 Niveau 3 — Analyses avancées multi-niveaux (5 requêtes)

11. Taux de service par pays et mois  
12. Utilisation de la capacité entrepôt  
13. Identification des entrepôts en surstock (>90 %)  
14. Produits à risque (fort stock + faibles ventes + ruptures fréquentes)  
15. Analyse pays “instables” (CV de la demande)  

---

## 🟥 Niveau 4 — Segmentation ABC/XYZ + Analyse Clients à Risque (5 requêtes)

16. Classement des SKU par CA (RANK)  
17. Attribution d’une classe ABC  
18. Calcul du coefficient de variation (CV) par SKU  
19. Attribution de la classe XYZ  
20. Identification des clients “VIP à risque” (CA haut + ruptures)  

---

## 🟫 Niveau 5 — Vues finales (5 requêtes)

Produire des vues SQL réutilisables dans Power BI :

21. `view_kpi_country_month` (CA, Stockouts, Service Rate)  
22. `view_kpi_warehouse` (Capacity Usage, Turnover, Active SKU)  
23. `view_sku_segments` (ABC + XYZ + CA + QTY)  
24. `view_client_risk` (ruptures, CA)  
25. `view_forecast_ready` (mois, pays, SKU, quantités, CA)  

---

# 4. Livrable final — Rapport business (rapport_businesscase.md)

À partir des requêtes, produire un rapport written contenant :

- Résumé des ruptures par pays  
- Analyse des entrepôts (saturation vs sous-utilisation)  
- Top 20 SKU critiques  
- Segmentation ABC + XYZ  
- Clients VIP à risque  
- 3 recommandations S&OP prêtes à présenter au directeur Supply Chain

---

# 5. Qualité attendue

- Requêtes **indentées**, **commentées**, **structurées**
- Pas de `SELECT *`
- Alias lisibles (`total_sales`, `service_rate`, etc.)
- Utilisation de :
  - `JOIN`
  - `CTE`
  - `WINDOW FUNCTIONS`  
  - `CASE WHEN`

---



