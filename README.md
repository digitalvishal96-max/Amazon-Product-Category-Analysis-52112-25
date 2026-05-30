# Amazon Product Review & Pricing Analysis

## Project Overview

This project analyzes over 1,000 Amazon products to understand the relationship between pricing, discounts, customer ratings, and review behavior. The dataset combines product information, pricing details, customer feedback, and review text, enabling both exploratory data analysis (EDA) and Natural Language Processing (NLP) applications.

The primary objective is to identify how discount strategies, product popularity, and customer sentiment influence consumer perception and potential sales performance.

---

## Business Problem

Online marketplaces rely heavily on pricing strategies and customer reviews to drive sales. This project investigates:

* How discounts affect customer engagement.
* Whether highly rated products also receive positive textual reviews.
* Which product categories generate the highest customer interaction.
* How review volume can be used as a proxy for product success.

---

## Dataset Summary

### Dataset Size

* 1,000+ Amazon products
* 16 Features

### Feature Categories

| Category            | Features                                            |
| ------------------- | --------------------------------------------------- |
| Product Information | product_id, product_name, category, about_product   |
| Pricing             | discounted_price, actual_price, discount_percentage |
| Customer Ratings    | rating, rating_count                                |
| User Information    | user_id, user_name                                  |
| Reviews             | review_id, review_title, review_content             |
| Links               | product_link, img_link                              |

---

## Key Concepts

### Discounted Price vs Actual Price

* Actual Price represents the original MRP.
* Discounted Price represents the current selling price.

### Discount Percentage

* Measures the depth of the discount.
* Often influences purchasing decisions and customer engagement.

### Rating Count

* Total number of customer ratings.
* Used as a proxy indicator for product popularity and potential sales volume.

### Customer Sentiment

* Derived from review titles and review content.
* Helps evaluate customer satisfaction beyond numerical ratings.

---

## Project Roadmap

### 1. Data Cleaning

* Remove currency symbols (₹).
* Remove comma separators from price columns.
* Convert price, rating, and rating count fields into numeric formats.
* Handle missing and inconsistent values.

### 2. Feature Engineering

* Extract primary product categories.
* Create discount-related metrics.
* Generate sentiment scores from review text.

### 3. Exploratory Data Analysis (EDA)

* Analyze pricing distributions.
* Study rating and review count patterns.
* Compare category-level performance.
* Investigate discount effectiveness.

### 4. Sentiment Analysis

* Process review text using NLP techniques.
* Compare sentiment scores against numerical ratings.
* Identify products with mismatched sentiment and ratings.

---

## Technologies Used

### Data Processing

* Pandas
* NumPy

### Visualization

* Matplotlib
* Seaborn

### Natural Language Processing

* NLTK
* TextBlob

### Machine Learning (Optional Extension)

* Scikit-learn

---

## Data Quality Challenges

### Currency Formatting

Price columns contain Indian Rupee symbols and commas that require preprocessing before analysis.

### Missing Values

Some products contain missing rating counts and review information.

### Data Type Issues

Certain rating values may be stored as text, causing failures during statistical calculations and correlation analysis.

### Category Complexity

Category fields contain hierarchical values that must be parsed before category-level analysis.

---

## Key Insights

### Price Elasticity

Higher discounts do not necessarily result in better ratings. Moderate discounts on quality products often generate stronger customer satisfaction.

### Importance of Review Volume

Products with thousands of reviews and strong ratings are generally more trustworthy than products with perfect ratings but limited feedback.

### Category Performance

Electronics and Home & Kitchen categories dominate the dataset and provide the most reliable patterns for analysis and predictive modeling.

### Customer Sentiment

Textual reviews provide additional context that numerical ratings alone cannot capture, making sentiment analysis a valuable complement to traditional rating metrics.

---

## Future Enhancements

* Product recommendation system
* Sales prediction modeling
* Sentiment-based product ranking
* Review summarization using NLP
* Customer satisfaction prediction
* Price optimization analysis

---

## Project Outcome

This project demonstrates practical skills in:

* Data Cleaning
* Exploratory Data Analysis (EDA)
* Feature Engineering
* Sentiment Analysis
* Data Visualization
* Business Insight Generation
* End-to-End Data Analytics Workflow

The analysis provides actionable insights into how pricing strategies, customer reviews, and product categories influence product performance on e-commerce platforms.
