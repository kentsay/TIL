#GYG Tech Interview

## Coding
#### 1. Separate a list of integers

Write a function to take the following list and return one list of odd numbers and one list of even numbers.

$a = array(1,21,53,84,50,66,7,38,9);

```java  
public ArrayList<ArrayList<Integer>> sepList(int[] inputData) {
	ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
  ArrayList<Integer> evenList = new ArrayList<Integer>();
  ArrayList<Integer> oddList = new ArrayList<Integer>();
  
  for(int i=0; i<inputData.length; i++) {
    if (inputData[i] % 2 == 0) {
      evenList.add(inputData[i]); 
    } else {
      oddList.add(inputData[i]);
    }
  }
  
  result.add(evenList);
  result.add(oddList);
  
  return result;
}
```
#### 2. SQL

Suppose you have a database with two tables, employee and department (see below for exact schema).
Write a SQL statement to answer the following questions:
1. Show a list of all employees that includes both the employee name and the department name.
2. Show a list of departments (name) and include the number of employees in each department.

employee
---------------
employee_id  
name  
salary  
department_id  

department
-----------------
department_id  
name  

```sql
  select 
    emmployee.name, department.name 
  from 
    employee,department 
  where employee.department_id = department.department_id;

  select 
    department.name, count(employee.employee_id) 
  from department
  left join employee on employee.department_id = department.department_id 
    group by department.name;
```
#### 3. Compute Fibonacci numbers

The Fibonacci numbers are the integer sequence 0, 1, 1, 2, 3, 5, 8, 13, 21, ...,
in which each item is formed by adding the previous two. The sequence can be defined recursively by

fib(0) = 0  
fib(1) = 1  
fib(n) = fib(n-1) + fib(n-2), for n>1  

E.g.
fib(0) = 0  
fib(1) = 1  
fib(2) = 1 + 0 = 1  
fib(3) = 1 + 1 = 2  
fib(4) = 2 + 1 = 3  
fib(5) = 3 + 2 = 5  
fib(6) = 5 + 3 = 8  
fib(7) = 8 + 5 = 13  


```java
public int fibonacci(int n) {
  if (n == 0) return 0;
  if (n == 1) return 1;
	int temp1 = 0;
  int temp2 = 1;
	int result = 0;
  for (int i=2; i<=n; i++) {
    result = temp1+temp2;
    temp1 = temp2;
    temp2 = result;
  }
  return result;
}
```

## Technical Questions

1. Consider database denormalized vs normalized. When do you want to do normalized. And when do you want to do denormalized.
2. You are writing a web crawler program. There are tons of url storing in one file now. Provide a method to find the deplicate url.
3. Assume you have many large image files on your work station. Please find the duplicate file of them. 


