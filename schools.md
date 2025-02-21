# PostgreSQL Beginner Guide: City, State, and School Database

## 1. Introduction
This guide will help you understand how to work with a simple PostgreSQL database containing three tables: `city`, `state`, and `school`. You will learn how to create tables, insert data, and run basic queries.

---
## 2. Database Schema
The database consists of three tables with the following structure:

### **City Table**
```sql
CREATE TABLE city (
    id SERIAL PRIMARY KEY,
    name VARCHAR(30) NOT NULL UNIQUE
);
```
- `id`: Auto-incremented primary key.
- `name`: Unique name of the city.

### **State Table**
```sql
CREATE TABLE state (
    id SERIAL PRIMARY KEY,
    city_id INT NOT NULL,
    name VARCHAR(30) NOT NULL,
    CONSTRAINT fk_state_city FOREIGN KEY (city_id) REFERENCES city(id) ON DELETE CASCADE
);
```
- `id`: Auto-incremented primary key.
- `city_id`: Foreign key referencing `city`.
- `name`: Name of the state.

### **School Table**
```sql
CREATE TABLE school (
    id SERIAL PRIMARY KEY,
    state_id INT NOT NULL,
    name VARCHAR(400) NOT NULL,
    CONSTRAINT fk_school_state FOREIGN KEY (state_id) REFERENCES state(id)
);
```
- `id`: Auto-incremented primary key.
- `state_id`: Foreign key referencing `state`.
- `name`: Name of the school.

---
## 3. Inserting Sample Data
You can insert sample data into the tables with the following queries:

```sql
INSERT INTO city (name) VALUES ('Edirne'), ('Istanbul');

INSERT INTO state (city_id, name) VALUES
    (1, 'Central'),
    (2, 'Kadikoy');

INSERT INTO school (state_id, name) VALUES
    (1, 'Edirne High School'),
    (2, 'Kadikoy College');
```

---
## 4. Basic Queries

### **Retrieve All Cities**
```sql
SELECT * FROM city;
```

### **Retrieve All States in a Specific City (e.g., Edirne - ID: 1)**
```sql
SELECT * FROM state WHERE city_id = 1;
```

### **Retrieve All Schools in a Specific State (e.g., State ID: 1)**
```sql
SELECT * FROM school WHERE state_id = 1;
```

### **Retrieve Schools in a Specific City (e.g., Edirne - ID: 1)**
```sql
SELECT sc.* FROM school sc
JOIN state s ON sc.state_id = s.id
JOIN city c ON s.city_id = c.id
WHERE c.id = 1;
```

### **Retrieve City, State, and School Names Together**
```sql
SELECT c.name AS city_name, s.name AS state_name, sc.name AS school_name
FROM school sc
JOIN state s ON sc.state_id = s.id
JOIN city c ON s.city_id = c.id;
```

---
## 5. Additional Tips
### **Check Existing Tables**
```sql
\dt
```

### **Describe Table Structure**
```sql
\d city
\d state
\d school
```

### **Convert Table & Column Names to Lowercase**
By default, PostgreSQL treats unquoted names as lowercase. To avoid quoting issues, ensure table and column names are lowercase when creating them:
```sql
CREATE TABLE city (
    id SERIAL PRIMARY KEY,
    name VARCHAR(30) NOT NULL UNIQUE
);
```


## 4. Advanced Queries

### 4.1 Join Query: Show City, State, and School
```sql
SELECT c.name AS city_name, s.name AS state_name, sch.name AS school_name
FROM school sch
JOIN state s ON sch.state_id = s.id
JOIN city c ON s.city_id = c.id;
```

### 4.2 Count the Number of Schools per State
```sql
SELECT s.name AS state_name, COUNT(sch.id) AS total_schools
FROM school sch
JOIN state s ON sch.state_id = s.id
GROUP BY s.name
ORDER BY total_schools DESC;
```

### 4.3 Find Cities with No Schools
```sql
SELECT c.name AS city_name
FROM city c
LEFT JOIN state s ON c.id = s.city_id
LEFT JOIN school sch ON s.id = sch.state_id
WHERE sch.id IS NULL;
```

### 4.4 Find the State with the Most Schools
```sql
SELECT s.name AS state_name, COUNT(sch.id) AS school_count
FROM school sch
JOIN state s ON sch.state_id = s.id
GROUP BY s.name
ORDER BY school_count DESC
LIMIT 1;
```

### 4.5 Retrieve Schools in a Specific City (e.g., Edirne)
```sql
SELECT sch.name AS school_name
FROM school sch
JOIN state s ON sch.state_id = s.id
JOIN city c ON s.city_id = c.id
WHERE c.name = 'Edirne';
```

---

## 5. Reporting Queries

### 5.1 Generate a Report of Schools Grouped by City and State
```sql
SELECT c.name AS city_name, s.name AS state_name, COUNT(sch.id) AS total_schools
FROM school sch
JOIN state s ON sch.state_id = s.id
JOIN city c ON s.city_id = c.id
GROUP BY c.name, s.name
ORDER BY c.name, total_schools DESC;
```

### 5.2 Find Cities with More Than 5 Schools
```sql
SELECT c.name AS city_name, COUNT(sch.id) AS school_count
FROM school sch
JOIN state s ON sch.state_id = s.id
JOIN city c ON s.city_id = c.id
GROUP BY c.name
HAVING COUNT(sch.id) > 5;
```

### 5.3 Monthly School Registration Report
```sql
SELECT TO_CHAR(created_at, 'YYYY-MM') AS registration_month, COUNT(id) AS total_schools
FROM school
GROUP BY registration_month
ORDER BY registration_month;
```
(Note: Requires `created_at` column in the `school` table)

---

## 6. Conclusion
This guide provides essential SQL queries to interact with PostgreSQL databases using a structured approach. The advanced and reporting queries help generate useful insights from the data. Happy querying! 🚀
