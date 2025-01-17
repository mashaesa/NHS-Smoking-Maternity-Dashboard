# NHS-Smoking-Maternity-Dashboard
A Power BI dashboard analysing smoking trends during maternity across NHS regions, including recommendations for improvement.

## Project Overview
This is my first Power BI dashboard, analysing smoking trends during maternity across NHS regions. Using NHS data, the report compares smoking rates, highlights areas for improvement, and provides actionable recommendations to the NHS.
## Dashboard Screenshots

### Dashboard Overview
![Dashboard Overview](Screenshot%202025-01-16%20at%2012.16.59.jpeg)

### Regional Smoking Comparisons
![Regional Smoking Comparisons](Screenshot%202025-01-16%20at%2012.13.26.jpeg)

### Key Findings and Recommendations
![Key Findings and Recommendations](Screenshot%202025-01-16%20at%2012.14.27.jpeg)

## Tools Used
- **Power BI**: For data modeling, visualisation, and dashboard creation.
- **Excel/CSV**: For preprocessing and cleaning raw data.
- **DAX**: For advanced calculations like Weighted Smoking Rate.

## Key Findings
- Highest percentage of smokers: Nottingham and Nottinghamshire (14.02%).
- High number of "Smoking Status Unknown" in Devon.
- Regional disparities between Northeast and Yorkshire vs. London.

## Smoking and Maternity Report: Key Findings and Analysis

### Highest Percentage of Smokers in NHS Region: Nottingham and Nottinghamshire (14.02%)
- Could be due to socio-economic factors, such as lower income. There may also be limited access to the correct healthcare and advice for smoking.
- **Recommendation:** Increase funding and availability for anti-smoking programmes within this region.

### High Number of ‘Smoking Status Unknown’ in Devon
The line chart comparison revealed that Devon reported 20% of cases where smoking status was unknown during maternity.
- Potential causes:
  - Incomplete or inconsistent data collection and reporting practices.
  - Lack of correct protocols for ensuring smoking status was accurately reported during healthcare visits.
  - Expecting mothers may have been unwilling to share certain information.
- **Recommendations:**
  - Implement better training for healthcare staff to improve the data collection process.
  - Introduce a non-judgmental approach in consultations to allow for more accurate data collection.

### Regional Division Comparisons: Northeast and Yorkshire vs. London
The regional division comparison recorded that the Northeast and Yorkshire had the highest percentage of smokers during maternity, whilst London had the lowest overall percentage.
- Potential reasons:
  - Lower incomes and limited access to smoking cessation resources in comparison to London.
  - Cultural factors that influence smoking rates between the two areas.
  - London may have greater funding and therefore access to healthcare resources, education, and public health campaigns targeting smoking.

- **Recommendations:**
  - Expand public health campaigns in the Northeast and Yorkshire area and prioritise low-income families especially.
  - Conduct a case study on the North West area of London specifically to identify successful strategies and adapt them to higher-risk areas.
  - Develop a nationwide strategy to reduce smoking during maternity and focus especially on higher-risk areas such as Nottinghamshire, the Northeast, and Yorkshire.

## Access the Dashboard
- View the interactive site with screenshots and findings [here](https://mashaesa.github.io/NHS-Smoking-Maternity-Dashboard).
---
### ANALYSING WITH PYTHON (EXTRA TASK)

```python
import pandas as pd

file_path = "nhs_smoking_data.csv"  # Replace with the actual path to your dataset
data = pd.read_csv(file_path)

print("Dataset Preview:")
print(data.head())

# HANDLE MISSING VALUES
print("\nMissing Values Before Cleaning:")
print(data.isnull().sum())

# FILL MISSING SMOKING UNKNOWN
data['Smoking_Status'] = data['Smoking_Status'].fillna("Unknown")

# FILL MISSING PERCENTAGES WITH MEAN 
data['Smoking_Percentage'] = data.groupby('Region')['Smoking_Percentage'].transform(lambda x: x.fillna(x.mean()))

print("\nMissing Values After Cleaning:")
print(data.isnull().sum())

# REGIONAL STATISTICS
regional_summary = data.groupby('Region').agg({
    'Smoking_Percentage': 'mean',
    'Cases_Reported': 'sum',
    'Smoking_Status': lambda x: (x == "Unknown").sum()  # Count unknown statuses
}).reset_index()

regional_summary.rename(columns={
    'Smoking_Percentage': 'Average_Smoking_Percentage',
    'Cases_Reported': 'Total_Cases',
    'Smoking_Status': 'Unknown_Status_Count'
}, inplace=True)

# REGIONS WITH HIGHEST SMOKING PERCENTAGE
high_risk_regions = regional_summary[regional_summary['Average_Smoking_Percentage'] > regional_summary['Average_Smoking_Percentage'].mean()]

print("\nHigh-Risk Regions:")
print(high_risk_regions)

regional_summary.to_csv("regional_summary.csv", index=False)
print("\nRegional summary saved to 'regional_summary.csv'.")

print("\nKey Insights:")
print(f"Region with the highest smoking percentage: {high_risk_regions.iloc[0]['Region']} ({high_risk_regions.iloc[0]['Average_Smoking_Percentage']:.2f}%)")
print(f"Region with the highest unknown smoking statuses: {regional_summary.loc[regional_summary['Unknown_Status_Count'].idxmax()]['Region']} ({regional_summary['Unknown_Status_Count'].max()} cases)")

print("\nRecommendations:")
print("- Implement targeted anti-smoking programs in high-risk regions.")
print("- Improve data collection protocols to reduce 'Unknown' smoking statuses.")
