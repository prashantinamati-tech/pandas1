import pandas as pd

# 1. Load the dataset
df = pd.read_csv("matches.csv")

# 2. Basic Inspection
print("=== Shape ===")
print(df.shape)  # Returns (1095, 20)

print("\n=== Data Info & Types ===")
print(df.info())

print("\n=== Missing Values Summary ===")
print(df.isnull().sum())

print("\n=== Duplicate Check ===")
print(f"Duplicate rows: {df.duplicated().sum()}")

# 3. Data Cleaning & Missing Value Handling

# A. Drop columns with too many missing values or unused columns
# Note: 'method' has 1,074 missing values
df_cleaned = df.drop(columns=["method"])

# B. Fill missing categorical values with a default placeholder
categorical_cols = ["city", "winner", "player_of_match"]
for col in categorical_cols:
    df_cleaned[col] = df_cleaned[col].fillna("Unknown")

# C. Fill missing numerical values with 0 or the median
df_cleaned["result_margin"] = df_cleaned["result_margin"].fillna(0)
df_cleaned["target_runs"] = df_cleaned["target_runs"].fillna(0)
df_cleaned["target_overs"] = df_cleaned["target_overs"].fillna(0)

# D. Drop remaining rows with missing values (if any)
# Alternatively, inplace operations or reassignment can be used carefully:
# df_cleaned.drop_duplicates(inplace=True)  # Proper inplace usage without assignment

print("\n=== Cleaned Dataset Missing Values ===")
print(df_cleaned.isnull().sum())

print("\n=== First 5 Rows of Cleaned Data ===")
print(df_cleaned.head())
