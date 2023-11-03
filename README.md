# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
1. Query Proficiency: Develop a strong foundation in writing SQL queries, including SELECT, INSERT, UPDATE, DELETE, and JOIN statements.
2. Data Retrieval and Analysis: Gain proficiency in retrieving specific data from databases, performing data filtering, aggregation, and basic analysis.
3. Database Design and Modeling: Understand the principles of database design, including normalization, entity-relationship diagrams (ERDs), and creating well-structured databases.

## Process
1. Set Up and Initialization Action: Clone the project repository and import the SQL database. 

2. Write an initial README.md with a brief project overview. 

3. Data Inspection and Cleaning (cleaning_data.md)

4. Answer Predefined Questions (starting_with_questions.md)

5. Generate New Questions (starting_with_data.md)

6. Quality Assurance (QA.md) Action: Identify risk areas and outline a QA process.

## Results
(fill in what you discovered this data could tell you and how you used the data to answer those questions)

1. How many visits to the site actually translated to purchases or placing orders for products (i.e. percentage of visitors to the site that actually makes a purchase)
2. The total number of unique visitors by referring sites

## Challenges 
(discuss challenges you faced in the project)
1. While loading the csv files into the database ecommerce, I was able to identify primary key and load the tables using the different data types for the sales_report, sales_by_sku and products CSV files. I had difficulty with loading the remaining 2 CSV files (all_sessions and analytics) and couldn't identify a primary key at the time of loading the CSV into the database. Those two tables would need substantial data cleaning.
2. Data cleaning took a lot of effort and was almost 85% of my time.
3. I dropped some null columns on one of the tables and when I tried to view the table - got an error (could not find the relation table). Tried to troubleshoot and eventually had to delete table from the database and re-create it.
4. Didn;t have enough time to complete the project due to multiple pressing demands at work and at home.

## Future Goals
(what would you do if you had more time?)
Done more Data cleaning and transformations.
More Data Analysis.
USe more advanced SQL query to achieve some of the task (fewer lines of code)
