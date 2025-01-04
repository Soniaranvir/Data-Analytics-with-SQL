# Data-Analytics-with-SQL
---

# <a href = https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.apple.com%2Fca%2Fapp-store%2F&psig=AOvVaw3PrWe8gizGgs65QzJ9yvUh&ust=1736047162453000&source=images&cd=vfe&opi=89978449&ved=0CBQQjRxqFwoTCOimoOWN24oDFQAAAAAdAAAAABAE![image](https://github.com/user-attachments/assets/08dbe5b8-e0ac-4604-9670-ff93398ecf9c) </a>

Apple Store Data Analysis Project  

## Scenario

An aspiring app developer seeks insights into what kind of app to build. This analysis provides data-driven recommendations for making informed decisions.

### 🚀 **Overview**  
This project focuses on analyzing Apple Store data to uncover trends, insights, and characteristics that contribute to app success. By leveraging SQL, we explore key aspects like user ratings, app genres, pricing models, and app descriptions.  

---

## 🎯 **Project Goals**  
- **Understand App Performance:** Identify factors affecting app ratings and popularity.  
- **Improve Developer Strategies:** Provide actionable insights for app developers.  
- **Data-Driven Decision Making:** Explore the relationship between pricing, language support, and app success.  

---

## 🛠️ **What We Did**  
### 1️⃣ **Data Integration**  
Combined four datasets (`appleStore_description1` to `appleStore_description4`) into a single table (`appleStore_description_combined`) using SQL for seamless analysis.  

### 2️⃣ **Exploratory Data Analysis (EDA)**  
- ✅ Ensured data integrity by counting unique app IDs across datasets.  
- 🔍 Checked for missing values in critical fields such as:  
  - `track_name`  
  - `user_rating`  
  - `prime_genre`  
- 📊 Analyzed app distribution by genre to identify the most and least popular categories.  
- 📈 Summarized user ratings with metrics like minimum, maximum, and average.  

### 3️⃣ **Advanced Data Analysis**  
- 💸 **Paid vs. Free Apps:** Compared user ratings to determine if paid apps outperform free ones.  
- 🌍 **Language Support:** Explored whether apps supporting more languages receive better ratings.  
- 📉 **Low-Rated Genres:** Highlighted categories with the lowest average ratings.  
- ✍️ **Description Length:** Investigated correlations between app description length and user ratings.  
- 🏆 **Top-Rated Apps by Genre:** Used SQL window functions and ranking to identify the best-rated apps in each category.  

---

## 🎯 **Why Was It Done?**  
This project was conducted to provide insights for app developers and businesses, helping them:  
- Build better products by understanding factors affecting ratings.  
- Target the right audience through pricing and language strategies.  
- Identify high-performing genres and areas needing improvement.  

---

## 📁 **About the Files**  
Four files were uploaded to the repository:  
1. `appleStore_description1`  
2. `appleStore_description2`  
3. `appleStore_description3`  
4. `appleStore_description4`  

These files were combined to ensure comprehensive analysis of app descriptions, creating the `appleStore_description_combined` table.  

---

## 📝 **SQL Code Highlights**

### 📂 **Data Integration**  
```sql
CREATE TABLE appleStore_description_combined AS
SELECT * FROM appleStore_description1
UNION ALL
SELECT * FROM appleStore_description2
UNION ALL
SELECT * FROM appleStore_description3
UNION ALL
SELECT * FROM appleStore_description4;
```

### 🔍 **EDA**  
- Count unique apps:  
  ```sql
  SELECT COUNT(DISTINCT id) AS UniqueAppIds 
  FROM AppleStore;

  SELECT COUNT(DISTINCT id) AS UniqueAppIds 
  FROM appleStore_description_combined;
  ```

- Check for missing values:  
  ```sql
  SELECT COUNT(*) AS MissingValues
  FROM AppleStore
  WHERE track_name IS NULL 
     OR user_rating IS NULL 
     OR prime_genre IS NULL;

  SELECT COUNT(*) AS MissingValues
  FROM appleStore_description_combined
  WHERE app_desc IS NULL;
  ```

- App distribution by genre:  
  ```sql
  SELECT prime_genre, COUNT(*) AS NumberOfApps 
  FROM AppleStore 
  GROUP BY prime_genre 
  ORDER BY NumberOfApps DESC;
  ```

### 📊 **Data Analysis**  
- Paid vs. Free apps:  
  ```sql
  SELECT 
    CASE 
      WHEN Price > 0 THEN 'Paid' 
      ELSE 'Free' 
    END AS AppType,
    AVG(user_rating) AS AverageRating 
  FROM AppleStore 
  GROUP BY AppType;
  ```

- Ratings by language support:  
  ```sql
  SELECT 
    CASE 
      WHEN lang_num < 10 THEN 'Less than 10 Languages'
      WHEN lang_num BETWEEN 10 AND 30 THEN '10-30 Languages'
      ELSE 'More than 30 Languages' 
    END AS LanguageBucket,
    AVG(user_rating) AS AverageRating
  FROM AppleStore 
  GROUP BY LanguageBucket 
  ORDER BY AverageRating DESC;
  ```

- Genres with low ratings:  
  ```sql
  SELECT prime_genre, AVG(user_rating) AS AverageRating 
  FROM AppleStore 
  GROUP BY prime_genre
  ORDER BY AverageRating ASC
  LIMIT 10;
  ```

- Description length and ratings:  
  ```sql
  SELECT 
    CASE 
      WHEN LENGTH(b.app_desc) < 500 THEN 'Short'
      WHEN LENGTH(b.app_desc) BETWEEN 500 AND 1000 THEN 'Medium'
      ELSE 'Long' 
    END AS DescriptionLengthBucket,
    AVG(a.user_rating) AS AverageRating
  FROM AppleStore AS a
  JOIN appleStore_description_combined AS b
  ON a.ID = b.ID
  GROUP BY DescriptionLengthBucket
  ORDER BY AverageRating DESC;
  ```

- Top-rated apps by genre:  
  ```sql
  SELECT prime_genre, track_name, user_rating, 
         RANK() OVER (PARTITION BY prime_genre ORDER BY user_rating DESC) AS Rank
  FROM AppleStore;
  ```

---

## 📈 **Key Insights**  
- **Paid apps** tend to have slightly higher ratings than free apps.  
- Apps supporting **more languages** generally receive better user ratings.  
- The most popular genres include **Games** and **Entertainment**, while some niche genres underperform.  
- **Longer app descriptions** are associated with higher user ratings, highlighting the importance of detailed app descriptions.  

---

## 🛡️ **Takeaways for Developers**  
- Offering apps in multiple languages can enhance their appeal to a global audience.  
- Investing in high-quality app descriptions can positively impact user perception.  
- Analyzing underperforming genres can reveal untapped opportunities for growth.  
