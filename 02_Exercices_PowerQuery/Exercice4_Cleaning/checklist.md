# ✔️ Checklist Professionnelle — Data Cleaning (Power Query)

Cette checklist vous permet de valider que votre nettoyage de données Supply Chain est complet, fiable, robuste.

---

# 🟦 1. Vérifications Préliminaires

### ☐ 1.1 Les colonnes ont été identifiées correctement  
- colonnes date  
- SKU  
- quantités  
- champs texte  
- statut / WMS  
- nombres

### ☐ 1.2 Les types d’erreurs ont été listés  
- dates impossibles  
- formats multiples  
- texte dans colonnes numériques  
- SKU corrompus  
- doublons  
- quantités négatives ou en texte  
- espaces parasites  

---

# 🟩 2. Nettoyage des Dates

### ☐ 2.1 Conversion 100 % des dates valides au même format  
Format recommandé : `yyyy-MM-dd`

### ☐ 2.2 Dates invalides traitées  
- converties si possible  
- sinon transformées en `null`

### ☐ 2.3 Uniformisation des formats  
- `12/03/23`  
- `2023-03-12`  
- `03.12.2023`  
→ convertis dans un format unique  

---

# 🟨 3. Nettoyage des SKUs

### ☐ 3.1 Trim (espace début/fin) effectué  
### ☐ 3.2 Majuscules appliquées  
### ☐ 3.3 SKU invalides supprimés ou mis en null  
Exemples invalides :  
- `""`  
- `"NULL"`  
- `"SKU-???"`  
- `"sku - 344"`  
### ☐ 3.4 SKU normalisés au format `SKU-999`

---

# 🟧 4. Nettoyage des Quantités

### ☐ 4.1 Conversion texte → nombre  
Exemples traités :  
- `"12 unités"`  
- `"unit-45"`  

### ☐ 4.2 Quantités négatives mises en null  
### ☐ 4.3 Outliers supprimés  
Ex : > 50 000  

### ☐ 4.4 Conversion finale en `Int64`

---

# 🟥 5. Doublons

### ☐ 5.1 Détection de tous les doublons  
Critères possibles :  
- OrderID unique  
- OrderID + SKU  
- OrderID + SKU + Date  

### ☐ 5.2 Règle métier appliquée  
Garder par exemple :  
- la ligne la plus récente  
- ou la quantité la plus élevée  
- ou la ligne la plus complète  

### ☐ 5.3 Doublons supprimés

---

# 🟫 6. Champs divers (SupplierID, Status, Warehouse)

### ☐ 6.1 SupplierID uniformisé en texte  
### ☐ 6.2 Status normalisé (`Pending`, `Confirmed`, `Cancelled`)  
### ☐ 6.3 Warehouse nettoyé (`WH-A`, `WH-B`, etc.)  

---

# 🟪 7. Structuration finale

### ☐ 7.1 Tous les types ont été forcés correctement  
- dates  
- textes  
- nombres  

### ☐ 7.2 Lignes invalides filtrées  
### ☐ 7.3 Données triées par `OrderDate`

---

# 🟦 8. Export Final

### ☐ 8.1 Tableau final nommé `Raw_Cleaned`  
### ☐ 8.2 Optionnel : `Report_Errors_Detected`  
### ☐ 8.3 Fichier final créé :   PQ_Cleaning_Output.xlsx


---

# ✔️ Cette checklist doit être 100 % validée avant de passer à l’exercice suivant.

