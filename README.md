# SQL Challenge Readme
## Overview
This repository contains SQL queries for a fictional employee database. The queries retrieve specific information about employees, departments, and managers. The goal is to perform various analyses on the data.

## Database Schema
The database schema is created using the Quuick DBD.
 - Table structure is created with the data type and primary keys.
 - the relations between the tabbles is created.
 - export the data as postgreSQL file.
The database is created using pgAdmin4 and import the schema from the local machine.
Create the tables using the schema.

The database includes the following tables:

 * employees: Information about employees, including employee number, employee title, last name, first name, sex, birth date, hire date.
 * Departments: Information about departments, including department number and department name.
 * dept_emp: Employee-to-department assignments.
 * dept_manager: Department managers and their assignments.
 * Salaries: employees and their salaries.
 * titles: titles with title ID.


### Import data to the tabels
* Using pgAdmin4 select the table which you are going to import to and click on the import/expor data and select the path of the data and type of data.
* set the delimiters and other formating opotions along with the header on.
* Use psql if you are facing issues importing data.
    - open the terminal and access the database using the command 
        psql -h your_host -d your_database -U your_username
           (host: localhost, database: empoyee_db, username: postgres)
    - copy the file using the psql command 
        \COPY employees FROM 'C:/Users/vinay/ut/sql_challenge/data/employees.csv' WITH CSV HEADER;
           (change the path as for the reqirement)

    -Update the date to the 'ISO' format which is used by the postgres
        UPDATE employees
        SET 
            birth_date = TO_DATE(birth_date, 'MM/DD/YYYY'),
            hire_date = TO_DATE(hire_date, 'MM/DD/YYYY');
* Make sure the data is imported by using the SELECT * FROM command in the pgAdmin4
### SQL Queries
 ** List of aemployees:
 - Query: List of employee Number, last name, first name, sex, and salary of each employee.
 - SQL:
    SELECT e.emp_no, e.last_name, e.first_name, e.sex, s.salary
    FROM employees e
    JOIN salaries s ON e.emp_no = s.emp_no;
 
 ** Employees Hired in 1986:

 - Query: List the first name, last name, and hire date for the employees who were hired in 1986.
 - SQL: 
    SELECT first_name, last_name, hire_date
    FROM employees
    WHERE EXTRACT(YEAR FROM hire_date) = 1986;
 
 ** Managers and Departments:
 - Query: List the manager of each department along with their department number, department name, employee number, last name, and first name.
 - SQL:
    SELECT
        dm.dept_no AS department_number,
        d.dept_name AS department_name,
        dm.emp_no AS employee_number,
        e.last_name,
        e.first_name
    FROM
        dept_manager dm
    JOIN
        Departments d ON dm.dept_no = d.dept_no
    JOIN
        employees e ON dm.emp_no = e.emp_no;
    
 ** Employees and Departments:
 - Query: List the department number for each employee along with their employee number, last name, first name, and department name.
 - SQL: 
    SELECT
        de.emp_no AS employee_number,
        e.last_name,
        e.first_name,
        de.dept_no AS department_number,
        d.dept_name AS department_name
    FROM
        dept_emp de
    JOIN
        employees e ON de.emp_no = e.emp_no
    JOIN
        Departments d ON de.dept_no = d.dept_no;

 ** Specific Employee Information:

 - Query: List first name, last name, and sex of each employee whose first name is Hercules and whose last name begins with the letter B.
 - SQL: 
    SELECT first_name, last_name, sex
    FROM employees
    WHERE first_name = 'Hercules' AND last_name LIKE 'B%';

 **Employees in Sales Department:

 - Query: List each employee in the Sales department, including their employee number, last name, and first name.
 - SQL: 
    SELECT
        de.emp_no AS employee_number,
        e.last_name,
        e.first_name
    FROM
        dept_emp de
    JOIN
        employees e ON de.emp_no = e.emp_no
    JOIN
        Departments d ON de.dept_no = d.dept_no
    WHERE
        d.dept_name = 'Sales';

 ** Employees in Sales and Development Departments:
 - Query: List each employee in the Sales and Development departments, including their employee number, last name, first name, and department name.
 - SQL: 
    SELECT
        de.emp_no AS employee_number,
        e.last_name,
        e.first_name,
        d.dept_name AS department_name
    FROM
        dept_emp de
    JOIN
        employees e ON de.emp_no = e.emp_no
    JOIN
        Departments d ON de.dept_no = d.dept_no
    WHERE
        d.dept_name IN ('Sales', 'Development');

 ** Last Name Frequency Counts:
 - Query: List the frequency counts, in descending order, of all the employee last names (how many employees share each last name).
 -SQL:
    SELECT
        last_name,
        COUNT(*) AS frequency
    FROM
        employees
    GROUP BY
        last_name
    ORDER BY
        frequency DESC;
    
## How to Run Queries
* Ensure you have access to a PostgreSQL database.
* Use a SQL client (e.g., pgAdmin, psql) to connect to the database.
* Open and run the queries from the provided SQL files in the queries directory.
