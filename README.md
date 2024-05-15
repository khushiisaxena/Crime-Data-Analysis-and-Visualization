## Project Title: Crime Data Analysis and Visualization

### Overview
This project involves importing, cleaning, and analyzing crime data in India from 2001 to 2012. The primary objectives are to preprocess the data, visualize various crime trends, and extract meaningful insights through different types of plots and visualizations.

### Table of Contents
1. [Data Import and Cleaning](#data-import-and-cleaning)
2. [Data Visualization](#data-visualization)
3. [Usage](#usage)
4. [Dependencies](#dependencies)
5. [Contributing](#contributing)
6. [License](#license)

### Data Import and Cleaning
The initial step involves reading the crime dataset from a CSV file and performing data cleaning operations to prepare it for analysis.

#### Steps:
1. **Reading Data:** Load the dataset using `pandas.read_csv()`.
2. **Dropping Columns:** Remove irrelevant or less significant columns.
3. **Filtering Rows:** Exclude rows that aggregate data at higher levels like 'TOTAL' and 'DELHI UT TOTAL'.
4. **Sorting:** Sort the DataFrame based on 'STATE/UT'.
5. **Saving Cleaned Data:** Save the cleaned data to a new CSV file for further use.

```python
import pandas as pd

# Load the data
df = pd.read_csv('/Users/yashsingh/Desktop/DAV_Project_Crime/01_District_wise_crimes_committed_IPC_2001_2012.csv')

# Drop unnecessary columns
columns_to_drop = ['CULPABLE HOMICIDE NOT AMOUNTING TO MURDER', 'CUSTODIAL RAPE', 'OTHER RAPE', 
                   'PREPARATION AND ASSEMBLY FOR DACOITY', 'AUTO THEFT', 'OTHER THEFT', 
                   'CRIMINAL BREACH OF TRUST', 'ASSAULT ON WOMEN WITH INTENT TO OUTRAGE HER MODESTY', 
                   'IMPORTATION OF GIRLS FROM FOREIGN COUNTRIES', 'CAUSING DEATH BY NEGLIGENCE', 
                   'ATTEMPT TO MURDER', 'OTHER THEFT', 'KIDNAPPING AND ABDUCTION OF WOMEN AND GIRLS', 
                   'COUNTERFIETING', 'KIDNAPPING AND ABDUCTION OF OTHERS', 
                   'PREPARATION AND ASSEMBLY FOR DACOITY']

df_cleaned = df.drop(columns=columns_to_drop)

# Filter out 'TOTAL' and 'DELHI UT TOTAL' from 'DISTRICT'
df_cleaned = df_cleaned[(df_cleaned['DISTRICT'] != 'TOTAL') & (df_cleaned['DISTRICT'] != 'DELHI UT TOTAL')]

# Sort by 'STATE/UT'
df_sorted = df_cleaned.sort_values(by='STATE/UT')

# Save cleaned data
df_sorted.to_csv('/Users/yashsingh/Desktop/DAV_Project_Crime/cleaned_file.csv', index=False)
```

### Data Visualization
Various plots are created to visualize crime data trends over the years, across different states, and for specific crime types. The visualizations include bar charts, line plots, box plots, heatmaps, violin plots, and pie charts.

#### Examples:
1. **Total IPC Crimes by State:**
    ```python
    import matplotlib.pyplot as plt
    import seaborn as sns
    
    # Group data by 'STATE/UT' and aggregate crime counts
    df_grouped = df_sorted.groupby('STATE/UT').sum()
    
    # Plot
    plt.figure(figsize=(12, 6))
    colors = sns.color_palette("hsv", len(df_grouped))
    df_grouped['TOTAL IPC CRIMES'].plot(kind='bar', color=colors)
    plt.title('Total IPC Crimes by State (2001-2012)')
    plt.xlabel('State/UT')
    plt.ylabel('Total IPC Crimes')
    plt.xticks(rotation=90)
    plt.tight_layout()
    plt.show()
    ```

2. **Crime Distribution in Madhya Pradesh:**
    ```python
    # Filter data for Madhya Pradesh
    df_mp = df_sorted[df_sorted['STATE/UT'] == 'MADHYA PRADESH']
    
    # Plot
    plt.figure(figsize=(12, 6))
    sns.barplot(x='YEAR', y='TOTAL IPC CRIMES', data=df_mp, palette='coolwarm')
    plt.title('Total IPC Crimes in Madhya Pradesh (2001-2012)')
    plt.xlabel('Year')
    plt.ylabel('Total IPC Crimes')
    plt.xticks(rotation=45)
    plt.tight_layout()
    plt.show()
    ```

3. **Correlation Matrix Heatmap:**
    ```python
    # Drop non-relevant columns
    data_corr = df_sorted.drop(columns=['STATE/UT', 'DISTRICT', 'YEAR', 'TOTAL IPC CRIMES', 'OTHER IPC CRIMES'])
    
    # Calculate the correlation matrix
    corr = data_corr.corr()
    
    # Generate a heatmap
    plt.figure(figsize=(10, 8))
    sns.heatmap(corr, annot=True, fmt=".2f", cmap='coolwarm')
    plt.title('Correlation Matrix of Crime Data')
    plt.xticks(rotation=45)
    plt.yticks(rotation=0)
    plt.tight_layout()
    plt.show()
    ```

### Usage
To use this project, clone the repository and execute the code in a Python environment with the required libraries installed. The data visualization scripts will generate various plots to help analyze the crime data trends.

### Dependencies
- `pandas`
- `matplotlib`
- `seaborn`
- `numpy`
- `scipy`
- `statsmodels`
- `squarify`

### Contributing
Contributions are welcome! Please fork the repository and submit a pull request.

### License
This project is licensed under the MIT License.
