## [Injection] - [Database Schema]

## General Information

* **Difficulty Level :** ⭐⭐⭐ / 6

* **Date of Resolution :** 26-03-2026

---

## Vulnerability Analysis

* **Vulnerability Name :** SQL Injection

* **Affected Components :** Product Search (`/rest/products/search`)

* **Severity :** <span style="color:red"> High </span>

---

## Proof of Concept (PoC)

#### 1. Enumeration & Techniques

* **Techniques used :** SQL Injection, Database Enumeration, Union-Based Injection
* **Tools used :** Web Browser

#### 2. Reproduction Steps

**Step 1 : Initial Observation**

In the application, when `f12` is pressed, you can access the main.js file in the Sources. You can find the information that there is a path to see products informations (`https://ctf.juice.cyber.epitest.eu/rest/products/search?q=`). In this file, you can find informations about any products, and it comes with a search feature.

**Step 2 : UNION Injection**

To use a UNION-based injection, the number of columns (9 here) in the query must match the injected query. This is necessary to avoid SQL errors and ensure that the injected auery is processed correctly.

A UNION-based payload is constructed to extract schema information from the database:

```
https://ctf.juice.cyber.epitest.eu/rest/products/search?q=%27))UNION%20SELECT%20sql,2,3,4,5,6,7,8,9%20FROM%20sqlite_master--
```

This payload injects a secondary query into the original one.

**Step 3 : Explanation**

- `%27` → `'` closes the original string
- `))` → closes remaining query structure
- `UNION SELECT` → merges attacker-controlled query with original query
- `sql` → retrieves SQL schema definitions
- `sqlite_master` → internal SQLite table storing database structure
- `2,3,4,...` → filler values to match the expected number of columns
- `--` → comments out the rest of the query

**Step 4 : Results**

Once the payload is executed, the application returns SQL statements that now reveals table names, column names, database structure. 

---

## Risk Assessment

* **Business Impact :** Exposure of database structure allows attackers to identify sensitive tables and fields (e.g., users, passwords). This significantly increases the risk of further exploitation and full data compromise.

* **Technical Description of the Impact :** The application is vulnerable to SQL injection due to improper input handling. By leveraging UNION-based queries, an attacker can extract internal database metadata, revealing the entire schema.

---

## Actions & Remediation

#### Mitigation Strategies

   - Use parameterized queries (prepared statements)

   - Sanitize and validate all user inputs
#### Recommended Fixes

   - Avoid dynamic SQL query with user input
   
   - Implement proper error handling (avoid leaking DB info)

   - Limit database permissions

#### Security Best Practices

  - A03:2021-Injection -> Prevent SQL injection

  - A05:2021-Security Misconfiguration -> Secure database access
  
  - A02:2021-Cryptographic Failures-> Protect sensitive data storage
