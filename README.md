# Page View Time Series Visualizer

This is the boilerplate for the Page View Time Series Visualizer project. Instructions for building your project can be found at https://www.freecodecamp.org/learn/data-analysis-with-python/data-analysis-with-python-projects/page-view-time-series-visualizer

# My collaboration in the project:
This Python script visualizes time series data from the freeCodeCamp.org forum, specifically the daily page views from May 2016 to December 2019. The visualization includes a line chart, a bar chart, and box plots to identify patterns, trends, and seasonality in the data.

## Data Preparation

### 1 - Import and Parse Data:
The script imports data from "fcc-forum-pageviews.csv" and parses the dates, setting the 'date' column as the index.
```python
df = pd.read_csv('fcc-forum-pageviews.csv', parse_dates=['date'], index_col='date')
```
### 2 - Data Cleaning:
The data is filtered to exclude days when page views were in the top 2.5% or bottom 2.5% of the dataset.
```python
df = df[(df['value'] >= df['value'].quantile(0.025)) & (df['value'] <= df['value'].quantile(0.975))]
```

## Data Visualization Functions

### 1 - Draw Line Plot:
The draw_line_plot function creates a line chart using Matplotlib to visualize daily page views over the entire period.
```python
fig, ax = plt.subplots(figsize=(14, 6))
ax.plot(df.index, df['value'], color='r', linewidth=1)
ax.set_title('Daily freeCodeCamp Forum Page Views 5/2016-12/2019')
ax.set_xlabel('Date')
ax.set_ylabel('Page Views')
fig.savefig('line_plot.png')
```

### 2 - Draw Bar Plot:
The draw_bar_plot function generates a bar chart to display the average daily page views for each month grouped by year.
```python
fig, ax = plt.subplots(figsize=(14, 6))
df_bar = df.copy()
df_bar['year'] = df_bar.index.year
df_bar['month'] = df_bar.index.month
df_bar = df_bar.groupby(['year', 'month']).mean().unstack()
df_bar.plot(kind='bar', ax=ax)
ax.set_title('Average Page Views Per Year and Month')
ax.set_xlabel('Years')
ax.set_ylabel('Average Page Views')
ax.legend(title='Months', labels=[month.strftime('%b') for month in pd.date_range(start='2022-01-01', end='2022-12-01', freq='M')])
fig.savefig('bar_plot.png')
```

### 3 - Draw Box Plot:
The draw_box_plot function uses Seaborn to create two adjacent box plots, illustrating the distribution of page views within a given year and month.
```python
fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(18, 6))
sns.boxplot(x='year', y='value', data=df_box, ax=axes[0])
sns.boxplot(x='month', y='value', data=df_box, order=pd.date_range(start='2022-01-01', end='2022-12-01', freq='M').strftime('%b'), ax=axes[1])
axes[0].set_title('Year-wise Box Plot (Trend)')
axes[0].set_xlabel('Year')
axes[0].set_ylabel('Page Views')
axes[1].set_title('Month-wise Box Plot (Seasonality)')
axes[1].set_xlabel('Month')
axes[1].set_ylabel('Page Views')
fig.savefig('box_plot.png')
```
