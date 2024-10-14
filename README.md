# Employee Database SQL Analysis

## Project Overview
It’s been two weeks since I was hired as a new data engineer at Pewlett Hackard, a fictional company. My first major task is a research project focused on employees who worked for the company during the 1980s and 1990s. The employee database from that period was lost, and all that remains are six CSV files containing partial information. My goal is to rebuild the database by designing the tables, importing the CSV data into a SQL database, and answering key questions about the data through analysis.

## Data Files
The following CSV files contain the employee data:
 - employees.csv: Employee information including their employee number, name, gender, and hire date.
 - salaries.csv: Salary information for employees.
 - departments.csv: List of departments in the company.
 - dept_emp.csv: Which employees belong to which departments.
 - dept_manager.csv: Information about department managers.
 - titles.csv: Job titles for employees.

## Database Schema
The database schema is structured as follows:
 1. employees: Contains employee details.
 2. salaries: Stores employee salaries.
 3. departments: Lists all company departments.
 4. dept_emp: Tracks which employees belong to which departments.
 5. dept_manager: Tracks which employees manage which departments.
 6. titles: Lists all job titles.

## Table Schemas
1. employees:
   CREATE TABLE employees (
    emp_no INT PRIMARY KEY,
    emp_title_id VARCHAR(10),
    birth_date DATE,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    sex CHAR(1),
    hire_date DATE
);
2. salaries:
   CREATE TABLE salaries (
    emp_no INT,
    salary INT,
    PRIMARY KEY (emp_no, salary),
    FOREIGN KEY (emp_no) REFERENCES employees(emp_no)
);
3. departments:
   CREATE TABLE departments (
    dept_no VARCHAR(10) PRIMARY KEY,
    dept_name VARCHAR(50)
);
4. dept_emp
   CREATE TABLE dept_emp (
    emp_no INT,
    dept_no VARCHAR(10),
    PRIMARY KEY (emp_no, dept_no),
    FOREIGN KEY (emp_no) REFERENCES employees(emp_no),
    FOREIGN KEY (dept_no) REFERENCES departments(dept_no)
);
5. dept_manager:
   CREATE TABLE dept_manager (
    dept_no VARCHAR(10),
    emp_no INT,
    PRIMARY KEY (dept_no, emp_no),
    FOREIGN KEY (dept_no) REFERENCES departments(dept_no),
    FOREIGN KEY (emp_no) REFERENCES employees(emp_no)
);
6. titles:
   CREATE TABLE titles (
    title_id VARCHAR(10) PRIMARY KEY,
    title VARCHAR(50)
);

## Data Analysis
Once the tables were created and populated, the following queries were run to analyze the data:

1. List employee number, last name, first name, sex, and salary of each employee:

SELECT employees.emp_no, employees.last_name, employees.first_name, employees.sex, salaries.salary
FROM employees
JOIN salaries ON employees.emp_no = salaries.emp_no;

2. List first name, last name, and hire date for employees hired in 1986:

SELECT first_name, last_name, hire_date
FROM employees
WHERE EXTRACT(YEAR FROM hire_date) = 1986;

3. List department managers along with their department and employee details:

SELECT dept_manager.dept_no, departments.dept_name, employees.emp_no, employees.last_name, employees.first_name
FROM dept_manager
JOIN employees ON dept_manager.emp_no = employees.emp_no
JOIN departments ON dept_manager.dept_no = departments.dept_no;

4. List department number, employee number, last name, first name, and department name for each employee:

SELECT dept_emp.dept_no, employees.emp_no, employees.last_name, employees.first_name, departments.dept_name
FROM dept_emp
JOIN employees ON dept_emp.emp_no = employees.emp_no
JOIN departments ON dept_emp.dept_no = departments.dept_no;

5. List employees named Hercules whose last name starts with B:

SELECT first_name, last_name, sex
FROM employees
WHERE first_name = 'Hercules' AND last_name LIKE 'B%';

6. List employees in the Sales department:

SELECT employees.emp_no, employees.last_name, employees.first_name
FROM dept_emp
JOIN employees ON dept_emp.emp_no = employees.emp_no
JOIN departments ON dept_emp.dept_no = departments.dept_no
WHERE departments.dept_name = 'Sales';

7. List employees in the Sales and Development departments:

SELECT employees.emp_no, employees.last_name, employees.first_name, departments.dept_name
FROM dept_emp
JOIN employees ON dept_emp.emp_no = employees.emp_no
JOIN departments ON dept_emp.dept_no = departments.dept_no
WHERE departments.dept_name IN ('Sales', 'Development');

8. List the frequency counts of employee last names:

SELECT last_name, COUNT(last_name) AS name_count
FROM employees
GROUP BY last_name
ORDER BY name_count DESC;

## Limitations:
While this project provides valuable insights into employee data, it has several limitations:

1. Limited Historical Data: The dataset only covers the 1980s and 1990s, which limits its applicability to current company trends.
2. Missing or Incomplete Data: There may be missing records, such as incomplete salary or department history for some employees.
3. Static Data: The data is a snapshot and does not track changes over time (e.g., promotions, salary changes, department transfers).
4. No Employee Attrition Data: The dataset does not track when employees left the company, limiting insights into employee turnover.
5. Lack of Performance Data: No data is available on employee performance or bonuses, preventing performance-based analysis.
6. Salary Information: Salaries reflect base pay only, without accounting for bonuses, stock options, or other compensation.
7. Limited Job Titles: The job title data lacks granularity, making it difficult to fully analyze career progression.
  
These limitations should be considered when interpreting the results of the analysis.

## How to Run the Project
### Requirements
- PostgreSQL installed and running
- pgAdmin 4 for database management
- The CSV files from the dataset (e.g., employees.csv, salaries.csv, etc.)

### Steps to Run
1. Create the Database:
  - Use pgAdmin 4 or psql to create a new PostgreSQL database (e.g., employee_db).
2. Create the Tables:
  - Execute the provided SQL schema to create the necessary tables in the database.
3. Import the CSV Files:
  - Import the CSV files into their corresponding tables using pgAdmin 4's Import/Export Data feature.
4. Run SQL Queries:
  - Use the provided SQL queries to analyze the data.

## Conclusion
This project provides insight into employee data for Pewlett Hackard, focusing on the 1980s and 1990s. By constructing a relational database, running SQL queries, and performing data analysis, we can extract meaningful insights from the employee information.

