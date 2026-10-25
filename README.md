# Python DA Assignment 2 – Data Visualization

## Project Overview

This project uses the Seaborn `taxis` dataset to practice missing-value handling and data visualization with Pandas, Matplotlib, and Seaborn. It explores fare trends, borough totals, payment methods, trip distances, tips, and relationships among numerical variables.

## Objectives

- Load the `taxis` dataset and handle missing values.
- Create five Matplotlib/Pandas visualizations and five Seaborn visualizations.
- Interpret each chart using findings and insights.

## Tools and Technologies

- Python
- Jupyter Notebook
- Pandas
- Matplotlib
- Seaborn

## Dataset

**Dataset:** Seaborn built-in `taxis` dataset, loaded with `sns.load_dataset("taxis")`.

It includes pickup timestamps, fare, distance, tip, tolls, total, payment method, and pickup/drop-off locations. No separate CSV is required.

---

## 1) Loading the Taxis Dataset

**Original Assignment Instruction:**

```python
import seaborn as sns

# Load the 'taxis' dataset
df = sns.load_dataset("taxis")
```

**Python Code:**

```python
import seaborn as sns

# Load the 'taxis' dataset
df = sns.load_dataset("taxis")
```

**Output:** The dataset is loaded into the DataFrame `df`.

---

## 2) Handling Missing Values

**Original Assignment Questions:**

- Check for missing values in the dataset and identify columns with missing data.
- Impute missing values using appropriate strategies based on the column type (e.g., using mean, median, or mode for numerical columns, and mode for categorical columns).
- For columns that are critical and cannot be reasonably imputed, remove rows with missing values to maintain data integrity.

### Check Missing Values

```python
# Checking missing values in each column
df.isnull().sum()
```

**Output:**

| Column | Missing Values |
|---|---:|
| payment | 44 |
| pickup_zone | 26 |
| dropoff_zone | 45 |
| pickup_borough | 26 |
| dropoff_borough | 45 |

All other columns had zero missing values.

### Handle Missing Values

```python
# Fill missing payment values using mode
df['payment'] = df['payment'].fillna(df['payment'].mode()[0])

# Remove rows with missing critical location information
df = df.dropna(subset=[
    'pickup_zone',
    'dropoff_zone',
    'pickup_borough',
    'dropoff_borough'
])
```

**Cleaning Decisions:**

- **Payment:** Filled missing values using the mode because it is a categorical column.
- **Location columns:** Removed rows with missing values because actual pickup and drop-off locations cannot be reliably determined.
- **Numerical columns:** No imputation was required because they contained no missing values.

### Verify Missing Values After Cleaning

```python
# Check missing values after handling
df.isnull().sum()
```

**Output:** All columns returned zero missing values after cleaning.

---

## 3) Visualizations using Matplotlib/Pandas Plot

### ★ Line Chart

**Original Assignment Question:**

Plot a line chart to visualize the fare over time, using the pickup timestamp as the x-axis and fare as the y-axis. Ensure the pickup column is converted to a datetime format before plotting.

**Python Code:**

```python
import matplotlib.pyplot as plt
import pandas as pd

# Convert pickup column to datetime format
df['pickup'] = pd.to_datetime(df['pickup'])

# Plot line chart
df.sort_values('pickup').plot(
    x='pickup',
    y='fare',
    kind='line',
    figsize=(10, 5),
    color='#A3195B'
)

plt.title('Fare Over Time')
plt.xlabel('Pickup Date and Time')
plt.ylabel('Fare')
plt.show()
```

**Output:** A line chart showing fare amounts over pickup timestamps.

**Findings:**

- Taxi fares fluctuate frequently throughout March 2019.
- Most fares are approximately between USD 5 and USD 50.
- A few high-fare trips reach approximately USD 150.

**Insights:**

- Most trips have relatively low to moderate fares.
- Occasional high-fare trips create noticeable spikes.
- No clear increasing or decreasing trend is observed during the month.

### ★ Bar Chart

**Original Assignment Question:**

Create a bar chart to show the total fare for each pickup_borough. Group the data by pickup_borough and sum the fare for each group.

**Python Code:**

```python
# Group by pickup_borough and calculate total fare
total_fare = df.groupby('pickup_borough')['fare'].sum()

# Plot bar chart
total_fare.plot(
    kind='bar',
    figsize=(10, 5),
    color=['#6B1E59', '#A3195B', '#D94B69', '#F19A7E']
)

plt.title('Total Fare by Pickup Borough')
plt.xlabel('Pickup Borough')
plt.ylabel('Total Fare')
plt.show()
```

**Output:** A bar chart showing total fares by pickup borough.

**Findings:**

- Manhattan records the highest total fare, approximately USD 58,000, followed by Queens at USD 15,500.
- Brooklyn generates approximately USD 6,000, while the Bronx has the lowest total fare at around USD 2,000.
- Total fare amounts vary significantly across the four pickup boroughs.

**Insights:**

- Manhattan is the largest contributor to total taxi fare revenue.
- Queens is the second-largest contributor, while Brooklyn and the Bronx contribute comparatively less.
- Differences in total fares may be influenced by trip frequency, distance, and fare amounts across boroughs.

### ★ Pie Chart

**Original Assignment Question:**

Plot a pie chart showing the distribution of trips based on the payment method (credit card, cash, etc.). Each slice should represent the count of trips for a specific payment method.

**Python Code:**

```python
# Count trips for each payment method
payment_counts = df['payment'].value_counts()

# Plot pie chart
payment_counts.plot(
    kind='pie',
    figsize=(10, 5),
    autopct='%1.1f%%',
    colors=['#A3195B', '#F19A7E']
)

plt.title('Distribution of Trips by Payment Method')
plt.ylabel('')
plt.show()
```

**Output:**

| Payment Method | Percentage |
|---|---:|
| Credit Card | 71.9% |
| Cash | 28.1% |

**Findings:**

- Credit card payments account for 71.9% of total taxi trips.
- Cash payments represent the remaining 28.1% of trips.
- Credit card payments are more than twice as common as cash payments.

**Insights:**

- Credit cards are the dominant payment method among taxi passengers in the dataset.
- The higher proportion of credit card transactions indicates a greater use of cashless payments.
- Cash remains a significant payment option, accounting for more than one-quarter of trips.

### ★ Histogram

**Original Assignment Question:**

Create a histogram to visualize the distribution of distance. Customize the number of bins for better granularity and ensure the plot is easy to interpret.

**Python Code:**

```python
# Plot histogram for distance
df['distance'].plot(
    kind='hist',
    bins=30,
    figsize=(10, 5),
    color='#D94B69',
    edgecolor='black'
)

plt.title('Distribution of Trip Distance')
plt.xlabel('Distance')
plt.ylabel('Frequency')
plt.show()
```

**Output:** A histogram displaying the frequency distribution of trip distances using 30 bins.

**Findings:**

- Most taxi trips cover short distances, primarily between 0 and 5 miles.
- The frequency of trips decreases as distance increases.
- A few trips cover longer distances, extending beyond 20 miles.
- The distribution is right-skewed, with a long tail toward higher distances.

**Insights:**

- Short-distance trips are more common than long-distance trips.
- The right-skewed distribution indicates that most trips have relatively low distances.
- Longer-distance trips are relatively rare compared to shorter trips.

### ★ Box Plot

**Original Assignment Question:**

Plot a box plot to visualize the distribution of tip amounts for each pickup_borough. Use pickup_borough as the categorical axis and tip as the numeric axis.

**Python Code:**

```python
# Plot box plot for tip amounts by pickup borough
box_plot = df.boxplot(
    column='tip',
    by='pickup_borough',
    figsize=(10, 5),
    patch_artist=True
)

# Add different colors to each box
colors = ['#6B1E59', '#A3195B', '#D94B69', '#F19A7E']

for box, color in zip(box_plot.patches, colors):
    box.set_facecolor(color)

plt.title('Distribution of Tip Amounts by Pickup Borough')
plt.suptitle('')
plt.xlabel('Pickup Borough')
plt.ylabel('Tip Amount')
plt.show()
```

**Output:** A box plot comparing tip distributions across pickup boroughs.

**Findings:**

- The Bronx has the lowest tip amounts, with most values concentrated around zero.
- Brooklyn shows relatively low tip amounts, with several outliers reaching approximately 7.5.
- Manhattan has a median tip of approximately 1.5, with numerous high-value outliers.
- Queens shows the greatest variation in tip amounts, with outliers exceeding 20.

**Insights:**

- Tipping patterns vary across pickup boroughs.
- Manhattan has a higher typical tip amount than the Bronx and Brooklyn.
- Queens exhibits greater variability in tipping.
- Outliers suggest that some passengers provide significantly higher tips than most others.

---

## 4) Visualizations using Seaborn

### ★ Count Plot

**Original Assignment Question:**

Create a count plot to visualize the number of trips in each pickup_borough. The x-axis should represent the boroughs, and the y-axis should show the count of trips.

**Python Code:**

```python
# Plot count plot for trips by pickup borough
sns.countplot(
    data=df,
    x='pickup_borough',
    hue='pickup_borough',
    palette='rocket',
    legend=False
)

plt.title('Number of Trips by Pickup Borough')
plt.xlabel('Pickup Borough')
plt.ylabel('Number of Trips')
plt.show()
```

**Output:** A count plot showing trip counts by pickup borough.

**Findings:**

- Manhattan records the highest number of taxi trips, approximately 5,250.
- Queens ranks second with around 650 trips, followed by Brooklyn with approximately 370 trips.
- The Bronx has the lowest number of trips, with approximately 100 recorded.
- Trip counts vary significantly across the four pickup boroughs.

**Insights:**

- Manhattan dominates taxi pickup activity, accounting for the majority of recorded trips.
- Queens and Brooklyn contribute considerably fewer trips compared to Manhattan.
- The Bronx shows the lowest taxi pickup activity.
- The differences indicate an uneven distribution of taxi pickups across boroughs.

### ★ Scatter Plot

**Original Assignment Question:**

Plot a scatter plot to show the relationship between distance and fare. Use distance on the x-axis and fare on the y-axis to visualize any correlation. Color the points based on the pickup_borough to differentiate the trips by their respective boroughs.

**Python Code:**

```python
# Plot scatter plot for distance vs fare
sns.scatterplot(
    data=df,
    x='distance',
    y='fare',
    hue='pickup_borough',
    palette='rocket'
)

plt.title('Distance vs Fare by Pickup Borough')
plt.xlabel('Distance')
plt.ylabel('Fare')
plt.show()
```

**Output:** A scatter plot showing the relationship between distance and fare, colored by pickup borough.

**Findings:**

- The scatter plot shows a positive relationship between trip distance and fare.
- Most taxi trips are concentrated within distances of 0 to 15 miles, with fares generally below 50.
- A few longer trips have significantly higher fares, reaching approximately 150.
- Trips from different pickup boroughs follow a broadly similar increasing pattern.

**Insights:**

- Longer taxi trips generally result in higher fares.
- Most trips involve relatively short distances and lower fare amounts.
- A few high-fare trips stand out from the general pattern.
- The overlapping points across boroughs suggest that the distance-fare relationship is broadly similar across pickup locations.

### ★ Heatmap

**Original Assignment Question:**

Plot a heatmap to visualize the correlation between numerical variables such as distance, fare, tip, tolls, and total. Use a correlation matrix to highlight the relationships.

**Python Code:**

```python
# Calculate correlation matrix
correlation_matrix = df[
    ['distance', 'fare', 'tip', 'tolls', 'total']
].corr()

# Plot heatmap
sns.heatmap(
    correlation_matrix,
    annot=True,
    cmap='rocket'
)

plt.title('Correlation Heatmap of Numerical Variables')
plt.show()
```

**Output:** An annotated correlation heatmap showing relationships between distance, fare, tip, tolls, and total.

**Findings:**

- Fare and total have the strongest positive correlation (0.97).
- Distance shows a strong positive correlation with fare (0.95) and total (0.93).
- Tolls have a moderate positive correlation with total (0.69) and distance (0.64).
- Tip has a moderate positive correlation with total (0.65), while its correlation with distance (0.48) and fare (0.49) is comparatively weaker.
- All five numerical variables show positive correlations with one another.

**Insights:**

- Longer taxi trips are strongly associated with higher fare amounts and total charges.
- Fare is closely related to total trip cost.
- Tips and tolls contribute to total charges but show weaker relationships with other variables compared to fare and distance.
- The correlation matrix highlights distance and fare as important variables associated with total taxi trip charges.

### ★ Pair Plot

**Original Assignment Question:**

Create a pair plot to visualize the pairwise relationships between distance, fare, tip, and total. Color the data points according to the pickup_zone method to compare how different zones affect these variables.

**Python Code:**

```python
# Plot pairwise relationships between numerical variables
g = sns.pairplot(
    data=df,
    vars=['distance', 'fare', 'tip', 'total'],
    hue='pickup_zone',
    palette='rocket'
)

plt.show()
```

**Output:** A pair plot showing the relationships among distance, fare, tip, and total, colored by pickup zone.

**Findings:**

1. The pair plot shows a strong positive relationship between trip distance and fare.
2. Total fare also increases as trip distance and fare increase.
3. Tip amounts show a weaker relationship with distance and fare, indicating greater variation in tipping behavior.
4. Most trips are concentrated at lower distances, fares, tips, and total amounts.
5. Different pickup zones are represented by different colors, although the large number of zones makes individual comparisons difficult.

**Insights:**

1. Trip distance is an important factor influencing fare and total trip cost.
2. Fare and total are closely related because fare contributes directly to the total amount.
3. Tipping behavior varies across trips and is not determined by trip distance alone.
4. The pair plot helps identify overall relationships and unusual values, but the large number of pickup zones limits clear zone-by-zone comparisons.

### ★ Violin Plot

**Original Assignment Question:**

Plot a violin plot to show the distribution of fare for each payment method. Use the payment method as the categorical axis and fare as the numeric axis to visualize its distribution.

**Python Code:**

```python
# Create violin plot for fare by payment method
sns.violinplot(
    data=df,
    x='payment',
    y='fare',
    hue='payment',
    palette='rocket',
    legend=False
)

plt.title('Distribution of Fare by Payment Method')
plt.xlabel('Payment Method')
plt.ylabel('Fare')
plt.show()
```

**Output:** A violin plot showing fare distributions for credit card and cash payments.

**Findings:**

1. The violin plot shows the distribution of fare amounts for credit card and cash payments.
2. Most fare amounts for both payment methods are concentrated in the lower fare range, particularly below 20.
3. Both payment methods show similar central fare distributions, with most trips having relatively low fares.
4. Cash payments show a slightly wider spread in some fare ranges compared to credit card payments.
5. Both payment methods have long upper tails, indicating a small number of trips with unusually high fares.

**Insights:**

1. Lower-fare trips are common for both credit card and cash payments.
2. The overall fare distributions are broadly similar across the two payment methods.
3. High-fare trips occur with both payment methods but are relatively uncommon.
4. Payment method alone does not appear to strongly differentiate fare amounts in this dataset.

---

## Key Findings

- Manhattan has the highest recorded taxi trip count and total fare among pickup boroughs.
- Credit card payments represent 71.9% of recorded trips, compared with 28.1% for cash.
- Most trips are relatively short.
- Fare and total have a strong positive correlation of approximately 0.97.
- Distance and fare also have a strong positive correlation of approximately 0.95.
- Tip distributions vary across boroughs.
- Fare distributions are broadly similar for cash and credit card payments.

## Conclusion

This assignment demonstrates loading and cleaning a real-world taxi dataset and using ten visualizations to explore time trends, category comparisons, numerical distributions, and correlations.

Pandas and Matplotlib were used for the Line Chart, Bar Chart, Pie Chart, Histogram, and Box Plot. Seaborn was used for the Count Plot, Scatter Plot, Heatmap, Pair Plot, and Violin Plot.

The findings highlight the concentration of taxi trips in Manhattan, the prevalence of credit card payments, the frequency of shorter trips, and the strong association between trip distance and fare.

Overall, the project strengthened practical skills in data cleaning, exploratory data analysis, visualization, and interpretation of analytical results using Python.

## Repository Files

| File | Description |
|---|---|
| `Python_DA_Assignment_2_Data_Visualization_Solution.ipynb` | Completed Jupyter Notebook containing Python code, outputs, visualizations, findings, and insights |
| `README.md` | Project overview, original assignment questions, Python code, outputs, findings, insights, and conclusion |
| `Python DA Assignment 2 – Data Visualization.pdf` *(optional)* | Original assignment instruction file, if uploaded |

## Author

**Deepthi D**

GitHub: [DeepthiD13](https://github.com/DeepthiD13)
