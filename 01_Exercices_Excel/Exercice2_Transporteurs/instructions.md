# Exercice 2 — Analyse de la Performance Transporteurs (Excel)

## 🎯 Objectif
Analyser la qualité de service de plusieurs transporteurs : retards, variabilité des délais, coût par kilomètre, et créer un mini-dashboard.

Cet exercice simule la mission d’un(e) Analyst Transport ou Customer Logistics.

---

# 1. Données fournies

Vous utiliserez les fichiers suivants :

### 📁 A. `transporteurs.csv`
Colonnes :
- `TransporterID`
- `TransporterName`
- `Country`
- `AvgCostPerKm`

### 📁 B. `livraisons_2023.csv`
Colonnes :
- `DeliveryID`
- `TransporterID`
- `DepartureDate`
- `ArrivalDate`
- `Distance_km`
- `Cost`
- `SKU`
- `Quantity`

Ces données simulent des livraisons internationales sur une année.

---

# 2. Tâches à réaliser

## Étape 1 — Nettoyer les données
- Vérifier formats de dates.
- Vérifier distances > 0.
- Vérifier coûts > 0.
- Ajouter une colonne : Delay_Days = ArrivalDate - DepartureDate


## Étape 2 — Déterminer si la livraison est en retard
Nous considérons qu’une livraison est **en retard si Delay_Days > 5** (exemple).

Créer une colonne : Late = IF(Delay_Days > 5; 1; 0)


## Étape 3 — Calculer les KPI par transporteur
Créer un tableau ou TCD contenant :

- Nombre total de livraisons
- % de retards
- Coût moyen par km
- Distance moyenne
- Délai moyen
- Variabilité (écart-type des délais)

## Étape 4 — Identifier les transporteurs à risque
- Transporteurs avec > 25 % de retards
- Transporteurs avec coût / km trop élevé
- Transporteurs avec variabilité forte

## Étape 5 — Créer un mini-dashboard
Onglet `Dashboard` :

- Graphique % retards par transporteur
- Carte KPI : délai moyen global
- Liste : Top 3 transporteurs les plus fiables
- Liste : Top 3 transporteurs les moins fiables

---

# 3. Livrables attendus

Créer le fichier :

📁 `Transporteurs_Performance.xlsx`

Feuilles obligatoires :
- `Raw_Data`
- `KPI_Transporteurs`
- `Dashboard`

Vous pouvez utiliser le template fourni dans :
`06_Solutions/Exercice2_Transporteurs/Transporteurs_Performance_template.xlsx`

---

# 4. Difficulté estimée
⭐⭐⭐✩✩ – Intermédiaire

---

# 5. Objectifs pédagogiques
- Nettoyer une base logistique
- Calculer des KPI sur les transporteurs
- Travailler sur délais, retards, coûts logistiques
- Réaliser un dashboard simple mais professionnel


