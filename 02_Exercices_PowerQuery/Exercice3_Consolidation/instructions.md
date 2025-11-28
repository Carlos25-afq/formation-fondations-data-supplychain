# Exercice 3 — Consolidation Mensuelle Automatique (Power Query / ETL Supply Chain)

## 🎯 Objectif
Mettre en place un pipeline ETL robuste dans Power Query permettant de consolider automatiquement **12 fichiers mensuels hétérogènes**, issus d’extractions ERP/WMS différentes, et d’obtenir un tableau final propre, cohérent et directement exploitable pour les analyses Supply Chain (prévisions, contrôle logistique, analyses SKU…).

Cet exercice simule un cas réel vécu dans la plupart des entreprises industrielles, retail et e-commerce.

---

# 1. Contexte Métier (Réel & Professionnel)

Dans une organisation Supply Chain moderne, les fichiers issus de l’ERP évoluent fréquemment :

- changement du modèle de données d’un mois à l’autre  
- colonnes renommées par les équipes IT ou métiers  
- ajout / suppression temporaire de colonnes  
- variations d’ordre et de typage  
- champs mélangés (SKU / Product Code, Quantity / Qty, OrderDate / Date_Commande, etc.)

Conséquences métiers :
- Analyse impossible si chaque mois a un schéma différent  
- Consolidation manuelle extrêmement chronophage  
- Risque d’erreurs dans le S&OP ou le Demand Planning  
- KPIs incohérents (OTD, service client, prévisions…)  
- Perte de confiance dans les données

Votre mission :
👉 Construire un processus de **consolidation entièrement automatisé**, robuste à toutes les variations de structure.

---

# 2. Données fournies

Vous trouverez les fichiers sources dans :

05_Datasets/Exercice3_Consolidation/


Ces fichiers représentent **12 exports mensuels bruts**, volontairement non harmonisés.

Chaque fichier porte le nom :
commandes_2023_M01_brut.xlsx
commandes_2023_M02_brut.xlsx
...
commandes_2023_M12_brut.xlsx


## Les fichiers présentent plusieurs types de variations réelles :

### 🔹 Variations de noms de colonnes
Par exemple :
- `OrderDate`  
- `Date_Commande`  
- `Order_Date`

- `SKU`  
- `Product_Code`  
- `ItemCode`

- `Quantity`  
- `Qty`  

### 🔹 Variations de structure
Selon les mois :
- colonnes manquantes  
- colonnes supplémentaires (ex. `Status`, `Commentaire`, `Warehouse`)  
- colonnes en trop supprimées dans d’autres mois  
- ordre complètement différent  

### 🔹 Variations de contenu
- formats de dates incohérents  
- textes mal typés  
- champs numériques précisés en texte  
- espaces et erreurs typiques d’extraction

Tous les fichiers contiendront **entre 4 000 et 8 000 lignes chacun**, pour rester réalistes, mais < 15 Mo.

---

# 3. Travail attendu (ETL Complet)

## Étape 1 — Connexion au dossier (Power Query)

1. Dans Excel → Données → Obtenir des données → À partir d’un dossier  
2. Sélectionner le dossier contenant les 12 fichiers bruts  
3. Visualiser la liste des fichiers  
4. Cliquer sur **Combiner + Charger**  
5. Ouvrir l’éditeur avancé pour inspecter la fonction générée automatiquement

---

## Étape 2 — Création d’une **fonction personnalisée robuste (M Code)**

L’objectif est d’obtenir une fonction capable de :

### 🔧 Normaliser les noms de colonnes
Exemples de mapping à gérer :

| Nom brut              | Nom final |
|-----------------------|-----------|
| `Date_Commande`       | OrderDate |
| `Order_Date`          | OrderDate |
| `Product_Code`        | SKU       |
| `ItemCode`            | SKU       |
| `Qty`                 | Quantity  |
| `Order_Qty`           | Quantity  |

### 🔧 Ajouter les colonnes manquantes selon le schéma cible
Schéma final attendu :


OrderID
OrderDate
DeliveryDate
SKU
SupplierID
Quantity
Warehouse
Month
SourceFile


Tout fichier ne respectant pas ce schéma devra être corrigé automatiquement.

### 🔧 Forcer les types
- dates → date  
- SKU → texte  
- Quantity → entier  
- SupplierID → texte  

### 🔧 Nettoyer les valeurs incohérentes
- valeurs null → remplacer par ""  
- dates invalides → remplacer par null  

---

## Étape 3 — Application de la fonction à tous les fichiers

- Appliquer la fonction aux 12 fichiers source  
- Vérifier que toutes les colonnes sont normalisées  
- Empiler les fichiers (“Append Queries”)  
- Ajouter une colonne `Month` extraite du nom du fichier  

---

## Étape 4 — Nettoyage final

Actions obligatoires :
- supprimer les lignes vides  
- retirer les doublons éventuels  
- trier par `OrderDate`  
- filtrer les lignes avec `SKU` manquant  
- vérifier que la table finale respecte 100 % du schéma attendu  

---

## Étape 5 — Chargement final dans Excel

Créer un fichier :

PQ_Consolidation_Output.xlsx


Feuille :
- `Final_Consolidated`

La table doit contenir environ **50 000 à 90 000 lignes**, selon les variations.

---

# 4. Livrables attendus

- Table consolidée et nettoyée
- Fonction M personnalisée capable de resynchroniser n’importe quel fichier brut
- Fichier final prêt à être utilisé dans Power Pivot ou pour la prévision

Template possible dans :
`06_Solutions/Exercice3_Consolidation/PQ_Consolidation_template.xlsx`

---

# 5. Objectifs pédagogiques (Niveau Expert)

Vous apprendrez à :

- maîtriser un flux complet ETL Power Query, comme en entreprise  
- manipuler des schémas instables avec une fonction personnalisée  
- construire une architecture de données fiable  
- préparer des données pour le Demand Planning  
- automatiser une consolidation mensuelle à 100 %  
- rendre votre pipeline résistant aux futures variations  

---

# 6. Difficulté

⭐⭐⭐⭐⭐ — **Avancé (Data Engineering Supply Chain)**  


