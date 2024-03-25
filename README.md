1- CREATE TABLE department_file (
    department_id   NUMBER(5) PRIMARY KEY,
    department_name VARCHAR2(50),
    location        VARCHAR2(50)
);
   INSERT INTO department_file (
    department_id,
    department_name,
    department_location
) VALUES (
    4,
    'Sales',
    'Belfast'
);
------------------------------------------------------------------------
2- CREATE TABLE employee_file (
    emp_id     NUMBER(10) PRIMARY KEY,
    emp_name   VARCHAR2(50),
    job_title  VARCHAR2(50),
    manager_id NUMBER(10),
    date_hired DATE,
    salary     NUMBER(10),
    dept_id    NUMBER(5)
);
   INSERT INTO employee_file VALUES (
    90010,
    'Mildred Hall',
    'Secretary',
    90001,
    TO_DATE('12/10/96', 'DD/MM/YY'),
    35000,
    1
);
--------------------------------------------------------------------------
3- CREATE OR REPLACE PROCEDURE new_employee (
    p_emp_id     IN NUMBER,
    p_emp_name   IN VARCHAR2,
    p_job_title  IN VARCHAR2,
    p_manager_id IN NUMBER,
    p_date_hired IN DATE,
    p_salary     IN NUMBER,
    p_dept_id    IN NUMBER
) IS
BEGIN
    INSERT INTO employee_file (
        emp_id,
        emp_name,
        job_title,
        manager_id,
        date_hired,
        salary,
        dept_id
    ) VALUES (
        p_emp_id,
        p_emp_name,
        p_job_title,
        p_manager_id,
        p_date_hired,
        p_salary,
        p_dept_id
    );

    COMMIT;
    dbms_output.put_line('NEW EMPLOYEE FILE CREATED!');
END new_employee;
---------------------------------------------------------------------
4- CREATE OR REPLACE PROCEDURE salaryadj (
    p_emp_id     IN NUMBER,
    p_percentage IN NUMBER
) IS
    v_new_sal NUMBER;
BEGIN
    SELECT
        salary * ( 1 + p_percentage / 100 )
    INTO v_new_sal
    FROM
        employee_file
    WHERE
        emp_id = p_emp_id;

    UPDATE employee_file
    SET
        salary = v_new_sal
    WHERE
        emp_id = p_emp_id;

    COMMIT;
    dbms_output.put_line('EMPLOYEE SALARY ADJUSTED!');
END; 
---------------------------------------------------------------------
5- CREATE OR REPLACE PROCEDURE transfer_employee (
    p_emp_id      IN NUMBER,
    p_new_dept_id IN NUMBER
) IS
BEGIN
    UPDATE employee_file
    SET
        emp_id = p_new_dept_id
    WHERE
        dept_id = p_emp_id;

    COMMIT;
    dbms_output.put_line('EMPLOYEE TRANSFERED!');
END transfer_employee;
----------------------------------------------------------------------------
6- CREATE OR REPLACE FUNCTION get_emp_sal (
    p_emp_sal IN NUMBER
) RETURN NUMBER IS
    v_sal NUMBER;
BEGIN
    SELECT
        salary
    INTO v_sal
    FROM
        employee_file
    WHERE
        emp_id = p_emp_sal;

    RETURN v_sal;
END get_emp_sal;
----------------------------------------------------------------------------
7- SELECT * FROM employee_file WHERE DEPT_ID= (---DESIRED DEPARTMENT NUMBER);
----------------------------------------------------------------------------
8- SELECT sum(salary) as total_sal from employee_file where DEPT_ID=(---desired department number);
