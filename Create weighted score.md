category_analysis = (
    df.groupby('category')
    .agg(
        top_products=('product_name', lambda x: list(x.head(5))),
        total_products=('product_name', 'count'),
        average_rating=('rating', 'mean'),
        total_reviews=('rating_count', 'sum')
    )
    .reset_index()
)

category_analysis['weighted_score'] = (
    category_analysis['average_rating'] *
    category_analysis['total_reviews'].apply(lambda x: np.log1p(x))
)

category_analysis = (
    category_analysis
    .sort_values(
        by='weighted_score',
        ascending=False
    )
    .head(20)
)

category_analysis

The objective of this code is:

> **Find the best-performing product categories by combining product ratings and review popularity into a weighted ranking score.**

A simple average rating can be misleading, so we create a **weighted score** that considers both:

* How good the products are (`average_rating`)
* How many customers reviewed them (`total_reviews`)

---

# Step 1: Group products by category

```python
category_analysis = (
    df.groupby('category')
```

### What happens?

Your dataset contains multiple products:

Example:

| product_name  | category             | rating | rating_count |
| ------------- | -------------------- | ------ | -----------: |
| Wayona Cable  | Computer Accessories | 4.2    |        24269 |
| Ambrane Cable | Computer Accessories | 4.0    |        43994 |
| Boat Cable    | Computer Accessories | 4.2    |        94363 |
| Coffee Maker  | Kitchen              | 4.5    |         1065 |

`groupby('category')` creates groups:

```
Computer Accessories
    |
    |-- Wayona Cable
    |-- Ambrane Cable
    |-- Boat Cable


Kitchen
    |
    |-- Coffee Maker
```

Now we can calculate category-level statistics.

---

# Step 2: Aggregate category information

```python
.agg(
```

`agg()` allows us to perform multiple calculations on each category.

---

## A. Collect top 5 product names

```python
top_products=('product_name', lambda x: list(x.head(5)))
```

Meaning:

Take the `product_name` column from each category.

Example:

Category:

```
Computer Accessories
```

Products:

```
1. Wayona USB Cable
2. Ambrane Cable
3. Sounce Cable
4. Boat Cable
5. Portronics Cable
6. Amazon Basics Cable
```

`head(5)` selects:

```
[
Wayona USB Cable,
Ambrane Cable,
Sounce Cable,
Boat Cable,
Portronics Cable
]
```

`list()` converts it into a Python list.

Output:

```
top_products:

[
'Wayona USB Cable',
'Ambrane Cable',
'Sounce Cable',
'Boat Cable',
'Portronics Cable'
]
```

---

## B. Count products in each category

```python
total_products=('product_name', 'count')
```

Counts how many products exist in each category.

Example:

Before:

```
Category:
Computer Accessories

Products:
Wayona
Ambrane
Boat
Sounce
```

After:

```
total_products = 4
```

---

## C. Calculate average rating

```python
average_rating=('rating', 'mean')
```

Calculates the average rating of products in that category.

Example:

Computer Accessories:

```
Product Ratings:

Wayona       4.2
Ambrane      4.0
Boat         4.2
Sounce       3.9
```

Calculation:

```
(4.2 + 4.0 + 4.2 + 3.9) / 4
```

Result:

```
average_rating = 4.075
```

---

## D. Calculate total reviews

```python
total_reviews=('rating_count', 'sum')
```

Adds all customer reviews.

Example:

```
Wayona       24,269
Ambrane      43,994
Boat         94,363
Sounce        7,928
```

Total:

```
170,554 reviews
```

This tells us the popularity/trust level.

---

# Step 3: Reset index

```python
.reset_index()
```

After groupby, category becomes the dataframe index.

Before:

```
                    total_products
category
Computer Access          4
Kitchen                  8
```

After:

```
category              total_products

Computer Access            4
Kitchen                    8
```

It converts category back into a normal column.

---

# Step 4: Create weighted score

```python
category_analysis['weighted_score'] = (
```

Creates a new column.

Example:

```
category_analysis

category              average_rating

Computer Accessories       4.2
Kitchen                   4.5
```

Now we calculate ranking.

---

# Step 5: Log transform review count

```python
category_analysis['total_reviews'].apply(lambda x: np.log1p(x))
```

Why use `log1p()`?

Because review counts can be extremely large.

Example:

```
Category A:
100 reviews

Category B:
100000 reviews
```

Without scaling:

```
100000 dominates everything
```

Log transformation reduces this difference.

Example:

```python
np.log1p(100)
```

Output:

```
4.61
```

```python
np.log1p(100000)
```

Output:

```
11.51
```

Difference becomes manageable.

---

# Step 6: Calculate weighted score

Formula:

```
Weighted Score = Average Rating × log(Total Reviews)
```

Code:

```python
average_rating *
np.log1p(total_reviews)
```

Example:

## Category A

```
Rating = 5.0

Reviews = 10
```

Score:

```
5 × log(10)

= 5 × 2.39

= 11.95
```

---

## Category B

```
Rating = 4.5

Reviews = 50,000
```

Score:

```
4.5 × log(50000)

= 4.5 × 10.82

= 48.69
```

Category B ranks higher because many customers validated it.

---

# Step 7: Sort categories

```python
.sort_values(
    by='weighted_score',
    ascending=False
)
```

Sorts categories from highest score to lowest.

Example:

Before:

| category    | score |
| ----------- | ----: |
| Kitchen     |    35 |
| Electronics |    55 |
| Computers   |    42 |

After:

| category    | score |
| ----------- | ----: |
| Electronics |    55 |
| Computers   |    42 |
| Kitchen     |    35 |

---

# Step 8: Select Top 20 categories

```python
.head(20)
```

Returns only the top 20 ranked categories.

---

# Final Output Example

Your dataframe becomes:

| category             | top_products                | total_products | average_rating | total_reviews | weighted_score |
| -------------------- | --------------------------- | -------------: | -------------: | ------------: | -------------: |
| Mobile Accessories   | [Boat Cable, Ambrane Cable] |              7 |           4.47 |         55386 |           48.9 |
| Computer Accessories | [Keyboard, Mouse]           |             15 |           4.35 |         85000 |           49.1 |
| Kitchen Appliances   | [Mixer, Cooker]             |             20 |           4.20 |         90000 |           48.0 |

---

# Why this is useful for Recommendation Systems

This creates a **category popularity score**.

You can use it as a feature:

```
Product Recommendation Score =

Content Similarity
+
Rating Quality
+
Popularity Score
+
Customer Sentiment
```

A production recommendation engine would combine these signals instead of recommending only based on text similarity.
