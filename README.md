# Model Paper: all 15 sets solved

Every set from `MLT Lab-Model Questions.pdf` is solved here as one notebook (Question 1 + Question 2). All 15 notebooks run with no errors and already show their output. Open one and read it top to bottom.

Every notebook reads its file from the `data/` folder, e.g. `pd.read_csv("data/Diabetes.csv")`.

## Which set is which

| Set | Q1 dataset | Q2 asks for | Q2 is from | Notebook | Learn Q2? |
|---|---|---|---|---|---|
| 1 | Diabetes | Simple linear regression | Exp 3 | `Set01_EDA_and_Simple_Linear_Regression` | ✅ |
| 2 | Toyota | Multiple linear regression | Exp 3 | `Set02_EDA_and_Multiple_Linear_Regression` | ✅ |
| 3 | Stores* | Logistic regression | Exp 3 | `Set03_EDA_and_Logistic_Regression` | ✅ |
| 4 | Stores | Single layer perceptron | Exp 4 | `Set04_EDA_and_Single_Layer_Perceptron` | ✅ |
| 5 | Kasol* | MLP regression + Randomized/Grid search | Exp 5 | `Set05_EDA_and_MLP_Regression` | ✅ |
| 6 | Toyota* | Random Forest + optimise | Exp 10 | `Set06_EDA_and_Random_Forest` | ✅ |
| 7 | IRIS | MLP classifier + optimise | Exp 5 | `Set07_EDA_and_MLP_Classifier` | ✅ |
| 8 | Diabetes | Agglomerative clustering + dendrogram | Exp 6 | `Set08_EDA_and_Agglomerative_Clustering` | if time |
| 9 | IRIS | Fuzzy C-Means | Exp 7 | `Set09_EDA_and_Fuzzy_C_Means` | ❌ skip |
| 10 | Solar | SOM clustering | Exp 8 | `Set10_EDA_and_SOM` | ❌ skip |
| 11 | Diabetes | Apriori | Exp 9 | `Set11_EDA_and_Apriori` | ❌ skip |
| 12 | Diabetes | Random Forest, 4 models | Exp 10 | `Set12_EDA_and_Random_Forest_4_models` | ✅ |
| 13 | Diabetes | AdaBoost, 4 models | Exp 10 | `Set13_EDA_and_AdaBoost` | ✅ |
| 14 | Diabetes | Perceptron vs logistic regression | Exp 4 + 3 | `Set14_EDA_and_Perceptron_vs_Logistic` | ✅ |
| 15 | Solar | MLP regression + Randomized search | Exp 5 | `Set15_EDA_and_MLP_Regression` | ✅ |

**Even for the ❌ sets, learn Q1.** It's always the easy EDA question and carries its own marks.

\* **Stand-in datasets.** We don't have every file the paper names, so these use a similar file. The code stays the same, and in the exam you change only the file name and the column names:

| Paper asks for | Used here | Why it works |
|---|---|---|
| `Rampur.csv` (set 1) | `Kasol.csv` | Same river data, with the same `Wind` and `Discharge` columns |
| `Customers.csv` (set 3) | `Stores.csv` | Text columns + missing values above and below 10% |
| `Solar-Regression.csv` (sets 5, 15) | `Kasol.csv` → `Solar` column | A continuous solar radiation value to predict |
| `Precipitation.csv` (set 6) | `Toyota.csv` | Numbers + text columns, for the count plot and label encoding |
| `Supermarket.csv` (set 11) | `Shop1.csv` | Shopping baskets, the same format Apriori needs |
| `house_price.csv` (set 12) | `Diabetes.csv` | 2 classes. For a price column, first do `(df["Price"] > df["Price"].median()).astype(int)` |

`Diabetes.csv` is the standard Pima Indians diabetes data (with the `Outcome` column), and `IRIS.csv` is the standard Iris data.

---

## How to learn this (the easy way)

### Step 1: Q1 is only about 25 small tasks, repeated across the 15 sets
Learn each task **once** and you can answer every Q1. Here they are, with the sets that ask each one:

**EDA (Exp 1A)**

| Task | Key code | Sets |
|---|---|---|
| Columns with nulls | `df.isnull().sum()` | 1, 11 |
| 5-number summary + box plot | `df[col].describe()[["min","25%","50%","75%","max"]]` · `sns.boxplot(x=df[col])` | 1, 9, 10, 11, 14 |
| min / max / mean / mode | `pd.DataFrame({"min": num.min(), "max": num.max(), "mean": num.mean()})` (mode: `num.mode().iloc[0]`) | 2, 4, 15 |
| Count categorical / numerical columns | `df.select_dtypes("object")` · `df.select_dtypes("number")` | 2 |
| High +ve / high −ve / no correlation | `df.corr()`, then loop: > 0.5, < −0.5, close to 0 | 3, 5, 7 |
| Pearson / Spearman heatmap | `sns.heatmap(num.corr(method="spearman"), annot=True)` | 7 |
| Skewness: right / left / none | `num.skew()`: > 0.5 right, < −0.5 left · `num.plot(kind="kde", subplots=True)` | 4, 10 |
| Z-score outliers | `z = ((num - num.mean()) / num.std()).abs()` → `z > 3` | 1, 6, 8, 12 |
| Plots | `sns.catplot` (continuous vs category) · `sns.swarmplot` · bar: `value_counts().plot(kind="bar")` · `plt.scatter` · `sns.countplot(x=, hue=)` · `sns.pairplot(df, hue=)` · `sns.violinplot` | all |

**Pre-processing (Exp 1B)**

| Task | Key code | Sets |
|---|---|---|
| Fill missing (mean / median / mode) when % is above or below 10 | `pct = df.isnull().mean() * 100` → `if pct[col] > 10: df[col] = df[col].fillna(df[col].mean())` | 2, 3, 4, 5, 12 |
| Nearest interpolation | `df[col].interpolate(method="nearest")` | 8 |
| Drop columns with too many missing | `df.drop(columns=pct[pct > 10].index)` · `df.dropna(axis=1)` | 3, 5, 13 |
| Label encoding | `LabelEncoder().fit_transform(df[col])` | 6, 13, 15 |
| One-hot encoding | `pd.get_dummies(df, columns=[col], dtype=int)` | 7 |
| StandardScaler | `StandardScaler().fit_transform(num)` | 4, 10 |
| Min-max (by hand) | `(x - x.min()) / (x.max() - x.min())` | 9, 15 |
| Z-score (by hand) | `(x - x.mean()) / x.std()` | 10, 12 |
| Binarize | `Binarizer(threshold=140).fit_transform(df[[col]])` | 14 |

**Feature selection (Exp 2)**

| Task | Key code | Sets |
|---|---|---|
| Drop low variance | `num.var()` → drop `< 10` · or `VarianceThreshold(0.3)` | 6, 15 |
| Drop highly correlated | upper triangle of `corr().abs()`, drop columns `> 0.5` (or 0.8) | 14 |
| SelectKBest / SelectPercentile | `SelectKBest(f_classif, k=4).fit(X, y).get_feature_names_out()` | 11 |
| PCA | `PCA(n_components=2).fit_transform(X)` | 8 |
| LDA | `LinearDiscriminantAnalysis(n_components=2).fit_transform(X, y)` (needs y) | 9, 13 |

### Step 2: Q2 is one pattern
```
X, y  →  train_test_split(X, y, test_size=0.25, random_state=42)  →  model.fit  →  model.predict  →  score
```
Regression is scored with MSE and RMSE. Classification is scored with the confusion matrix plus accuracy, precision, recall and F1. "Optimise" always means `GridSearchCV` (or `RandomizedSearchCV`) with a small dictionary of parameters.

### Step 3: Order for tonight
1. **Set 1 → Set 4**: they cover most Q1 tasks plus linear, logistic and perceptron.
2. **Set 6, 12, 13**: Random Forest and AdaBoost.
3. **Set 5, 7, 15**: MLP.
4. **Set 14**: perceptron vs logistic (you'll already know both).
5. **Q1 only** of sets 8, 9, 10, 11.
6. Set 8 Q2, if you have time left.

### Exam tips
- In the exam, look at your file first: `df.head()`, `df.info()`. Then change the column names in the code.
- If a question's condition doesn't match the data (for example "fill if missing > 10%" and nothing is above 10%), the code prints why it left the column alone. Say that out loud in the viva. It shows you checked.
- Precision / recall with 3 classes (IRIS) need `average="macro"`.
