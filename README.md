# PL-SQL

#Syntax

DECLARE
-- variable section
ordernumber constant number := 1001;
orderid number default 1002;
customername varchar2(20) := 'John';
BEGIN
/*
this is a mutliline comment
we can write what we are doing on the program
*/
dbms_output.put_line('Welcome to the course');
dbms_output.put_line(ordernumber);
dbms_output.put_line(orderid);
dbms_output.put_line(customername);
END;

#Global and Local variable

DECLARE
--Global variables
  num1 number := 95;
BEGIN
  dbms_output.put_line('Outer variable num1 ' || num1);

  DECLARE
    -- Local variables
    num2 number := 105;
  BEGIN
    dbms_output.put_line('Inner variable num1 ' || num1);
    dbms_output.put_line('Inner variable num2 ' || num2);
  END;
END;
/

# If Else Program

DECLARE
  total_amount number  := 201;
  discount number := 0;
BEGIN
  if total_amount > 200
  then
  discount := total_amount * .2;
  elsif total_amount >= 100 and total_amount <= 200
  then 
  discount := total_amount * .1;
  else
  discount := total_amount * .05;
  end if;

  dbms_output.put_line(discount);

END;
/

# Program related to CASE statement

DECLARE
  total_amount number := 201;
  discount number := 0;
BEGIN
  CASE
  WHEN total_amount > 200
  then
  discount := total_amount * 0.2;
  WHEN total_amount >= 100 and total_amount <= 200
  then
  discount := total_amount * 0.1;
  else
  discount := total_amount * 0.05;
  end CASE

  dbms_output.put_line(discount);
END;
/

# While loop Statement

DECLARE
  CNTR number(2) := 10;
BEGIN
  WHILE CNTR < 20
  LOOP
    dbms_output.put_line('Value of CNTR: ' || CNTR);
    CNTR := CNTR + 1;
  END LOOP;
END;
/

# For loop statement

DECLARE
  CNTR number(2) := 10;
BEGIN 
  FOR CNTR IN 10..20    -- FOR CNTR IN REVERSE 10..20
  LOOP
    dbms_output.put_line('Value of CNTR: ' || CNTR);
  END LOOP;
END;
/

#Fetching data from Database

You can use the SELECT INTO statement of SQL to assign values to PL/SQL variables. For each item in the SELECT list, there must be a corresponding, type-compatible variable in the INTO list.

DECLARE
  c_id number := 10;
  c_name varchar2(50);
  c_addr varchar2(50);
BEGIN
  SELECT first_name, country INTO c_name, c_addr
  FROM customer
  WHERE customer_id = c_id;

  dbms_output.put_line('Name: ' || c_name);
  dbms_output.put_line('Country: ' || c_addr);
END;
/

# what is %type (it is used to indicate the same datatype as it is stored in the table)

DECLARE
  c_id customer.customer_id%type := 10;
  c_name customer.first_name%type;
  c_addr customer.country%type;
BEGIN
  SELECT first_name, country INTO c_name, c_addr
  FROM customer
  WHERE customer_id = c_id;

  dbms_output.put_line('Name: ' || c_name);
  dbms_output.put_line('Country: ' || c_addr);
END;
/

# Inserting data into table

DECLARE
  c_id  customer.customer_id%type := 14;
  c_fname  customer.first_name%type := 'PETER';
  c_lname  customer.last_name%type := 'AFONSO';
  c_mname  customer.middle_name%type := 'A';
  c_add1  customer.address_line1%type := '23 SUWANEE RD';
  c_add2  customer.adddress_line2%type := NULL;
  c_city  customer.city%type := 'ALPHARETTA';
  c_country  customer.country%type := 'USA';
  c_date_added  customer.date_added%type := SYSDATE;
  c_region  customer.region%type := 'SOUTH';

BEGIN

  INSERT INTO CUSTOMER(customer_id, first_name, last_name, middle_name, address_line1, address_line2, city, country, date_added, region)
  VALUES(c_id, c_fname, c_lname, c_mname, c_add1, c_add2, c_city, c_country, c_date_added, c_region);

  COMMIT;

  dbms_output.put_line('Data successfully inserted');
END;
/

#Anonymous blocks

All the blocks we have seen so far are "anonymous" - they have no names.

If using anonymous blocks were the only way you could organize your statements, it would be very hard to use PL/SQL to build a large complex application.

Instead, PL/SQL supports the definition of named blocks of code (Procedures and Functions)

DECLARE
  c_id number := 10;
  c_name varchar2(50);
  c_addr varchar2(50);
BEGIN
  SELECT first_name, country INTO c_name, c_addr
  FROM customer
  WHERE customer_id = c_id;

  dbms_output.put_line('Name: ' || c_name);
  dbms_output.put_line('Country: ' || c_addr);
END;
/

#Procedures

A procedure is a group of PL/SQL statements that you can call by name. It can accepts value as input, process the data and return output if required.

CREATE [ OR REPLACE ] PROCEDURE procedure_name
  (parameter1 MODE DATATYPE [ DEFAULT expression ],
  parameter2 MODE DATATYPE [ DEFAULT expression ],
  ...)
AS
  [ variable1 DATATYPE;
  variable2 DATATYPE; ... ]
BEGIN
  executable_statements
[EXCEPTION
  WHEN
    exception_name
  THEN
    executable_statements ]
END;
/

MODE

MODE is usually one of the following: IN, OUT, IN OUT.

IN
  Keyword usable as mODE that means read-only. The caller supplies the value of the parameter, and PL/SQl prevents you from changing it inside the program.

  OUT
    Keyword usable as MODE that means write-only. As you might expect, OUT mode means that the procedure sets the value of the parameter, and the calling program can read it.
    (Any parameter value you attempt to supply when you call the program will be silently ignored.)

  IN OUT
    Keyword usable as MODE that means read or write. If you need to send a variable to a program that it can both read and update, and then have the updated value available to 
    the calling program, use the parameter mode IN OUT.

  #Creating a Procedure

  CREATE OR REPLACE PROCEDURE ADD_CUSTOMER
  (
  c_id  IN number,
  c_fname  IN varchar2,
  c_lname  IN varchar2,
  c_mname  IN varchar2,
  c_add1  IN varchar2,
  c_add2  IN varchar2,
  c_city  IN varchar2,
  c_country  IN varchar2,
  c_date_added  IN date,
  c_region  IN varchar2
  )
  AS
  BEGIN
    SELECT INTO CUSTOMER(customer_id, first_name, last_name, middle_name, address_lin1, address_line2, city, country, date_added, region)
    VALUES(c_id, c_fname, c_lnmae, c_mname, c_add1, c_add2, c_city, c_country, c_date_added, c_region);
    COMMIT;

    dbms_output.put_line('Data successfully inserted');
  END ADD_CUSTOMER;
  /
  
