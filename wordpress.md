# WordPress SQL Queries Cheat Sheet

A comprehensive guide to SQL queries in WordPress, focusing on examples for interacting with the WordPress database, including WooCommerce-specific queries. This tutorial covers step-by-step queries for WordPress developers and database administrators.

## Table of Contents
1. [Introduction to the WordPress Database](#introduction-to-the-wordpress-database)
2. [Basic SQL Queries](#basic-sql-queries)
3. [Fetching Posts and Pages](#fetching-posts-and-pages)
4. [Working with Meta Data](#working-with-meta-data)
5. [WooCommerce Queries](#woocommerce-queries)
6. [Managing Users](#managing-users)
7. [Custom Queries for Optimization](#custom-queries-for-optimization)

---
<a name="introduction-to-the-wordpress-database"></a>
## 1. Introduction to the WordPress Database

The WordPress database uses MySQL or MariaDB and consists of several tables by default. Here's a list of commonly used tables:

- `wp_posts`: Stores posts, pages, and custom post types.
- `wp_postmeta`: Stores meta data for posts.
- `wp_users`: Stores user information.
- `wp_usermeta`: Stores meta data for users.
- `wp_terms`, `wp_term_taxonomy`, `wp_term_relationships`: Stores taxonomy data like categories and tags.
- `wp_options`: Stores site settings and options.
- `wp_woocommerce_order_items`: Stores WooCommerce order details (WooCommerce-specific).

To query WordPress effectively, familiarity with these tables is essential.

---
<a name="basic-sql-queries"></a>
## 2. Basic SQL Queries

### Fetch All Data from a Table
```sql
SELECT * FROM wp_posts;
```

### Count Total Rows in a Table
```sql
SELECT COUNT(*) AS total_posts FROM wp_posts;
```

### Select Specific Columns
```sql
SELECT ID, post_title, post_status FROM wp_posts;
```

---
<a name="fetching-posts-and-pages"></a>
## 3. Fetching Posts and Pages

### Fetch Published Posts
```sql
SELECT ID, post_title, post_date FROM wp_posts
WHERE post_type = 'post' AND post_status = 'publish';
```

### Fetch Pages
```sql
SELECT ID, post_title, post_date FROM wp_posts
WHERE post_type = 'page' AND post_status = 'publish';
```

### Search Posts by Keyword
```sql
SELECT ID, post_title FROM wp_posts
WHERE post_title LIKE '%keyword%';
```

### Fetch Posts Published in a Date Range
```sql
SELECT ID, post_title, post_date FROM wp_posts
WHERE post_date BETWEEN '2024-01-01' AND '2024-12-31';
```

---
<a name="working-with-meta-data"></a>
## 4. Working with Meta Data

### Fetch Posts with Specific Meta Key
```sql
SELECT p.ID, p.post_title, pm.meta_value AS custom_field
FROM wp_posts p
JOIN wp_postmeta pm ON p.ID = pm.post_id
WHERE pm.meta_key = 'custom_field_key';
```

### Fetch Posts Based on Meta Value
```sql
SELECT p.ID, p.post_title
FROM wp_posts p
JOIN wp_postmeta pm ON p.ID = pm.post_id
WHERE pm.meta_key = 'price' AND pm.meta_value > 50;
```

---
<a name="woocommerce-queries"></a>
## 5. WooCommerce Queries

### Fetch All WooCommerce Products
```sql
SELECT ID, post_title
FROM wp_posts
WHERE post_type = 'product' AND post_status = 'publish';
```

### Fetch Orders and Total Amount
```sql
SELECT o.ID AS order_id, o.post_date AS order_date, om.meta_value AS total_amount
FROM wp_posts o
JOIN wp_postmeta om ON o.ID = om.post_id
WHERE o.post_type = 'shop_order' AND om.meta_key = '_order_total';
```

### Fetch Products by Category
```sql
SELECT p.ID, p.post_title
FROM wp_posts p
JOIN wp_term_relationships tr ON p.ID = tr.object_id
JOIN wp_term_taxonomy tt ON tr.term_taxonomy_id = tt.term_taxonomy_id
JOIN wp_terms t ON tt.term_id = t.term_id
WHERE p.post_type = 'product' AND t.name = 'Category Name';
```

### Fetch Stock Quantity of Products
```sql
SELECT p.ID, p.post_title, pm.meta_value AS stock_quantity
FROM wp_posts p
JOIN wp_postmeta pm ON p.ID = pm.post_id
WHERE p.post_type = 'product' AND pm.meta_key = '_stock';
```

---
<a name="managing-users"></a>
## 6. Managing Users

### Fetch All Users
```sql
SELECT ID, user_login, user_email, user_registered
FROM wp_users;
```

### Fetch Users with Specific Role
```sql
SELECT u.ID, u.user_login
FROM wp_users u
JOIN wp_usermeta um ON u.ID = um.user_id
WHERE um.meta_key = 'wp_capabilities' AND um.meta_value LIKE '%administrator%';
```

### Fetch Users Registered in the Last Month
```sql
SELECT ID, user_login, user_email
FROM wp_users
WHERE user_registered >= DATE_SUB(CURDATE(), INTERVAL 1 MONTH);
```

---
<a name="custom-queries-for-optimization"></a>
## 7. Custom Queries for Optimization

### Delete Revisions to Save Space
```sql
DELETE FROM wp_posts
WHERE post_type = 'revision';
```

### Optimize Database
```sql
OPTIMIZE TABLE wp_posts, wp_postmeta, wp_users, wp_usermeta;
```

### Find Orphaned Meta Data
```sql
SELECT pm.*
FROM wp_postmeta pm
LEFT JOIN wp_posts p ON pm.post_id = p.ID
WHERE p.ID IS NULL;
```

### Delete Orphaned Meta Data
```sql
DELETE pm
FROM wp_postmeta pm
LEFT JOIN wp_posts p ON pm.post_id = p.ID
WHERE p.ID IS NULL;
```

---

This guide provides a starting point for working with SQL in WordPress. For advanced queries or specific needs, consult the [WordPress Codex](https://codex.wordpress.org/) or the [WooCommerce Documentation](https://woocommerce.com/documentation/).
