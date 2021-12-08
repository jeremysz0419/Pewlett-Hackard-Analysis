# Pewlett Hackard Analysis with SQL

## Overview of Pewlett Hackard Analysis

### Purpose
The purpose of this analysis was to determine the number of retiring employees by their titles and create a table of employees that are eligible for a mentorship program within the firm. 

## Results

### Retiring Employees Sorted By Titles

To find a list of retiring employees within the firm, we generated a list "retirement_titles" where we filtered for employees that were born between 1952-01-01 and 1955-12-31. 

```
SELECT e.emp_no,
       e.first_name,
       e.last_name,
       t.title,
       t.from_date,
       t.to_date
INTO retirement_titles
FROM employees as e
INNER JOIN titles as t
ON (e.emp_no = t.emp_no)
WHERE (e.birth_date BETWEEN '1952-01-01' AND '1955-12-31')
order by e.emp_no;
```
As many employees have gotten different titles over the years, we used the DISTINCT ON statement to remove any duplicate names and showed each employee with their most recent title/position within the firm.

```
SELECT DISTINCT ON (emp_no) emp_no,
first_name,
last_name,
title
INTO unique_titles
FROM retirement_titles
ORDER BY emp_no, title DESC;

```
We then created a table called "Retiring Titles" containing the number of employees within retirement age in each department. 


```
SELECT COUNT(ut.emp_no),
ut.title
INTO retiring_titles
FROM unique_titles as ut
GROUP BY title 
ORDER BY COUNT(title) DESC;

```

### Employees Eligible for Mentorship Program

To find employees who are eligibile for the mentorship program, we generated the "mentorship_eligibility.csv" file which contains all employees that were born between '1965-01-01' and '1965-12-31'. See our query below. 

```
SELECT DISTINCT ON (e.emp_no) e.emp_no,
	e.first_name,
	e.last_name,
	e.birth_date,
	de.from_date,
	de.to_date,
	t.title
INTO mentorship_eligibility
FROM employees AS e
LEFT OUTER JOIN dept_emp AS de
ON(e.emp_no = de.emp_no)
LEFT OUTER JOIN titles AS t
ON (e.emp_no = t.emp_no)
WHERE(e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')
AND (de.to_date = '9999-01-01')
ORDER BY e.emp_no;
```


### Results and Conclusion
- From our original analysis of finding employees that were born between 1952 and 1955, we uncovered that there were a significant amount of employees (almost 100,000) that are approching or within the retirement age. 
- We noted that a majority of the retiring employees are Staffs, Senior Enginneers, and Engineers. Only a handful of retiring employees are within the role of technique leaders and assistant engineer. 
- From filtering for employees born in 1965, we noted that there are about 1549 employees eligible for the mentorship program. 
- With 300,000 employees and 90,000 of them retiring, it meant that 1549 employees are responsible for mentoring about 200,000 individuals within the firm. This ratio may be a bit high and thus our recommendation may be to reduce the age in which an employee is eligible for mentorship. Mentorship programs should not be about the age in which an employee is but rather the merits in which the employee has. We recommend that management find another standard in which to develop this mentorship program. 




