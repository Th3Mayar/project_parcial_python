## **Project Overview**
This project involves the analysis of three distinct datasets using Python. The datasets represent:
1. Supermarket sales data.
2. Academic performance data of university students.
3. Store inventory data.

For each dataset, several tasks are performed using `pandas` and `numpy` libraries, including filtering, grouping, and performing calculations. Additionally, data is visualized and highlighted using Pandas styling to enhance readability and presentation.

## **Requirements**

### **Libraries Needed:**
- `pandas`: For data manipulation and analysis.
- `numpy`: For numerical operations and handling arrays.
- Jupyter Notebook or Google Colab for running the code and viewing the styled tables.

Install any missing libraries using:
```bash
pip install pandas numpy
```

---

## **Problem 1: Supermarket Sales Analysis**

### **Objective:**
Analyze the supermarket sales data to calculate the total sales for each product, filter sales from March, and calculate the average sales during that month.

### **Steps:**
1. **Data Loading**: Import the sales data from a CSV file using `pandas`.
2. **Data Preparation**: Convert the 'Date' column to a `datetime` format and add a new column called 'Total Sale', which is calculated as `Quantity * Price - Discount`.
3. **Total Sales Calculation**: Group the data by product and calculate the total sales, then sort the results.
4. **March Filter**: Filter the dataset to show only sales from March using the `pd.to_datetime()` function.
5. **Array and Average Calculation**: Generate a `numpy` array with the total sales in March and calculate the average sales during this month.
6. **Data Styling**: Highlight the highest and lowest total sales using Pandas `.style`.

### **Key Functions/Methods Used:**
- `pd.read_csv()`: For reading CSV files into a DataFrame.
- `pd.to_datetime()`: For converting date strings to `datetime` objects.
- `groupby()`: For grouping data by product.
- `np.mean()`: For calculating the average of a NumPy array.
- Pandas styling for table visualization.

### **Example Code:**
```python
import pandas as pd
import numpy as np

# Load data
df_sales = pd.read_csv('sales_supermarket.csv')

# Convert 'Date' to datetime and create 'Total Sale' column
df_sales['Fecha'] = pd.to_datetime(df_sales['Fecha'])
df_sales['Total Sale'] = df_sales['Cantidad'] * df_sales['Precio'] - df_sales['Descuento']

# Calculate total sales by product and sort
total_sales = df_sales.groupby('Producto')['Total Sale'].sum().sort_values(ascending=False)

# Filter sales from March
march_sales = df_sales[df_sales['Fecha'].dt.month == 3]

# Generate NumPy array and calculate average sales in March
march_sales_array = np.array(march_sales.groupby('Producto')['Total Sale'].sum())
march_sales_avg = np.mean(march_sales_array)

# Styling the table
styled_sales = df_sales.style.highlight_max(subset=['Total Sale'], color='lightgreen')\
                               .highlight_min(subset=['Total Sale'], color='lightcoral')
styled_sales
```

---

## **Problem 2: Academic Performance Analysis**

### **Objective:**
Analyze the academic performance of students by calculating their average grades and determining whether they passed or failed based on certain criteria.

### **Steps:**
1. **Data Loading**: Load the student grades dataset from a CSV file.
2. **Average Calculation**: Add a column that calculates the average grade for each student.
3. **Pass/Fail Assignment**: Create a new column that assigns "Passed" if the average grade is 60 or above, otherwise "Failed".
4. **Counting Passed/Failed Students**: Calculate how many students passed and how many failed.
5. **Filter for Students Who Passed All Subjects**: Filter students who scored at least 70 in all subjects.
6. **Data Styling**: Apply conditional styling to highlight "Passed" in green and "Failed" in red.

### **Key Functions/Methods Used:**
- `np.where()`: For assigning "Passed" or "Failed" based on the average grade.
- Conditional filtering using logical operators (`&` for combining conditions).
- Pandas styling for status highlighting.

### **Example Code:**
```python
import pandas as pd
import numpy as np

# Load data
df_students = pd.read_csv('students_grades.csv')

# Calculate average grade
df_students['Average'] = df_students[['Matemáticas', 'Ciencias', 'Historia']].mean(axis=1)

# Assign "Passed" or "Failed"
df_students['Status'] = np.where(df_students['Average'] >= 60, 'Passed', 'Failed')

# Count how many passed and failed
passed_students = (df_students['Status'] == 'Passed').sum()
failed_students = (df_students['Status'] == 'Failed').sum()

# Filter students who passed all subjects with >= 70
passed_all = df_students[
    (df_students['Matemáticas'] >= 70) & 
    (df_students['Ciencias'] >= 70) & 
    (df_students['Historia'] >= 70)
]

# Styling the table
styled_students = df_students.style.applymap(lambda x: 'background-color: lightgreen' if x == 'Passed' else 'background-color: lightcoral', subset=['Status'])
styled_students
```

---

## **Problem 3: Store Inventory Simulation**

### **Objective:**
Simulate a store’s inventory by calculating the total value of products in stock, identifying products that need restocking, and calculating the restocking cost.

### **Steps:**
1. **Data Loading**: Load the inventory dataset from a CSV file.
2. **Total Value Calculation**: Add a column to calculate the total value of each product in inventory.
3. **Restocking Cost Calculation**: If the quantity of a product is below 20, calculate the cost of restocking it up to 50 units.
4. **Filtering Low Stock Items**: Filter products that need restocking and sort them by restocking cost.
5. **Data Styling**: Format the total values and restocking costs as currency, and highlight low stock products.

### **Key Functions/Methods Used:**
- `np.where()`: For conditional calculations (restocking cost).
- Pandas styling for currency formatting and highlighting low inventory.

### **Example Code:**
```python
import pandas as pd
import numpy as np

# Load data
df_inventory = pd.read_csv('store_inventory.csv')

# Calculate total value in inventory
df_inventory['Total Value in Inventory'] = df_inventory['Cantidad'] * df_inventory['Precio unitario']

# Calculate restocking cost for products with quantity < 20
df_inventory['Restocking Cost'] = np.where(df_inventory['Cantidad'] < 20, 
                                           (50 - df_inventory['Cantidad']) * df_inventory['Costo de reabastecimiento'], 
                                           0)

# Filter products that need restocking
products_to_restock = df_inventory[df_inventory['Cantidad'] < 20].sort_values(by='Restocking Cost', ascending=False)

# Styling the table
styled_inventory = df_inventory.style.format({
    'Total Value in Inventory': '${:,.2f}', 
    'Restocking Cost': '${:,.2f}'
}).highlight_min(subset=['Cantidad'], color='lightcoral', axis=0)
styled_inventory
```

---

## **Conclusion**

This project demonstrates how Python's `pandas` and `numpy` libraries can be used to analyze and manipulate data in real-world scenarios. The project applies a variety of data processing techniques such as grouping, filtering, and calculating statistics, while also showcasing the use of Pandas styling to enhance the visual presentation of the data.
