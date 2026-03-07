---
name: data-analysis
description: Analyze data with Python — statistics, visualization, machine learning, network analysis. Use when user needs charts, trends, correlations, clustering, or any quantitative analysis. Triggers on "analyze this data", "make a chart", "plot", "statistics", "trend analysis", "visualize", "correlation".
allowed-tools: Bash(python3:*), Bash(pip3:*)
---

# Data Analysis

Analyze and visualize data using Python's scientific computing stack.

## Available Libraries

| Library | Purpose |
|---------|---------|
| **pandas** | DataFrames, CSV/JSON/Excel parsing, aggregation |
| **numpy** | Numerical computing, linear algebra, statistics |
| **scipy** | Scientific computing, hypothesis testing, optimization |
| **matplotlib** | Publication-quality plots and charts |
| **seaborn** | Statistical visualization (heatmaps, distributions) |
| **scikit-learn** | ML: clustering, classification, regression, dimensionality reduction |
| **networkx** | Graph/network analysis (citation networks, relationships) |

## Quick Recipes

### Load and summarize data
```python
import pandas as pd
df = pd.read_csv("data.csv")
print(df.describe())
print(df.info())
print(df.head(10))
```

### Time series trend
```python
import pandas as pd
import matplotlib.pyplot as plt
df = pd.read_csv("data.csv", parse_dates=["date"])
df.set_index("date")["value"].plot(figsize=(12,6), title="Trend Over Time")
plt.tight_layout()
plt.savefig("trend.png", dpi=150)
```

### Correlation heatmap
```python
import seaborn as sns
import matplotlib.pyplot as plt
corr = df.select_dtypes(include="number").corr()
sns.heatmap(corr, annot=True, cmap="coolwarm", center=0, fmt=".2f")
plt.tight_layout()
plt.savefig("correlations.png", dpi=150)
```

### Clustering (K-Means)
```python
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler
X = StandardScaler().fit_transform(df[["feature1", "feature2"]])
labels = KMeans(n_clusters=3, random_state=42).fit_predict(X)
df["cluster"] = labels
```

### Statistical test
```python
from scipy import stats
group_a = df[df["group"]=="A"]["metric"]
group_b = df[df["group"]=="B"]["metric"]
t_stat, p_value = stats.ttest_ind(group_a, group_b)
print(f"t={t_stat:.3f}, p={p_value:.4f}")
```

### Network/graph analysis
```python
import networkx as nx
G = nx.from_pandas_edgelist(edges_df, "source", "target", edge_attr="weight")
print(f"Nodes: {G.number_of_nodes()}, Edges: {G.number_of_edges()}")
centrality = nx.betweenness_centrality(G)
top = sorted(centrality.items(), key=lambda x: x[1], reverse=True)[:10]
```

### SEC EDGAR financial data
```python
from edgar import Company
company = Company("AAPL")
filings = company.get_filings(form="10-K").latest(5)
for f in filings:
    print(f"{f.filing_date}: {f.form} - {f.description}")
```

## Workflow

1. **Understand the data**: Load, inspect shape, types, missing values
2. **Clean**: Handle nulls, fix types, remove duplicates
3. **Explore**: Summary stats, distributions, correlations
4. **Analyze**: Apply appropriate technique (regression, clustering, time series, etc.)
5. **Visualize**: Create clear, labeled charts
6. **Report**: Summarize findings with specific numbers

## Chart Guidelines

- Always include axis labels and title
- Use `tight_layout()` to prevent label clipping
- Save at 150+ DPI for readability
- Use colorblind-friendly palettes when possible
- Keep charts simple — one insight per chart

## When to Use Which Technique

| Question | Technique |
|----------|-----------|
| "Is there a trend?" | Time series plot, rolling average |
| "Are these groups different?" | t-test, Mann-Whitney, ANOVA |
| "What correlates with what?" | Correlation matrix, scatter plots |
| "Are there natural groups?" | K-Means, DBSCAN clustering |
| "What predicts this outcome?" | Linear/logistic regression, random forest |
| "How are things connected?" | Network analysis with networkx |
| "What's the distribution?" | Histogram, KDE, box plots |
