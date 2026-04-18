## 📊 Project Overview

This project explores the relationship between **movie discussions and commercial success**. We use data from Reddit, IMDb, and Box Office Mojo to understand whether the movies people talk about online are also financially successful.

Our main goal is to answer the question:

> **How does movie discussion relate to commercial success?** 

We analyze this by combining different datasets and applying SQL, MongoDB, and Python-based analysis.

---

## ❓ Research Questions

The project is structured around three key research questions:

1. How does Reddit recommendation frequency relate to the proportion of international vs domestic box office revenue?
2. How does IMDb rating relate to Reddit discussions and box office success?
3. How do Reddit upvotes vary with movie duration across genres? 


## 🗃️ Datasets Used

We combine three datasets:

### 1. IMDb Dataset

* 300,000+ movies
* Includes title, genre, year, ratings, and votes
* Acts as the **main dataset for joining all data** 

### 2. Reddit Dataset

* 420,000+ rows of movie discussions
* Extracted from subreddits like r/movies and r/moviesuggestions
* Used to measure **discussion frequency and engagement** 

### 3. Box Office Mojo Dataset

* ~4000 movies (2010–2018)
* Contains domestic and foreign revenue
* Used to measure **commercial success** 

---

## 🧹 Data Cleaning

We performed extensive cleaning to ensure consistency across datasets:

* Filtered movies (English, 2010–2018, selected genres)
* Standardized titles (lowercase, removed punctuation)
* Removed missing and duplicate values
* Extracted IMDb IDs from Reddit text
* Converted data types for accurate analysis 

---

## 🗄️ Database Design

We used both **SQL (Oracle)** and **MongoDB**:

### SQL

* Joined IMDb, Reddit, and Box Office datasets
* Used queries to compute recommendation counts and revenue shares

### MongoDB

* Stored data in a **nested document structure**
* Each movie contains:

  * Basic info (title, year, genre)
  * Embedded box office data
  * Embedded Reddit mentions

This reduces the need for joins and improves query efficiency 

---

## 📈 Methodology

We used:

* SQL queries for data extraction
* Python (pandas, matplotlib) for analysis and visualization
* Correlation analysis (Pearson)
* Regression models to study relationships between variables

For example, we analyzed how Reddit recommendations relate to **international revenue share** using scatterplots and regression models 

---

## 📊 Outputs

The project generates:

* Cleaned datasets (CSV files)
* SQL query results
* Visualizations (scatter plots, charts)
* Statistical analysis results

## 📄 Reports

You can find detailed explanations of the project in the following reports:

* **"Data cleaning and Oracle.pdf"**
* **"Formal Report.pdf"**
* **"MongoDB Report.pdf"**

These reports include full methodology, queries, analysis, and results.

---

## ⚠️ Limitations

* Data is limited to movies from **2010–2018**
* Only English movies and selected genres are considered
* Reddit data reflects discussions from **2022 only**
* Possible bias from Reddit users and missing data

These limitations may affect how generalizable the results are 

---

## 💡 Key Insight

This project helps answer an important question:

👉 *Do popular discussions reflect actual success, or are they driven by different audience preferences?*
