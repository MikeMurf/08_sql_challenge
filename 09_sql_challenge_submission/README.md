## SQL_challenge_submission


This SQL Challenge Submission has been prepared by Mike Murphy.

The instructions for the SQL Challenge can be found at the following link: -
https://monash.bootcampcontent.com/monash-coding-bootcamp/monu-virt-data-11-2021-u-c/-/tree/master/02-Homework/09-SQL/Instructions


This submission comprises the following elements: -

1.	An image of the Entity Relationship Diagram for the Hewlett Packard “employeeSQL” database,
2.	An SQL file of the Table Schemata code exported from QuickDBD,
3.	A PostgreSQL file of the queries required for the SQL Challenge (including the Table Schemata code exported from QuickDBD,
4.	Screenshots of the SQL Queries and their Results, and,
5.	A Jupyter Notebook file of the Analysis required for the Bonus Part of the assignment. 

**1.	The Entity Relationship Diagram**

   ![QuickDBD-export_mm](https://user-images.githubusercontent.com/89948865/149834524-8bb7436a-95b3-48bb-ab75-46fe4f3a654e.png) 
      
**2.	The Table Schemata Code Exported from QuickDBD** 
 
 
-- Exported from QuickDBD: https://www.quickdatabasediagrams.com/
-- NOTE! If you have used non-SQL datatypes in your design, you will have to change these here.

    DROP TABLE departments CASCADE; 
    DROP TABLE employees CASCADE; 
    DROP TABLE dept_manager; 
    DROP TABLE dept_emps; 
    DROP TABLE titles;  
    DROP TABLE salaries; 

    CREATE TABLE "departments" ( 
        "dept_no" varchar   NOT NULL, 
        dept_name" varchar   NOT NULL, 
        CONSTRAINT "pk_departments" PRIMARY KEY ( 
            "dept_no" 
         ) 
    ); 


    CREATE TABLE "employees" (
        "emp_no" int   NOT NULL,
        "title_id" varchar   NOT NULL,
        "birth_date" date   NOT NULL,
        "first_name" varchar   NOT NULL,
        "last_name" varchar   NOT NULL,
        "gender" varchar   NOT NULL,
        "hire_date" date   NOT NULL,
        CONSTRAINT "pk_employees" PRIMARY KEY (
            "emp_no"
         )
    );


    CREATE TABLE "dept_manager" (
        "dept_no" varchar   NOT NULL,
        "emp_no" int   NOT NULL
    );

    CREATE TABLE "dept_emps" (
        "emp_no" int   NOT NULL,
        "dept_no" varchar   NOT NULL
    );

    CREATE TABLE "titles" (
       "title_id" varchar   NOT NULL,
       "title" varchar   NOT NULL,
        CONSTRAINT "pk_titles" PRIMARY KEY (
            "title_id")
    );

    CREATE TABLE "salaries" (
        "emp_no" int   NOT NULL,
        "salary" int   NOT NULL
    );

    ALTER TABLE "dept_manager" ADD CONSTRAINT "fk_dept_manager_dept_no" FOREIGN KEY("dept_no")
    REFERENCES "departments" ("dept_no");

    ALTER TABLE "dept_manager" ADD CONSTRAINT "fk_dept_manager_emp_no" FOREIGN KEY("emp_no")
    REFERENCES "employees" ("emp_no");

    ALTER TABLE "dept_emps" ADD CONSTRAINT "fk_dept_emps_emp_no" FOREIGN KEY("emp_no")
    REFERENCES "employees" ("emp_no");

    ALTER TABLE "dept_emps" ADD CONSTRAINT "fk_dept_emps_dept_no" FOREIGN KEY("dept_no")
    REFERENCES "departments" ("dept_no");

    TABLE "titles" ADD CONSTRAINT "fk_titles_title_id" FOREIGN KEY("title_id")
    REFERENCES "employees" ("title_id");

    ALTER TABLE "salaries" ADD CONSTRAINT "fk_salaries_emp_no" FOREIGN KEY("emp_no")
    REFERENCES "employees" ("emp_no");
 

**3.	The PostgreSQL File of the Queries Required for the SQL Challenge** 
        **Note that this includes the Table Schemata Code Exported from QuickDBD 
	  and shows the sequence in which the code was run, the csv files imported 
	  and the sequence of the queries** 
	  
 
    -- Exported from QuickDBD: https://www.quickdatabasediagrams.com/
    -- NOTE! If you have used non-SQL datatypes in your design, you will have to change these here.

    DROP TABLE departments CASCADE;
    DROP TABLE employees CASCADE;
    DROP TABLE dept_manager;
    DROP TABLE dept_emps;
    DROP TABLE titles;
    DROP TABLE salaries;

    CREATE TABLE "departments" (
        "dept_no" varchar   NOT NULL,
        "dept_name" varchar   NOT NULL,
        CONSTRAINT "pk_departments" PRIMARY KEY (
            "dept_no"
         )
    );

    CREATE TABLE "employees" (
        "emp_no" int   NOT NULL,
        "emp_title_id" varchar   NOT NULL,
        "birth_date" date   NOT NULL,
        "first_name" varchar   NOT NULL,
        "last_name" varchar   NOT NULL,
        "gender" varchar   NOT NULL,
        "hire_date" date   NOT NULL,
        CONSTRAINT "pk_employees" PRIMARY KEY (
            "emp_no"
         )
    );

    CREATE TABLE "dept_manager" (
        "dept_no" varchar   NOT NULL,
        "emp_no" int   NOT NULL
    );

    CREATE TABLE "dept_emps" (
        "emp_no" int   NOT NULL,
        "dept_no" varchar   NOT NULL
    );

    CREATE TABLE "titles" (
        "title_id" varchar   NOT NULL,
        "title" varchar   NOT NULL,
        CONSTRAINT "pk_titles" PRIMARY KEY (
            "title_id")
    );

    CREATE TABLE "salaries" (
        "emp_no" int   NOT NULL,
        "salary" int   NOT NULL
    );

    -- manually imported departments
    SELECT * FROM departments
    -- verified departments

    -- manually imported employees
    SELECT * FROM employees LIMIT 10
    -- verified employees

    -- manually imported dept_manager
    SELECT * FROM dept_manager LIMIT 10
    -- verified dept_manager

    -- manually imported dept_emps
    SELECT * FROM dept_emps LIMIT 10
    -- verified dept_emps

    -- manually imported titles
    SELECT * FROM titles LIMIT 10
    -- verified titles

    -- manually imported salaries
    SELECT * FROM salaries LIMIT 10
    -- verified salaries

    -- now create constraints
    ALTER TABLE "dept_manager" ADD CONSTRAINT "fk_dept_manager_dept_no" FOREIGN KEY("dept_no")
    REFERENCES "departments" ("dept_no");

    ALTER TABLE "dept_manager" ADD CONSTRAINT "fk_dept_manager_emp_no" FOREIGN KEY("emp_no")
    REFERENCES "employees" ("emp_no");

    ALTER TABLE "salaries" ADD CONSTRAINT "fk_salaries_emp_no" FOREIGN KEY("emp_no")
    REFERENCES "employees" ("emp_no");

    ALTER TABLE "employees" ADD CONSTRAINT "fk_employees_emp_title_id" FOREIGN KEY("emp_title_id")
    REFERENCES "titles" ("title_id");

    ALTER TABLE "dept_emps" ADD CONSTRAINT "fk_dept_emp_emp_no" FOREIGN KEY("emp_no")
    REFERENCES "employees" ("emp_no");

    ALTER TABLE "dept_emps" ADD CONSTRAINT "fk_dept_emp_dept_no" FOREIGN KEY("dept_no")
    REFERENCES "departments" ("dept_no");

    --1.   List the following details of each employee: employee number, last name, first name, sex, and salary.
    SELECT employees.emp_no, employees.last_name, employees.first_name, employees.gender, salaries.salary
    FROM employees
    JOIN salaries
    ON employees.emp_no = salaries.emp_no
    LIMIT (10);
    -- returns 300,024 rows consistent with original csv file

    --2.   List first name, last name, and hire date for employees who were hired in 1986.
    SELECT first_name, last_name, hire_date 
    FROM employees
    WHERE hire_date BETWEEN '1/1/1986' AND '12/31/1986'
    ORDER BY hire_date
    LIMIT (10);
    -- returns 36,150 rows

    --3.   List the manager of each department with the following information: department number, department name, the manager's employee number, last name, first name.
    SELECT departments.dept_no, departments.dept_name, dept_manager.emp_no, employees.last_name, employees.first_name
    FROM departments
    JOIN dept_manager
    ON departments.dept_no = dept_manager.dept_no
    JOIN employees
    ON dept_manager.emp_no = employees.emp_no
    LIMIT (10);
    -- returns 24 rows

    --4.   List the department of each employee with the following information: employee number, last name, first name, and department name.
    SELECT dept_emps.emp_no, employees.last_name, employees.first_name, departments.dept_name
    FROM dept_emps
    JOIN employees
    ON dept_emps.emp_no = employees.emp_no
    JOIN departments
    ON dept_emps.dept_no = departments.dept_no
    LIMIT (10);
    -- returns 331,603 rows

    --5.  List first name, last name, and sex for employees whose first name is "Hercules" and last names begin with "B."
    SELECT employees.first_name, employees.last_name, employees.gender
    FROM employees
    WHERE first_name = 'Hercules'
    AND last_name Like 'B%'
    LIMIT (10);
    -- returns 20 rows

    --6.   List all employees in the Sales department, including their employee number, last name, first name, and department name.
    SELECT departments.dept_name, employees.last_name, employees.first_name
    FROM dept_emps
    JOIN employees
    ON dept_emps.emp_no = employees.emp_no
    JOIN departments
    ON dept_emps.dept_no = departments.dept_no
    WHERE departments.dept_name = 'Sales'
    LIMIT (10);
    -- returns 53,245 rows

    --7.   List all employees in the Sales and Development departments, including their employee number, last name, first name, and department name.
    SELECT employees.emp_no, employees.last_name, employees.first_name, departments.dept_name
    FROM dept_emps
    JOIN employees
    ON dept_emps.emp_no = employees.emp_no
    JOIN departments
    ON dept_emps.dept_no = departments.dept_no
    WHERE departments.dept_name = 'Sales' 
    OR departments.dept_name = 'Development'
    LIMIT (10);
    -- returns 137,952 rows

    --8.   In descending order, list the frequency count of employee last names, i.e., how many employees share each last name.
    SELECT last_name,
    COUNT(last_name) AS "frequency"
    FROM employees
    GROUP BY last_name
    ORDER BY
    COUNT(last_name) DESC
    LIMIT (10);
    -- 1638 rows
    
    
**--------------------------END OF DATA ANALYSIS-------------------------------  

**-----------------------------START OF BONUS---------------------------------- 

**--------BONUS COMPLETED IN PANDAS - REFER ACCOMPANYING JUPYTER NOTEBOOK------ 

 
 
  
**##4.	The Screenshots of the SQL Queries and their Results** 


The screenshots of the SQL Queries and their Results required for the SQL Challenge follow. 


**--1.   List the following details of each employee: employee number, last name, first name, sex, and salary.** 
  
  
![image](https://user-images.githubusercontent.com/89948865/149842325-9393d382-a1b0-4998-934f-28b958e09626.png)

 
**--2.   List first name, last name, and hire date for employees who were hired in 1986.**

 
![image](https://user-images.githubusercontent.com/89948865/149842109-36424caa-be91-4296-82c4-282343a7d062.png) 
 
  
**--3.   List the manager of each department with the following information: department number, department name, the manager's employee number, last name, first name.** 
  
   
![image](https://user-images.githubusercontent.com/89948865/149842542-484d2115-9196-49b9-b0e6-41d5f12e535d.png) 
 
  
**--4.   List the department of each department with the following information: employee number,  last name, first name and department name.** 
   
    
![image](https://user-images.githubusercontent.com/89948865/149842674-0e4d68d1-145d-4697-935b-ea5ad4c74063.png) 
 
  
**--5.   List first name, last name, and sex for employees whose first name is "Hercules" and last names begin with "B."** 
 
![image](https://user-images.githubusercontent.com/89948865/149843021-fa8fb03b-5775-40f3-891b-ffa9a94d32be.png) 
  
  
**--6.   List all employees in the Sales department, including their employee number, last name, first name, and department name.** 
 
 
![image](https://user-images.githubusercontent.com/89948865/149843116-01fc3459-0db4-458b-ac67-fe4d8a617c7b.png) 
 
  
**--7.   List all employees in the Sales and Development departments, including their employee number, last name, first name, and department name.** 
 
 
![image](https://user-images.githubusercontent.com/89948865/149843245-7b0b30d1-e337-420b-a21b-c0fb61dda59d.png) 
 
 
 **--8.  In descending order, list the frequency count of employee last names, i.e., how many employees share each last name.** 
  
  
![image](https://user-images.githubusercontent.com/89948865/149843368-8dbca5c9-ccfb-4374-aa1d-cc08dbe01dd0.png) 
 
 
**5.	Copy of the Jupyter Notebook of the Analysis required for the Bonus Part of the assignment** 
        **Refer separate Jupyter Notebook for working code** 
		
