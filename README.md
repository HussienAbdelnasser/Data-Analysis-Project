# 🏠 Nashville Housing Data Cleaning Project

## 📌 Project Description

In this project, I performed **data cleaning using SQL** on a raw real estate dataset from Nashville, Tennessee. The goal was to prepare the data for analysis by:
- Standardizing date formats
- Filling missing property address data
- Splitting addresses into individual columns
- Normalizing text values (e.g. "Y"/"N" → "Yes"/"No")
- Removing duplicate records
- Dropping unnecessary columns

This project demonstrates my ability to work with real-world messy data using SQL techniques like `UPDATE`, `ALTER TABLE`, `SUBSTRING`, `CASE`, `ROW_NUMBER()`, and `JOIN`.

---

## 🛠️ Tools & Skills Used
- **SQL Server**
- **Data Cleaning & Preprocessing**
- **Window Functions (`ROW_NUMBER`)**
- **Text Parsing & Transformation**
- **Data Deduplication**
- **Column Management (`ALTER TABLE`)**

---

## 🔍 Key Cleaning Steps

### 1. Standardize Date Format
Converted `SaleDate` from string to proper `Date` type using `CONVERT()`.

```sql
ALTER TABLE NashvilleHousing
ADD SalesDateConverted DATE;

UPDATE NashvilleHousing
SET SalesDateConverted = CONVERT(Date, SaleDate);
````

---

### 2. Fill Missing Property Addresses

Used `JOIN` on `ParcelID` to fill missing `PropertyAddress` values by matching with entries that had the same parcel ID but a known address.

---

### 3. Split Address Into Columns

Extracted street address and city from `PropertyAddress` using `SUBSTRING()` and `CHARINDEX()`.

```sql
ALTER TABLE NashvilleHousing
ADD PropertySplitAddress NVARCHAR(255),
    PropertySplitCity NVARCHAR(255);
```

Also parsed `OwnerAddress` into:

* `OwnerSplitAddress`
* `OwnerSplitCity`
* `OwnerSplitState`

Using `PARSENAME()` with a delimiter replacement.

---

### 4. Standardize “SoldAsVacant” Values

Replaced `Y/N` with `Yes/No` using a `CASE` statement.

---

### 5. Remove Duplicate Records

Used a CTE with `ROW_NUMBER()` to find and later remove duplicate rows based on key columns.

```sql
WITH RowNumCTE AS (
  SELECT *,
    ROW_NUMBER() OVER (
      PARTITION BY ParcelID, PropertyAddress, SalePrice, SaleDate, LegalReference
      ORDER BY UniqueID
    ) row_num
  FROM NashvilleHousing
)
```

---

### 6. Drop Unused Columns

Dropped irrelevant columns like `TaxDistrict`, `OwnerAddress`, and `PropertyAddress` once split and cleaned versions were created.

---

## 📁 Dataset Source

The dataset was provided as part of a portfolio SQL challenge (source can be modified or mentioned here if applicable).

---

## 🚀 Outcome

This project prepared a messy real estate dataset into a clean and structured format, ready for analysis and visualization. It reflects best practices in SQL data cleaning and real-world data wrangling.

---

## 📎 File Name

`nashville-housing-data-cleaning.sql`
---

## 🙋‍♂️ Author

**Hussein Ahmed**
📫 [itshussein.ahmed@gmail.com](mailto:itshussein.ahmed@gmail.com)
