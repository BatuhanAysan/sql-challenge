# Employee Database Analysis

## 📌 Project Description
This project involves modeling an employee database, processing data in PostgreSQL, and performing SQL-based data analysis.  
The project consists of three main stages:  
1️⃣ **Data Modeling** (Designing the database schema)  
2️⃣ **Data Engineering** (Importing data and setting up the database)  
3️⃣ **Data Analysis** (Running queries to extract insights)  

---

## 📌 Technologies Used
- **QuickDBD** → To design the database schema  
- **PostgreSQL** → To manage the database  
- **pgAdmin** → To handle the database operations  
- **SQL** → To query and analyze the data  

---

## 📌 Step-by-Step Process

### **1️⃣ Data Modeling**
- Used **QuickDBD** to design a **database schema** with the following tables:
  - `departments`
  - `employees`
  - `salaries`
  - `titles`
  - `dept_emp`
  - `dept_manager`
- Defined **Primary Keys (PKs) and Foreign Keys (FKs)**.  
- Assigned appropriate data types (`VARCHAR`, `INT`, `DATE`).  
- Exported the schema as a **PostgreSQL-compatible SQL script**.  

### **2️⃣ Data Engineering**
- Created the database and tables in **pgAdmin** using the exported SQL script.  
- **Imported** CSV files into the respective tables via **pgAdmin’s Import Tool** (not using `COPY`).  
- Verified data integrity by running `SELECT * FROM` queries.

```sql
SELECT * FROM departments;
SELECT * FROM dept_emp;
SELECT * FROM dept_manager;
SELECT * FROM employees;
SELECT * FROM salaries;
SELECT * FROM titles; 
```
### **3️⃣ Data Analysis**
The following SQL queries were executed to analyze the employee database.

#### **Employee Data Queries**
📌 **List employee number, last name, first name, gender, and salary**
```sql
SELECT employees.emp_no, employees.last_name, employees.first_name, employees.sex, salaries.salary
FROM employees
INNER JOIN salaries 
ON salaries.emp_no = employees.emp_no;
```
📌 **List first name, last name, and hire date of employees hired in 1986**
```sql
SELECT first_name, last_name, hire_date
FROM employees
WHERE DATE_PART('year', hire_date) = 1986;
```
#### **Department & Manager Analysis**
📌 **List each department’s manager along with department number, department name, employee number, last name, and first name**
```sql
SELECT dm.dept_no, d.dept_name, e.emp_no, e.last_name, e.first_name
FROM dept_manager AS dm
JOIN employees AS e
ON dm.emp_no = e.emp_no
JOIN departments AS d
ON dm.dept_no = d.dept_no;
```
📌 **List each employee’s department number, employee number, last name, first name, and department name**
```sql
SELECT de.dept_no, de.emp_no, e.last_name, e.first_name, d.dept_name
FROM dept_emp AS de
JOIN employees AS e
ON de.emp_no = e.emp_no
JOIN departments AS d
ON de.dept_no = d.dept_no;
```
#### **Filtering Specific Employees**
📌 **List first name, last name, and gender of employees named "Hercules" whose last name starts with "B"**
```sql
SELECT first_name, last_name, sex
FROM employees
WHERE first_name = 'Hercules' AND last_name LIKE 'B%';
```
📌 **List employees in the Sales department (department ID: d007)**
```sql
SELECT emp_no , first_name, last_name
FROM employees
WHERE emp_no IN
(
	SELECT emp_no
	FROM dept_emp
	WHERE dept_no = 'd007'
);
```
**📌 List employees in both Sales (d007) and Development (d005) departments**
```sql
SELECT de.emp_no, e.last_name, e.first_name, d.dept_name
FROM dept_emp AS de
JOIN employees AS e
ON de.emp_no = e.emp_no
JOIN departments AS d
ON de.dept_no = d.dept_no
WHERE de.dept_no = 'd007' OR de.dept_no = 'd005';
```
### **Last Name Frequency Analysis**
📌 **Count how many employees share the same last name, sorted in descending order**
```sql
SELECT last_name, COUNT(last_name) AS "Frequency"
FROM employees
GROUP BY last_name
ORDER BY "Frequency" DESC;
```
---

## 📌 **Key Learnings & Insights**
Through this project: 
✅ Learned how to **design a relational database schema using QuickDBD.**
✅ Gained experience in defining **Foreign Key and Primary Key relationships.**
✅ Successfully **imported CSV data into PostgreSQL using pgAdmin’s Import Tool.**
✅ Performed **data analysis** using SQL queries.
