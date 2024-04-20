# Data-Analysis-Project
Project Title: Nashville Housing Data Cleaning

Description:

This project cleans and standardizes the "NashvilleHousing" dataset within the PortfolioProject schema, improving data quality for further analysis.

Data Source:
Kaggle

Cleaning Steps:

1. Standardize Sale Date Format:
   - The code converts the "SaleDate" column to a standard date format using `CONVERT(Date, SaleDate)`.
   - A new column, "SalesDateConverted," is added to store the standardized date.

2. Populate Missing Property Addresses:
   - Identifies rows with missing "PropertyAddress" values.
   - Uses a self-join to leverage matching "ParcelID" values and populate missing addresses from rows with the same "ParcelID" but a valid address.

3. Break Down Property Address:
   - Extracts address components (street address, city, state) from the "PropertyAddress" column using `SUBSTRING` and `CHARINDEX`.
   - New columns, "PropertySplitAddress" and "PropertySplitCity," are added to store the extracted components.

4. Split Owner Address:
   - Similar to step 3, extracts address, city, and state components from the "OwnerAddress" column using `PARSENAME` and string manipulation.
   - New columns, "OwnerSplitAddress," "OwnerSplitCity," and "OwnerSplitState," are added.

5. Standardize "SoldAsVacant" Values:
   - Replaces "Y" and "N" values in the "SoldAsVacant" column with "Yes" and "No" for better readability, using a `CASE` statement.

6. Remove Duplicates:
   - Employs a Common Table Expression (CTE) named `RowNumCTE` to assign a row number to each record based on specific columns ("ParcelID," "PropertyAddress," etc.).
   - Deletes duplicate rows (identified by `row_num > 1`) while preserving the most recent record based on the "UniqueID" order.

7. Drop Unused Columns:
   - Removes columns deemed unnecessary for further analysis, including "OwnerAddress," "TaxDistrict," and the original "PropertyAddress" with potentially inconsistent formatting.
