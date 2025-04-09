import numpy as np 
import pandas as pd

import os
for dirname, _, filenames in os.walk('/kaggle/input'):
    for filename in filenames:
        print(os.path.join(dirname, filename))

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

data = pd.read_excel('C:\\Users\\LENOVO\\OneDrive\\Desktop\\amazon sales data analysis.xlsx')

data.head()

data.isnull().sum()

data["Order Date"] = pd.to_datetime(data["Order Date"])

data["Year"]= data["Order Date"].dt.year  
data["Month"]= data["Order Date"].dt.month
data.head()

regions = data["Region"].nunique()
print("Number of Regions:", regions)

country = data["Country"].nunique()
print("Number of Countries:", country)

item_type = data["Item Type"].nunique()
print("Number of Item Types:", item_type)

unit_sold = data["Units Sold"].sum()
print("Total Unit Sold:", unit_sold)

unit_cost = data["Unit Cost"].sum()
print("Total Unit cost:", unit_cost)


total_revenue = data["Total Revenue"].sum()
print("Total Revenue:", total_revenue)

total_cost = data["Total Cost"].sum()
print("Total Cost:", total_cost)

total_profit = data["Total Profit"].sum()
print("Total Profit:", total_profit)

profit_by_region_channel = data.groupby(['Region', 'Sales Channel'])['Total Profit'].sum().sort_values(ascending=False)
print(profit_by_region_channel)

year_sales = data.groupby('Year')['Total Revenue'].mean().sort_values(ascending=False)
plt.figure(figsize=(8,3))
sns.barplot(x=year_sales.index, y=year_sales.values)
plt.grid(axis='y') 
plt.title('Average Sales By Year')
plt.xlabel('Year')
plt.ylabel('Total Revenue')
plt.show()

plt.figure(figsize=(6, 6))
region_TotalRevenue = data.groupby('Region')['Total Profit'].mean()
plt.pie(region_TotalRevenue, startangle=90, labels=region_TotalRevenue.index, autopct='%1.1f%%')
plt.title('Average Profit in Region wise')
plt.show()

TotalRevenue_ItemType = data.groupby('Item Type') ['Total Revenue'].sum()
plt.figure(figsize=(8,4))
TotalRevenue_ItemType.sort_values(ascending=False).plot(kind='bar')
plt.xlabel('Item Type')
plt.ylabel('Total Revenue')
plt.title('Total Revenue by Item Type')
plt.grid(axis='y')
plt.show()

TotalRevenue_by_SalesChannel = data.groupby('Sales Channel')['Total Revenue'].mean()
plt.figure(figsize=(6, 6))
plt.pie(TotalRevenue_by_SalesChannel, labels=TotalRevenue_by_SalesChannel.index, autopct='%1.1f%%', startangle=90)
plt.title('Average Total Revenue by Sales Channel')  # Corrected title
plt.tight_layout()
plt.show()

Region_UnitSold = data.groupby('Region')['Units Sold'].sum()
plt.figure(figsize=(8, 6)) 
plt.pie(Region_UnitSold, labels=Region_UnitSold.index, autopct='%1.1f%%', startangle=140, wedgeprops=dict(width=0.3))  
plt.title('Units Sold by Region (Donut Chart)', fontsize=14) 
plt.show()

UnitSold_YearMonth= data.groupby(['Year','Month'])['Units Sold'].sum().sort_values(ascending=False)

plt.figure(figsize=(11,6))
UnitSold_YearMonth.plot(kind='bar')
plt.xlabel('Year & Month')
plt.ylabel('Unit Sold')
plt.tight_layout()
plt.grid(axis='y')

TotalCost_SalesChannel= data.groupby('Sales Channel')['Total Cost'].sum()

plt.figure(figsize=(6,6))
TotalCost_SalesChannel.plot(kind='pie',autopct='%1.1f%%',startangle=90)
plt.title('Total Cost by Sales Channel')
plt.tight_layout()

# plt.figure(figsize=(10, 6))  

# sns.regplot(x='Units Sold', y='Total Profit',data=data, ci=None, scatter_kws={'s': 20}, line_kws={'color':'skyblue'})  # ci=None removes confidence interval


# plt.title('Relationship between Units Sold and Total Profit', fontsize=14)
plt.xlabel('Units Sold', fontsize=12)
plt.ylabel('Total Profit', fontsize=12)
plt.grid(axis='y') 
plt.ticklabel_format(style='plain', axis='y') 
sns.despine()
plt.tight_layout()
plt.show()

# Profit_Margin = (data['Total Profit'] / data['Total Revenue']) * 100 

# plt.figure(figsize=(8, 6))  
plt.hist(Profit_Margin, bins=range(10, 71, 5), rwidth=0.8, color='cornflowerblue') 
plt.title('Distribution of Profit Margins', fontsize=14)
plt.xlabel('Profit Margin (%)', fontsize=12)
plt.ylabel('Frequency', fontsize=12)
plt.xticks(range(10, 71, 5), rotation=45)
plt.grid(axis='y', alpha=0.75)  
plt.gca().spines['top'].set_visible(False)
plt.gca().spines['right'].set_visible(False)
plt.tight_layout()

