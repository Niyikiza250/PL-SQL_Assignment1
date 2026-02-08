# SQL Library System Project

**Author:** Eric Niyikiza  
**Student ID:** 24666  
**Course:** PL/SQL - Group D

## 📚 Overview

This project demonstrates SQL fundamentals and advanced analytics using a library management system. It showcases the power of SQL window functions for data analysis beyond traditional SELECT queries.

### Problem Statement

Managing library data requires more than just storing information—it demands analytical insights:
- Who are the top borrowers?
- How do borrowing trends change over time?
- How can we compare borrowing activity across different users or periods?
- How can we segment users into meaningful groups for reporting?

Traditional SQL queries provide basic answers, but **SQL window functions** unlock advanced analytical capabilities for ranking, aggregation, navigation, and distribution analysis.

---

## 🎯 Success Criteria

✅ **Database Schema Setup**
- Tables (Users, BookTable, Borrowers) with proper primary and foreign keys
- Sample data: 15 users, 15 books, 10+ borrow records

✅ **Joins Implemented**
- INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL OUTER JOIN
- Correctly combines data across tables

✅ **Window Functions Demonstrated**
- **Ranking Functions:** ROW_NUMBER, RANK, DENSE_RANK, PERCENT_RANK
- **Aggregate Functions:** SUM, AVG, MIN, MAX with ROWS and RANGE
- **Navigation Functions:** LAG, LEAD
- **Distribution Functions:** NTILE(4), CUME_DIST

✅ **Practical Insights**
- Each query includes explanation and use case
- Results demonstrate actionable analytics

---

## 🗄️ Database Schema

### Tables

#### 1. Users Table
Stores library user information.

#### 2. BookTable
```sql
CREATE TABLE BookTable (
    BookID INT PRIMARY KEY,
    BookName VARCHAR2(100) NOT NULL,
    BookAuthor VARCHAR2(100)
);
```

#### 3. Borrowers Table
Tracks book borrowing records with foreign keys to Users and BookTable.

---

## 📖 Sample Data

### Books Inserted (15 total)

```sql
-- Core Computer Science Books
INSERT INTO BookTable VALUES (1, 'Database Systems', 'C.J. Date');
INSERT INTO BookTable VALUES (2, 'Clean Code', 'Robert C. Martin');
INSERT INTO BookTable VALUES (3, 'Introduction to Algorithms', 'Thomas H. Cormen');
INSERT INTO BookTable VALUES (4, 'Design Patterns', 'Erich Gamma');
INSERT INTO BookTable VALUES (5, 'Effective Java', 'Joshua Bloch');

-- AI & Advanced Topics
INSERT INTO BookTable VALUES (6, 'Artificial Intelligence: A Modern Approach', 'Stuart Russell');
INSERT INTO BookTable VALUES (7, 'Operating System Concepts', 'Abraham Silberschatz');
INSERT INTO BookTable VALUES (8, 'Computer Networks', 'Andrew S. Tanenbaum');
INSERT INTO BookTable VALUES (9, 'Programming in C', 'Brian W. Kernighan');
INSERT INTO BookTable VALUES (10, 'The Pragmatic Programmer', 'Andrew Hunt');

-- Additional Technical Books
INSERT INTO BookTable VALUES (11, 'Computer Organization and Design', 'David A. Patterson');
INSERT INTO BookTable VALUES (12, 'Modern Operating Systems', 'Andrew S. Tanenbaum');
INSERT INTO BookTable VALUES (13, 'Compilers: Principles, Techniques, and Tools', 'Alfred V. Aho');
INSERT INTO BookTable VALUES (14, 'Data Mining: Concepts and Techniques', 'Jiawei Han');
INSERT INTO BookTable VALUES (15, 'Machine Learning', 'Tom M. Mitchell');
```

---

## 🔗 SQL Joins Examples

### INNER JOIN
Returns only matching records from all tables.

```sql
SELECT b.BorrowID, u.Username, bk.BookName
FROM Borrowers b
INNER JOIN Users u ON b.UserID = u.UserID
INNER JOIN BookTable bk ON b.BookID = bk.BookID;
```

**Use Case:** Show all active borrow records with user and book details.

#### Results:
![INNER JOIN Results](screenshots/inner_join_results.png)

---

### LEFT JOIN
Returns all records from Borrowers, with matching Users and Books (or NULL).

```sql
SELECT b.BorrowID, u.Username, bk.BookName
FROM Borrowers b
LEFT JOIN Users u ON b.UserID = u.UserID
LEFT JOIN BookTable bk ON b.BookID = bk.BookID;
```

**Use Case:** Detect orphaned borrow records (invalid UserID or BookID).

#### Results:
![LEFT JOIN Results](screenshots/left_join_results.png)

---

### RIGHT JOIN
Returns all records from Users, with matching borrow records (or NULL).

```sql
SELECT b.BorrowID, u.Username, bk.BookName
FROM Borrowers b
RIGHT JOIN Users u ON b.UserID = u.UserID
JOIN BookTable bk ON b.BookID = bk.BookID;
```

**Use Case:** Find users who haven't borrowed any books.

#### Results:
![RIGHT JOIN Results](screenshots/right_join_results.png)

---

### FULL OUTER JOIN
Returns all records from all tables, with NULLs where no match exists.

```sql
SELECT b.BorrowID, u.Username, bk.BookName
FROM Borrowers b
FULL OUTER JOIN Users u ON b.UserID = u.UserID
FULL OUTER JOIN BookTable bk ON b.BookID = bk.BookID;
```

**Use Case:** Complete data audit showing all users, books, and borrows.

#### Results:
![FULL OUTER JOIN Results](screenshots/full_outer_join_results.png)

---

## 📊 Window Functions Analysis

### Comprehensive Borrowing Analytics

```sql
SELECT 
    u.Username,
    COUNT(b.BorrowID) AS BooksBorrowed,
    
    -- Rank users by number of books borrowed
    RANK() OVER (ORDER BY COUNT(b.BorrowID) DESC) AS BorrowRank,
    
    -- Running total of books borrowed across all users
    SUM(COUNT(b.BorrowID)) OVER () AS TotalBooksBorrowed,
    
    -- Average number of books borrowed across all users
    AVG(COUNT(b.BorrowID)) OVER () AS AvgBooksBorrowed,
    
    -- Divide users into 4 quartiles based on borrow count
    NTILE(4) OVER (ORDER BY COUNT(b.BorrowID) DESC) AS Quartile
FROM Borrowers b
JOIN Users u ON b.UserID = u.UserID
GROUP BY u.Username
ORDER BY BorrowRank;
```

### What This Query Reveals:

1. **BorrowRank** - Top borrowers (gamification, loyalty programs)
2. **TotalBooksBorrowed** - Overall library engagement
3. **AvgBooksBorrowed** - Benchmark individual performance
4. **Quartile** - User segmentation (Q1 = heavy users, Q4 = light users)

#### Results:
![Window Functions Results](screenshots/window_functions_results.png)

---

## 🎯 Key Window Function Types

### 1. Ranking Functions
- `ROW_NUMBER()` - Unique sequential number
- `RANK()` - Rank with gaps for ties
- `DENSE_RANK()` - Rank without gaps
- `PERCENT_RANK()` - Relative rank (0 to 1)

### 2. Aggregate Functions
- `SUM() OVER()` - Running totals
- `AVG() OVER()` - Moving averages
- `MIN()/MAX() OVER()` - Cumulative extremes

### 3. Navigation Functions
- `LAG()` - Access previous row
- `LEAD()` - Access next row
- **Use Case:** Compare current vs previous borrowing activity

### 4. Distribution Functions
- `NTILE(n)` - Divide into n equal buckets
- `CUME_DIST()` - Cumulative distribution value
- **Use Case:** Customer segmentation, percentile analysis

---

## 💡 Business Insights

This system enables:

✨ **User Segmentation** - Identify heavy vs light borrowers for targeted campaigns  
✨ **Trend Analysis** - Track borrowing patterns over time  
✨ **Performance Benchmarking** - Compare users against average behavior  
✨ **Data Quality Checks** - Find orphaned records using JOINs  
✨ **Reporting** - Generate ranked lists for management

---

## 🚀 Getting Started

### Prerequisites
- Oracle Database (or compatible SQL database)
- SQL Developer / SQL*Plus

### Setup Instructions

1. **Create Tables**
   ```sql
   -- Run table creation scripts for Users, BookTable, Borrowers
   ```

2. **Insert Sample Data**
   ```sql
   -- Insert 15 books (provided in project)
   -- Insert 15 users
   -- Insert 10+ borrow records
   ```

3. **Run Queries**
   ```sql
   -- Execute JOIN examples
   -- Execute window function examples
   ```

### 📸 Adding Screenshots

To display your query results in this README:

1. Create a `screenshots` folder in your repository
2. Take screenshots of your SQL query results
3. Name them according to the placeholders in this README:
   - `inner_join_results.png`
   - `left_join_results.png`
   - `right_join_results.png`
   - `full_outer_join_results.png`
   - `window_functions_results.png`
4. Place all screenshots in the `screenshots/` folder

The images will automatically appear in the appropriate sections!

---

## 📈 Future Enhancements

- Add date-based analytics (borrowing trends by month/year)
- Implement overdue book tracking
- Create stored procedures for common queries
- Add triggers for automated reporting
- Build views for dashboard integration

---

## 📝 Notes

This project demonstrates that SQL is not just for storage—it's a powerful analytical tool. Window functions transform basic queries into sophisticated business intelligence.

**Key Takeaway:** Modern SQL databases can perform complex analytics without external tools, making them ideal for embedded reporting and real-time insights.

---

## 📄 License

Academic Project - PL/SQL Course

---

**Questions?** Contact Eric Niyikiza - Student ID: 24666
