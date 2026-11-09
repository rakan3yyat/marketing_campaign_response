# Marketing Campaign Response Prediction

**Type:** Classification

## Goal
A food & retail company ran a marketing campaign. Predict whether a customer will **accept the offer (1)** or **not (0)**.

**Target:** `response` (1 = accepted the offer, 0 = did not accept)

## Data
The data is **synthetic** (fake, created with Python) so it can be big and have all the problems we want to practice on.

- **Generator:** `generate_data.py` (random seed = 123, so you always get the same data)
- **File:** `data/marketing_campaign_customers.csv` - about 110,000 rows (+0.5% duplicate rows) and 54 columns
- **Data types in the file:** int, float, text (object) and dates saved as text

Re-create (or change) the data:
```bash
python generate_data.py
```
Change `N_ROWS` (100,000 - 300,000) or `SEED` at the top of `generate_data.py` to get a bigger / different dataset.

### Problems added on purpose
- **ID / personal columns** (IDs, names, e-mail, phone, date of birth)
- **Messy column names** (`Annual Income ($)`, `CreditScore`, `No. of Dependents`, ...)
- **Short codes** that should be full values (`M`/`F`, `Y`/`N`, `T`/`F`, `FT`/`PT`, ...)
- **Numbers saved as text** (`$1,250.00`, `12,500`, `34 yrs`, `12.5 km`, `15%`)
- **Missing values**, also hidden as text (`N/A`, `unknown`, `?`, `-`, a space) and one **mostly empty** column
- **Outliers** (extreme values) and **impossible values** (age = 150, negative mileage, 9999 codes, ...)
- **Dates saved as text** in different formats
- **Messy text** (`New York`, `NEW YORK`, `  new york`)
- **Categories saved as numbers** (`region_code`)
- **Useless columns**: a constant column, an almost-constant column, a duplicate column
- **Duplicate rows** (~0.5%)

### Columns
| Column in the file | Clean name | Problems |
|---|---|---|
| `ID` | `customer_id` | ID / personal detail -> drop |
| `Full Name` | `full_name` | ID / personal detail -> drop |
| `E-mail` | `email` | ID / personal detail -> drop |
| `Phone #` | `phone_number` | ID / personal detail -> drop |
| `DOB` | `date_of_birth` | ID / personal detail -> drop |
| `Age (Years)` | `age` | number saved as text (`12 yrs`); 2% missing; wrong values: `0`, `120`, `130` |
| `Gender` | `gender` | short codes: `M`, `F` |
| `Marital Status` | `marital_status` | short codes: `S`, `M`, `D`, `W` |
| `Education` | `education` | short codes: `HS`, `BSC`, `MSC`, `PHD` |
| `City` | `city` | 1% missing; mixed upper / lower case, extra spaces |
| `Region Code` | `region_code` | category saved as a number |
| `Preferred Language` | `preferred_language` | - |
| `Marketing Opt-In` | `marketing_opt_in` | short codes: `Y`, `N` |
| `Customer Since Date` | `customer_since_date` | date saved as text (`%d/%m/%Y`) |
| `Annual Income` | `annual_income` | number saved as text (`$1,250.00`); 2% missing; extreme outliers |
| `Kidhome` | `kids_home` | - |
| `Teenhome` | `teens_home` | - |
| `Recency` | `recency_days` | - |
| `MntWines` | `spent_wines` | number saved as text (`$1,250.00`) |
| `MntFruits` | `spent_fruits` | - |
| `MntMeatProducts` | `spent_meat` | number saved as text (`$1,250.00`); extreme outliers |
| `MntFishProducts` | `spent_fish` | - |
| `MntSweetProducts` | `spent_sweets` | - |
| `MntGoldProds` | `spent_gold` | - |
| `NumDealsPurchases` | `num_deals_purchases` | - |
| `NumWebPurchases` | `num_web_purchases` | - |
| `NumCatalogPurchases` | `num_catalog_purchases` | - |
| `NumStorePurchases` | `num_store_purchases` | - |
| `NumWebVisitsMonth` | `num_web_visits_month` | wrong values: `99` |
| `AcceptedCmp1` | `accepted_cmp1` | - |
| `AcceptedCmp2` | `accepted_cmp2` | - |
| `AcceptedCmp3` | `accepted_cmp3` | - |
| `AcceptedCmp4` | `accepted_cmp4` | - |
| `AcceptedCmp5` | `accepted_cmp5` | - |
| `Complain` | `complain` | - |
| `Loyalty Tier` | `loyalty_tier` | short codes: `B`, `S`, `G`, `P` |
| `Preferred Channel` | `preferred_channel` | 2% missing; mixed upper / lower case, extra spaces |
| `Avg Basket Size` | `avg_basket_size` | number saved as text (`$1,250.00`); 3% missing; extreme outliers |
| `App Installed` | `app_installed` | short codes: `Y`, `N` |
| `Push Notifications On` | `push_notifications_on` | short codes: `T`, `F` |
| `Email Open Rate` | `email_open_rate` | number saved as text (`12.5%`); 5% missing |
| `Coupon Redemptions 12m` | `coupon_redemptions_12m` | - |
| `Returns 12m` | `returns_12m` | - |
| `Customer Rating Avg` | `customer_rating_avg` | 4% missing; wrong values: `0`, `9` |
| `Last Store Visit Date` | `last_store_visit_date` | 2% missing; date saved as text (`%Y-%m-%d`) |
| `Household Size` | `household_size` | - |
| `Pet Owner` | `pet_owner` | short codes: `Y`, `N` |
| `Has Vehicle` | `has_vehicle` | - |
| `Referral Code` | `referral_code` | 85% missing (mostly empty) |
| `Z_CostContact` | `z_cost_contact` | only one value |
| `Z_Revenue` | `z_revenue` | only one value |
| `Is Test Customer` | `is_test_customer` | almost only one value |
| `Store Purchases Count` | `store_purchases_count` | exact copy of `num_store_purchases` |
| `Response` | `response` | **TARGET** (text: 'Yes', 'No') |

## Files
| File | Description |
|---|---|
| `generate_data.py` | Creates the dataset |
| `marketing_campaign_response.ipynb` | The full project notebook |
| `data/marketing_campaign_customers.csv` | The dataset |
| `data/marketing_campaign_customers_metadata.xlsx` | Metadata (data dictionary): description, role, data types, missing values, statistics, codes and problems of every column |
| `models/` | Created when you run the notebook (best model, scaler, selected features) |

## Steps in the notebook
**A. Data cleaning**
- A0 Drop columns that are not useful (IDs, names, ...)
- A1 Rename columns
- A2 Data types: numbers saved as text, messy text, short codes, dates, categories saved as numbers, target to 0/1
- A3 Missing values - % per column, drop columns > 50% missing, fill the rest (mean / mode)
- A4 Outliers - box plots, IQR table, wrong values -> median, extreme values -> capping
- A5 Duplicate rows and duplicate columns
- A6 Columns with only one value

**B. EDA**
- B1 Target count and percentage
- B2 Correlation heatmap and correlation with the target
- B3 Charts for the independent features (and features vs target)

**C. Feature engineering**
- C1 Binning / new features
- C2 Label encoding and one-hot encoding
- C3 Data splitting: 80% train / 15% test / 5% unseen (random_state = 123)
- C4 Feature scaling (StandardScaler)
- C5 Feature selection (SelectKBest + Random Forest importance)
- C6 20,000-row training sample for comparing / tuning the models

**D. Feature extraction**
- D1 PCA
- D2 Factor Analysis
- D3 Cluster analysis (K-Means with elbow and silhouette)

**E. Machine learning (20 models)**
- E1 Define the models
- E2 Evaluation function (all metrics)
- E3 Cross validation (5 folds)
- E4 Training and testing evaluation
- E5 Hyperparameter dictionary
- E6 Grid search tuning
- E7 Compare the models, select the best, re-train it on the full training data
- E8 Save the best model (`joblib`)
- E9 Load the model
- E10 Predict and evaluate on the 5% unseen data

## How to run
```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost joblib jupyter
python generate_data.py
```
Then open `marketing_campaign_response.ipynb` **from this folder** and run all cells (the full notebook takes about 10-20 minutes).
