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

---
## 6. Conclusion
This guide introduces PostgreSQL basics, covering table creation, inserting data, and writing queries. You can extend it further by adding more relationships or queries!

Happy coding! 🚀

