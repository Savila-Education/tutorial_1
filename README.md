# 🎨 Conditional Formatting in Pandas — Supply Chain Lead Time Edition

Welcome to this hands-on tutorial where we use Python to spot supply chain delays. 
We go beyond just coloring a table: we **analyze lead times**, **count urgent orders**, and **identify which suppliers are underperforming**.

---

## 🎯 What you’ll learn

In this tutorial, we tackle a common supply chain challenge: **spotting delivery issues fast** using Python Pandas.

We’ll:
- Use **conditional formatting** to color the `lead_time` column in a DataFrame  
  → 🟢 Green for *optimal*, 🟡 yellow for *standard*, 🔴 red for *urgent*  
- Set custom thresholds to define what “urgent” really means in our context  
- **Count how many orders** fall into each lead time category  
- **Find out which suppliers** are consistently delivering late by building a **pivot table** to rank suppliers by their total urgent lead time

This project uses a fictional supply chain dataset and runs 100% in **Google Colab**.

---
## Uploading the dataset

Two options to load the dataset into your notebook: 
1. Use the GitHub URL directly

```python
#import dataset from github
url = 'https://raw.githubusercontent.com/Savila-Education/tutorial_1/refs/heads/main/supply_chain_data_color.csv'
#dataframe from url
df = pd.read_csv(url)
```
2. Upload the file manually to Google Colab
```python
#import dataset local file 
df = pd.read_csv('supply_chain_data_color.csv')
```
---
## 🧠 Key Concepts

### 🎯 Threshold Function

We build a function that:
- Takes in two values `x` and `y`
- Applies color formatting based on:
  - `Green` if value < x → Optimal
  - `Yellow` if x ≤ value ≤ y → Standard
  - `Red` if value > y → Urgent

```python
def threshold_style(x, y):
    def style_func(val):
        if val < x:
            return 'background-color: #77DD77' #green
        elif x <= val <= y:
            return 'background-color: #FDFD96' #yellow
        else:
            return 'background-color: #FF6961' #red
    return style_func
```

---

### 🖌️ Apply styling to DataFrame

Apply the formatting only to the `lead_time` column:

```python
styled_df = df.style.map(threshold_style(x_threshold, y_threshold), subset=['lead_time'])
```

---

## 📊 Count orders by category

Count how many lead times fall into each category:

```python
green_count = (df['lead_time'] < x_threshold).sum()
yellow_count = ((df['lead_time'] >= x_threshold) & (df['lead_time'] <= y_threshold)).sum()
red_count = (df['lead_time'] > y_threshold).sum()

print("Optimal:", green_count)
print("Standard:", yellow_count)
print("Urgent:", red_count)
```

---

## 🚩 Identify worst suppliers

Let’s figure out who’s causing the most problems:

```python
urgent_df = df[df['lead_time'] > y_threshold]

pd.pivot_table(urgent_df,values = 'lead_time',
               index = 'supplier_id',
               aggfunc='sum').reset_index().\
               sort_values(by='lead_time',ascending=False).rename(columns={'lead_time':'sum_lead_time'})
```

This helps you **spot suppliers** that are racking up urgent orders.

---

## 🧪 Use case

You can use this for:
- Internal reporting
- Supplier negotiations
- Proactive risk management
- Transitioning away from Excel-based tracking

---

## 🎥 Watch the full video

👉 [https://youtu.be/csK2QjD_LlM](#)  
We walk through this in Google Colab — step by step!

---

## 🛠️ Tech used

- Python Python 3.11.11
- pandas
- Google Colab

---

## 🧰 Bonus ideas to try

- Switch from `sum` to `count` or `mean` in the pivot table
- Add more categories (e.g., “critical”)
- Plot results with matplotlib or seaborn

---

## Connect with us 

- Instagram → [@savila.education](#)  
- TikTok → [@savila.education](#)  
- LinkedIn → [Stephania Kossman](#)
- LinkedIn → [Luis Fernandon Pérez](#)

Have a tutorial request? Drop it in the comments — we're building this channel with you 🙌
