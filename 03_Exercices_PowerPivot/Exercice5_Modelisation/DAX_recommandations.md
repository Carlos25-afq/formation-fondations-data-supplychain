# 📘 Recommandations DAX — Modélisation Supply Chain & S&OP
Ce document est la référence officielle des mesures DAX à utiliser dans l’Exercice 5.  
Toutes les formules sont expliquées, commentées et replacées dans un contexte métier Supply Chain.

---

# 🟦 1. Mesures de Base : Volume & Chiffre d’Affaires

### 🧮 Total Sales  
**Objectif métier :** Calculer le chiffre d’affaires total.  
**Variables utilisées :**  
- `SalesAmount` = `Quantity × UnitPrice` (déjà calculé dans la fact table)

```DAX
Total Sales :=
SUM(fact_sales[SalesAmount])
-- Retourne la somme du montant des ventes
-- SUM() → agrégation simple adaptée pour les colonnes numériques


📦 Total Quantity

Objectif métier : Volume total vendu (Demand Planning)

Total Quantity :=
SUM(fact_sales[Quantity])
-- Volume total vendu sur la période filtrée

💲 Average Unit Price

Objectif métier : Vérifier la cohérence pricing, identifier des anomalies, analyser le mix prix.

Avg Price :=
AVERAGE(fact_sales[UnitPrice])
-- Moyenne du prix unitaire indépendamment du volume

🟩 2. Stock, Ruptures et Rotation (KPIs Supply Chain)
📦 Stock On Hand Total

Objectif : Quantité en stock disponible dans les entrepôts.

Stock On Hand :=
SUM(fact_sales[OnHand])
-- Quantité totale en stock dans la période sélectionnée

🚨 Stockout Count

Objectif : Nombre de lignes où le stock est à zéro (rupture)

Stockout Count :=
COUNTROWS(
    FILTER(fact_sales, fact_sales[StockOutFlag] = 1)
)
-- Compte les lignes où StockOutFlag est égal à 1

❗ Stockout Rate

Objectif : Taux de rupture (fréquence des lignes en stock-out)

Stockout Rate :=
DIVIDE(
    [Stockout Count],
    COUNTROWS(fact_sales)
)
-- DIVIDE() évite les divisions par zéro

🔄 Stock Turnover (Rotation des stocks)

Objectif : KPI S&OP pour évaluer la vitesse d'écoulement des produits
Formule métier : Ventes annuelles / Stock moyen

Stock Turnover :=
DIVIDE(
    [Total Quantity],
    AVERAGE(fact_sales[OnHand])
)
-- Plus la rotation est élevée, moins il y a de stock immobilisé

📆 Coverage Days (Durée de couverture approximative)

Objectif : Nombre de jours que le stock actuel peut couvrir.

Coverage Days :=
DIVIDE(
    AVERAGE(fact_sales[OnHand]),
    AVERAGE(fact_sales[Quantity])
)
-- Nbre de jours couverts par le stock moyen selon la consommation moyenne

🟧 3. Time Intelligence (YoY, MoM, tendances)
🔁 Sales PY (previous year)

Objectif : Comparer les performances N/N-1

Sales PY :=
CALCULATE(
    [Total Sales],
    SAMEPERIODLASTYEAR(dim_date[Date])
)
-- SAMEPERIODLASTYEAR() → base des comparaisons YoY

📉 Sales MoM (Month-over-Month)
Sales MoM :=
DIVIDE(
    [Total Sales]
        - CALCULATE([Total Sales], DATEADD(dim_date[Date], -1, MONTH)),
    CALCULATE([Total Sales], DATEADD(dim_date[Date], -1, MONTH))
)
-- Variation mensuelle en pourcentage

📈 Sales YoY (%)
Sales YoY :=
DIVIDE(
    [Total Sales] - [Sales PY],
    [Sales PY]
)
-- Variation du CA d’une année sur l’autre

🟨 4. ABC Classification (Analyse de criticité)
🔢 Rank des SKU (sur CA)
Sales Rank :=
RANKX(
    ALL(dim_product[SKU]),
    [Total Sales],
    ,
    DESC
)
-- RANKX() classe les SKU du plus vendu au moins vendu

🔢 Nombre total de SKU
SKU Count :=
DISTINCTCOUNT(dim_product[SKU])
-- Nombre unique de SKU actifs dans la sélection

🅰🅱🅲 Classe ABC
ABC Class :=
SWITCH(
    TRUE(),
    [Sales Rank] <= [SKU Count] * 0.2, "A", -- 20 % = 80 % des ventes
    [Sales Rank] <= [SKU Count] * 0.5, "B",
    "C"
)

🟥 5. XYZ Classification (Variabilité de la demande)
📊 Coefficient de variation

Objectif : mesurer la stabilité de la demande.
Formule métier : écart-type / moyenne

CV :=
DIVIDE(
    STDEVX.P(fact_sales, fact_sales[Quantity]),
    AVERAGEX(fact_sales, fact_sales[Quantity])
)
-- STDEVX.P → écart-type population

🎚️ Classe XYZ

X = très stable

Y = moyennement stable

Z = très irrégulier

XYZ Class :=
SWITCH(
    TRUE(),
    [CV] < 0.5, "X",
    [CV] < 1.0, "Y",
    "Z"
)

🟫 6. Country / Channel / Warehouse Analysis
🌍 Sales by Country
Sales by Country :=
CALCULATE([Total Sales], ALLEXCEPT(dim_country, dim_country[Country]))

🛒 Sales by Channel
Sales by Channel :=
SUMX(
    FILTER(fact_sales, fact_sales[Channel] = SELECTEDVALUE(fact_sales[Channel])),
    fact_sales[SalesAmount]
)

🟪 7. Profit & Cost Measures
💰 Gross Margin
Gross Margin :=
SUMX(
    fact_sales,
    fact_sales[SalesAmount] -
    (fact_sales[Quantity] * RELATED(dim_product[UnitCost]))
)

📉 Gross Margin %
Gross Margin % :=
DIVIDE([Gross Margin], [Total Sales])

🟦 8. KPIs S&OP Avancés
🔁 Sell-Through (écoulement réel)
Sell Through :=
DIVIDE(
    [Total Quantity],
    [Total Quantity] + [Stock On Hand]
)

🏭 Inventory Value
Inventory Value :=
SUMX(
    fact_sales,
    fact_sales[OnHand] * RELATED(dim_product[UnitCost])
)

💥 Value of Stockouts
Value of Stockouts :=
SUMX(
    FILTER(fact_sales, fact_sales[StockOutFlag] = 1),
    fact_sales[SalesAmount]
)

🧠 Best Practices (à maîtriser absolument)

✔ Toujours préférer les mesures aux colonnes calculées
✔ Utiliser CALCULATE pour contrôler le contexte
✔ Utiliser DIVIDE au lieu de /
✔ Valider les relations (Many-to-One obligatoire entre dimensions et fact)
✔ Toujours utiliser les dimensions dans les visuels
✔ Ajouter des mesures formatées (FORMAT()) pour le rendu final


