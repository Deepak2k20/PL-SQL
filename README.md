# PL-SQL

#Syntax

```sql
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

# Anonymous blocks

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

# Procedures

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

# MODE

MODE is usually one of the following: IN, OUT, IN OUT.

IN
  Keyword usable as mODE that means read-only. The caller supplies the value of the parameter, and PL/SQl prevents you from changing it inside the program.

  OUT
    Keyword usable as MODE that means write-only. As you might expect, OUT mode means that the procedure sets the value of the parameter, and the calling program can read it.
    (Any parameter value you attempt to supply when you call the program will be silently ignored.)

  IN OUT
    Keyword usable as MODE that means read or write. If you need to send a variable to a program that it can both read and update, and then have the updated value available to 
    the calling program, use the parameter mode IN OUT.

  # Creating a Procedure

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

# Calling a Procedure

#Method1

BEGIN
  ADD_CUSTOMER(12,'JEFF','AFONSO','A",'23 SUWANNEE RD', NULL, 'ALPMARETTA', 'USA', SYSDATE, 'SOUTH');
END;

#Method2

BEGIN
  ADD_CUSTOMER
  (
  c_region  => 'SOUTH',
  c_id  => 15,
  c_fname  => 'JEFF',
  c_lname  => 'AFONSO',
  c_mname  => 'A',
  c_add1  => '23 SUWANNEE RD',
  c_add2  => NULL,
  c_city  => 'ALPHARETTA',
  c_country  => 'USA',
  c_date_added  => SYSDATE
  );
  END;

# Procedure with OUT Mode

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
  c_region  IN varchar2,
  total_count OUT number
  )
  AS
  BEGIN
    SELECT INTO CUSTOMER(customer_id, first_name, last_name, middle_name, address_lin1, address_line2, city, country, date_added, region)
    VALUES(c_id, c_fname, c_lnmae, c_mname, c_add1, c_add2, c_city, c_country, c_date_added, c_region);
    COMMIT;

    dbms_output.put_line('Data successfully inserted');

    SELECT COUNT(1) INTO total_count FROM customer;
    
  END ADD_CUSTOMER;
  /

  #Calling Procedure

  DECLARE
    tcount number(10);
  BEGIN
    ADD_CUSTOMER(14,'JEFF','AFONSO','A','23 SUWANNEE RD', NULL, 'ALPHARETTA', 'USA', SYSDATE,'SOUTH',tcount);
    dbms_output.put_line('Total Records: ' || tcount);
  END;

# Procedure with IN OUT Mode

CREATE OR REPLACE PROCEDURE ADD_CUSTOMER
  (
  c_id  IN OUT number,
  c_fname  IN varchar2,
  c_lname  IN varchar2,
  c_mname  IN varchar2,
  c_add1  IN varchar2,
  c_add2  IN varchar2,
  c_city  IN varchar2,
  c_country  IN varchar2,
  c_date_added  IN date,
  c_region  IN varchar2,
  )
  AS
  BEGIN
    SELECT INTO CUSTOMER(customer_id, first_name, last_name, middle_name, address_lin1, address_line2, city, country, date_added, region)
    VALUES(c_id, c_fname, c_lnmae, c_mname, c_add1, c_add2, c_city, c_country, c_date_added, c_region);
    COMMIT;

    dbms_output.put_line('Data successfully inserted');

    SELECT COUNT(1) INTO C_ID FROM customer;
    
  END ADD_CUSTOMER;
  /

  #Calling Procedure

  DECLARE
    tcount number(10) := 45;
  BEGIN
    ADD_CUSTOMER(tcount,'JEFF','AFONSO','A','23 SUWANNEE RD', NULL, 'ALPHARETTA', 'USA', SYSDATE,'SOUTH');
    dbms_output.put_line('Total Records: ' || tcount);
  END;

# Functions

A stored function (also called a user function or user-defined function) is a set of PL/SQL statements you can call by name.
Stored functions are very similar to procedures, except that a function returns a value to the environment in which it is called
User functions can be used as part of a SQL expression

CREATE [ OR REPLACE ] FUNCTION function_name
(parameter1 MODE DATATYPE [ DEFAULT expression ],
parameter2 MODE DATATYPE [ DEFAULT expression ],
...)
RETURN DATATYPE
AS
  [ variable1 DATATYPE,
  variable2 DATATYPE; ... ]
BEGIN
  executable_statements
  RETURN expression;
[ EXCEPTION
  WHEN
    exception_name
  THEN
    executable_statements ]
END;
/

# Function example

CREATE OR REPLACE FUNCTION find_salescount
(
  p_sales_date IN date,
) RETURN number
AS
  num_of_sales number := 0;
BEGIN
  SELECT COUNT(*) INTO num_of_sales FROM sales
  WHERE sales_date = p_sales_date;

  RETURN num_of_sales;

END find_salescount;

#Function calling

select find_salescount(to_date('01-jan-2015','dd-mon-yyyy'));

or

DECLARE
SCOUNT number := 0;
BEGIN
SCOUNT := find_salescount(to_date('01-jan-2015','dd-mon-yyyy'));
dbms_output.put_line(SCOUNT);
END;

# Exceptions

An error condition during a program execution is called an exception in PL/SQL.

We can catch errors using EXCEPTION block in the program and an appropriate action is taken against the error condition.

There are two types of exceptions:

1) System-defined exceptions
2) User-defined exceptions

DECLARE
  < declaration section >
BEGIN
  < executable section(s) >
EXCEPTION
  WHEN exception1 THEN
    exception1-handling-statements
  WHEN exception2 THEN
    exception2-handling-statements
  WHEN exception3 THEN
    exception3-handling-statements
  ........
  WHEN others THEN
    exception-handling-statements
END;

#What erros

CREATE OR REPLACE PROCEDURE GET_CUSTOMER
(
c_id IN number
)
AS
  c_name customer.first_name%type;
  c_cntry customer.country%type;
BEGIN
  SELECT first_name, country INTO c_name, c_cntry
  FROM customer
  WHERE customer_id = c_id;

  dbms_output.put_line('Name: ' || c_name);
  dbms_output.put_line('Country: ' || c_cntry);
END;
/

Number of rows matching where clause	        | runtime behaviour	                                    | value of sqlcode
1	                                            | success: assigns column values to local variables	    | 0 (no error)
0	                                            | Raises NO_DATA_FOUND exception	                      | 100
More than 1	                                  | Raises TOO_MANY_ROWs exception	                      | -1422
![image](https://github.com/Deepak2k20/PL-SQL/assets/65231118/94cd091f-0f23-4cba-b9c5-25f8adda58fe)


#Example

Let us write some simple code to illustrate the concept. We will be using the CUSTOMERS table we had created and used in the previous chapters;

CREATE OR REPLACE PROCEDURE GET_CUSTOMER
(
c_id  IN number
)
AS
  c_name customer.first_name%type;
  c_cntry customer.country%type;
BEGIN
  SELECT first_name, country INTO c_name, c_cntry
  FROM customer
  WHERE customer_id = c_id;

  dbms_output.put_line('Name: ' || c_name);
  dbms_output.put_line('Country: ' || c_cntry);
EXCEPTION
  WHEN no_data_found THEN
    dbms_output.put_line('No such customer!');
  WHEN too_many_rows THEN
    dbms_output.put_line('You got more than 1 row!');
  WHEN others THEN
    dbms_output.put_line('Error!');
END;
/

# User defined Exceptions

PL/SQL allows you to define your own exceptions according to the need of your program. A user-defined exception must be declared and then raised explicitly, using either a RAISE

CREATE OR REPLACE PROCEDURE GET_CUSTOMER
(
c_id  IN number
)
AS
DECLARE
  c_name customer.first_name%type;
  c_cntry customer.country%type;
  ex_customer_id EXCEPTION;
BEGIN
  IF c_id <= 0 THEN
    RAISE ex_customer_id;
  END IF;

  SELECT first_name, country INTO c_name, c_addr
  FROM customer
  WHERE customer_id = c_id;

  dbms_output.put_line('Name: " || c_name);
  dbms_output.put_line('Country: ' || c_addr);
EXCEPTION
  WHEN ex_customer_id THEN
    dbms_output.put_line('ID must be greater than zero!');
  WHEN no_data_found THEN
    dbms_output.put_line('No such customer!');
  WHEN too_many_rows THEN
    dbms_output.put_line(You got more than 1 row!');
  WHEN others THEN
    dbms_output.put_line('Error!');
END;
/

# PL/SQL Packages to organize code

A package is a schema object that groups logically related PL/SQl types, items, and subprograms like procedures and functions.

Packages can hold other constructs too, such as exceptions, variables, cursors and type declarations.

Packages usually have two parts:

1) Specification
2) Body

# Advantages of Packages

#Modularity

Packages let you encapsulate logically related types, items, and subprograms in a named PL/SQL module. Each package is easy to understand, and the interfaces between packages are simple, clear, and well defined.

#Easier Application Design

When designing an application, all you need initially is the interface information in the package specs. You can code and compile a spec without its body. Then, stored subprograms that reference the package can be compiled as well. You need not define the package bodies fully until you are ready to complete the application.

#Information Hiding

With packages, you can specify which types, items, and subprograms are public (visible and accessible) or private (hidden and inaccessible)

This simplifies maintenance and enhancement. Also, by hiding implementation details from users, you protect the integrity of the package.

#Better Performance

When you call a packaged subprogram for the first time, the whole package is loaded into memory. So, later calls to related subprograms in the package require no disk I/O.

#Added Functionality

Packaged public variables and cursors persist for the duration of a session. So, they can be shared by all subprograms that execute in the environment. Also, they allow you to maintain data across transactions without having to store it in the database.

# Package Specification

The package specification tells a user of the package what it can do rather than how it will do it. The spec conatins only the headers of the program units rather than any executable code.

It's kind of like a declaration section for program units.

CREATE OR REPLACE PACKAGE package_name
AS
  program1_header;
  program2_header;
  program3_header;
END package_name;
/

#Example

CREATE OR REPLACE PACKAGE CUSTOMER_PACKAGE
AS
PROCEDURE ADD_CUSTOMER
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
);

PROCEDURE DISPLAY_NAMES;

PROCEDURE GET_CUSTOMER
(
c_id  IN number
);

PROCEDURE SHOW_CUSTOMER
(
customer_id  IN customer%ROWTYPE
);

FUNCTION find_salescount
(
p_sales_date  IN date
) RETURN number;

FUNCTIONS get_names
(
cust_id  IN number
)RETURN SYS_REFCURSOR;

END CUSTOMER_PACKAGE;

# Package Body

The package body conatins the program unit bodies - that is to say, the executable statements that correspond with the headers in the package specification.

CREATE OR REPLACE PACKAGE BODY package_name
AS
  program1_header;
  program2_header;
  program3_header;

END package_name;
/

#Example

CREATE OR REPLACE PACKAGE BODY CUSTOMER_PACKAGE
AS

PROCEDURE ADD_CUSTOMER
  (
  c_id  IN OUT number,
  c_fname  IN varchar2,
  c_lname  IN varchar2,
  c_mname  IN varchar2,
  c_add1  IN varchar2,
  c_add2  IN varchar2,
  c_city  IN varchar2,
  c_country  IN varchar2,
  c_date_added  IN date,
  c_region  IN varchar2,
  )
  AS
  BEGIN
    SELECT INTO CUSTOMER(customer_id, first_name, last_name, middle_name, address_lin1, address_line2, city, country, date_added, region)
    VALUES(c_id, c_fname, c_lnmae, c_mname, c_add1, c_add2, c_city, c_country, c_date_added, c_region);
    COMMIT;

    dbms_output.put_line('Data successfully inserted');

    SELECT COUNT(1) INTO C_ID FROM customer;
    
  END ADD_CUSTOMER;

PROCEDURE DISPLAY_NAMES
IS
  C_REC SYS_REFCURSORS;
  FNAME varchar2(50);
  LNAME varchar2(50);
BEGIN
  C_REC := GET_NAMES(10);
LOOP
  FETCH C_REC INTO FNAME, LNAME;
  EXIT WHEN C_REC%NOTFOUND;
  dbms_output.put_line(FNAME);
  dbms_output.put_line(LNAME);
END LOOP;
CLOSE C_REC;
END;

PROCEDURE GET_CUSTOMER
(
c_id  IN number
)
AS
  c_name  customer.first_name%type;
  c_cntry  customer.country%type;
  c_coustomer_id_exception EXCEPTION;
BEGIN

  IF C_ID <= 0 THEN
  RAISE c_customer_id_exception;
  END IF;

  SELECT first_name, country INTO c_name, c_cntry
  FROM customer
  WHERE customer_id = c_id;

  dbms_output.put_line('Name: ' || c_name);
  dbms_output.put_line('Country: ' || c_cntry);
EXCEPTION
  WHEN NO_DATA_FOUND THEN
    dbms_output.put_line('No Data Found!');
  WHEN c_customer_id_exception THEN
    dbms_output.put_line('Customer id must be greater than 0');
  WHEN OTHERS THEN
    dbms_output.put_line('Other Errors');
END;

PROCEDURE show_customer
(
  customer_id IN customer%ROWTYPE;
)
IS
BEGIN
  update customer set row = customer_in where customer_id = customer_in.customer_id;
  COMMIT;
END;

FUNCTION find_salescount
(
  p_sales_date  IN date
)RETURN number
AS
  num_of_sales number := 0;
BEGIN
  SELECT COUNT(*) INTO num_of_sales FROM sales
  WHERE sales_date = p_sales_date;

  RETURN num_of_sales;
END find_salescount;

FUNCTION get_names
(
  custid IN number
)
RETURN SYS_REFCURSOR;
IS
  l_return SYS_REFCURSOR;
BEGIN
  OPEN l_return FOR
    SELECT first_name, last_name
    FROM customer
    WHERE customer_id = custid;
  RETURN l_return;
END get_names;

END CUSTOMER_PACKAGE;

# Executing subprograms present in Packages

execute customer_package.display_names;

execute customer_package.get_customer(16);

# Records

It is rare, Indeed, to find a PL/SQL program that does not either read or make changes to tables in a database. Tables are made up of rows of data, each consisting of one or more columns, so it stands to reason that Oracle Database would make it as easy as possible to work with those rows of data inside a PL/SQL program. And it does precisely that through its implementation of the record.

A record is a composite datatype, which means that it can hold more than on epiece of information, as compared to a scalar datatype, such as number or string.

c_fname | c_lname | c_mname | c_add1 | c_add2 | c_city | c_country | c_date_added | c_region
1       |   Afonso |   Jeffrino  | 123 Ste  | Drive | ATLAN  | USA | 12/DEC/2015 | South

customer_rec

1,Afonso,Jeffrino,123 Ste, Drive, ATLAN, USA, 12/DEC/2015,South

#Advantages

1) It enables you to write code that is simple, clean, and easy to maintain.
2) Rather than work with long lists of variables or parameters, you can work with a record that conatins all that information

# Records Example

CREATE OR REPLACE PROCEDURE process_customer
(
c_id  IN customer.customer_id%type
)
IS
  c_rec customer%rowtype;
  c_rec1 customer%rowtype;
BEGIN
  SELECT *
  INTO c_rec
  FROM customer
  WHERE customer_id = c_id;

  c_rec1 := c_rec;

  c_rec.first_name := 'sonu';
  c_rec.last_name := 'kumar';

  dbms_output.put_line('First Name: ' || c_rec.first_name);
  dbms_output.put_line('Last Name: ' || c_rec.last_name);

  dbms_output.put_line('First Name: ' || c_rec1.first_name);
  dbms_output.put_line('Last Name: " || c_rec1.last_name);

END;

# Working with Record Data

CREATE OR REPLACE PROCEDURE process_customer
(
c_id  IN customer.customer_id%type
)
IS
  c_rec customer%rowtype;
BEGIN
  SELECT *
  INTO c_rec
  FROM customer
  WHERE customer_id = c_id;

  dbms_output.put_line('First Name: ' || c_rec.first_name);
  dbms_output.put_line('Last Name: ' || c_rec.last_name);

END;

# Passing Records as parameters

CREATE OR REPLACE PROCEDURE process_customer
(
c_id  IN customer.customer_id%type
)
IS
  c_rec  customer%rowtype;
BEGIN
  SELECT *
  INTO c_rec
  FROM customer
  WHERE customer_id = c_id;

  show_customer(c_rec);

END;

CREATE PROCEDURE show_customer
(
customer_in IN customer%rowtype;
)
IS
BEGIN
  dbms_output.put_line('First Name: ' || customer_in.first_name);
  dbms_output.put_line('Last Name: ' || customer_in.last_name);
END;
/

EXECUTE process_customer(10);

# Inserting data using Records

CREATE OR REPLACE PROCEDURE process_customer
(
c_id  IN customer.customer_id%type
)
IS
  c_rec  customer%rowtype;
BEGIN
  SELECT *
  INTO c_rec
  FROM customer
  WHERE customer_id = c_id;

  show_customer(c_rec);

END;

CREATE OR REPLACE PROCEDURE show_customer
(
customer_in IN customer%rowtype;
)
IS
BEGIN
  INSERT INTO customer VALUES customer_in;
  COMMIT;
END;
/

EXECUTE process_customer(17);

SELECT * FROM customer WHERE customer_id = 17;

# Updating data using Records

CREATE OR REPLACE PROCEDURE process_customer
(
c_id  IN customer.customer_id%type
)
IS
  c_rec  customer%rowtype;
BEGIN
  SELECT *
  INTO c_rec
  FROM customer
  WHERE customer_id = c_id;

  c_rec.first_name := 'Amit';
  show_customer(c_rec);

END;

CREATE OR REPLACE PROCEDURE show_customer
(
customer_in IN customer%rowtype;
)
IS
BEGIN
  update customer set row = customer_in where customer_id = customer_in.customer_id;
  COMMIT;
END;
/

EXECUTE process_customer(16);

SELECT * FROM customer WHERE customer_id = 16;

# User defined Record Types

So far you've seen how to declare a record variable based on a table or a cursor by using the %rowtype attribute. You can also declare youw own, user-defined record types by using the TYPE...RECORD statement.

#Advantage

User-defined records offer the flexibility to construct youw own composite datatype, reflecting program-specific requirements that may not be represented by a relational table.

TYPE customer_info_rt IS RECORD
(
name varchar2(100),
total_sales number,
deliver_pref varchar2(10)
);

l_customer1 customer_info_rt;
l_customer2 customer_info_rt;

# User defined Record Example

CREATE OR REPLACE PROCEDURE process_customer
(
c_id IN customer.customer_id%type;
)
IS
TYPE CUSTOMER_REC IS RECORD
(
FIRST_NAME varchar2(50),
LAST_NAME varchar2(50)
);
c_rec CUSTOMER_REC;
BEGIN
  SELECT FIRST_NAME, LAST_NAME
  INTO c_rec
  FROM customer
  WHERE customer_id = c_id;

  dbms_output.put_line('First Name: ' || c_rec.first_name);
  dbms_output.put_line('Last Name: ' || c_rec.last_name);
END;

EXECUTE process_customer(16);

# Cursors

Oracle craetes a memory area, known as context area, for processing an SQL statement, which contains all information needed for processing the statement.

A cursor is a pointer to this context area. PL/SQL controls the context area through a cursor. A cursor holds the rows (one or more) returned by a SQL statement. The set of rows the cursor holds is referred to as the active set.

You can name a cursor so that it could be referred to in a program to fetch and process the rows returned by the SQL statement, one at a time. There are two types of cursors:

1) Implicit cursors
2) Explicit cursors

# Implicit Cursor

Implicit cursors are automatically created by Oracle whenever an SQL statement is executed.

Whenever a DML statement (INSERT, UPDATE, and DELETE) is issued, an implicit cursor is associated with this statement.

For INSERT operations, the cursor holds the data that needs to be inserted. For UPDATE and DELETE operations, the cursor identifies the rows that would be affected.

In PL/SQL, you can refer to the most recent implicit cursor as the SQL cursor, which always has the attributes like

%FOUND

Returns TRUE if an INSERT, UPDATE, or DELETE statement affected one or more rows or a SELECT INTO statement returned one or more rows. Otherwise, it returns FALSE.

%ISOPEN

Always return FALSE, because Oracle closes the SQL cursor automatically after executing its associated SQL statement.

%NOTFOUND

The logical opposite of %FOUND. It returns TRUE if an INSERT, UPDATE, or DELETE statement affected no rows, or a SELECT INTO statement returned no rows. Otherwise, it returns FALSE.

%ROWCOUNT

Returns the number of rows affected by an INSERT, UPDATE, or DELETE statement, or returned by a SELECT INTO statement.

# Implicit Cursor

CREATE OR REPLACE PROCEDURE process_customer
(
c_id IN customer.customer_id%type;
)
IS
TYPE CUSTOMER_REC IS RECORD
(
FIRST_NAME varchar2(50),
LAST_NAME varchar2(50)
);
c_rec CUSTOMER_REC;
total_rows number;
BEGIN
  SELECT FIRST_NAME, LAST_NAME
  INTO c_rec
  FROM customer
  WHERE customer_id = c_id;

  IF sql%found THEN
    total_rows := sql%rowcount;
    dbms_output.put_line(total_rows || ' customers selected ');
  END IF;

  dbms_output.put_line('First Name: ' || c_rec.first_name);
  dbms_output.put_line('Last Name: ' || c_rec.last_name);
END;

EXECUTE process_customer(16);

# Explicit Cursor

A cursor is the anme for a structure in memory, called a private SQL area, which the server allocates at runtime for each SQL statement.

If the host program uses any variables in the SQL statement, the cursor also contains the memory addresses of these variables.

#How to code an Explicit cursor

We explicitly code each one of the following steps:

1) Declare
2) Open
3) Fetch one or more times
4) Close

# Retrieving 1 row using Explicit Cursor

CREATE OR REPLACE PROCEDURE process_customer
(
c_id IN customer.customer_id%type;
)
IS
  c_fname  customer.first_name%type;
  c_lname  customer.last_name%type;
  c_mname  customer.middle_name%type;
  c_add1  customer.address_line1%type;
  c_add2  customer.address_line%type;
  c_city  customer.city%type;
  c_country  customer.country%type;
  c_date_added  customer.date_added%type;
  c_region  customer.region%type;

cursor c is SELECT first_name, last_name, middle_name, address_line1, address_line2, city, country, date_added, region
FROM customer
WHERE customer_id = c_id;  -- declaration of cursor

BEGIN

  open c; -- opening of cursor

  fetch c
  INTO c_fname, c_lname, c_mname, c_add1, c_add2, c_city, c_country, c_date_added, c_region; -- fetching of data

  dbms_output.put_line('First Name: ' || c_fname);
  dbms_output.put_line('Last Name: ' || c_lname);
  dbms_output.put_line('Middle Name: ' || c_mname);
  dbms_output.put_line('Address Line1: ' || c_add1);
  dbms_output.put_line('Address Line2: ' || c_add2);
  dbms_output.put_line('City: ' || c_city);
  dbms_output.put_line('Country: ' || c_country);
  dbms_output.put_line('Date Added: ' || c_date_added);
  dbms_output.put_line('Region: ' || c_region);

close c; -- closing of cursor

END;

execute process_customer(16);

# Retrieving more than 1 row using Explicit Cursor

CREATE OR REPLACE PROCEDURE process_customer
(
c_id IN customer.customer_id%type;
)
IS
  c_fname  customer.first_name%type;
  c_lname  customer.last_name%type;
  c_mname  customer.middle_name%type;
  c_add1  customer.address_line1%type;
  c_add2  customer.address_line%type;
  c_city  customer.city%type;
  c_country  customer.country%type;
  c_date_added  customer.date_added%type;
  c_region  customer.region%type;

cursor c is SELECT first_name, last_name, middle_name, address_line1, address_line2, city, country, date_added, region
FROM customer
WHERE customer_id = c_id;  -- declaration of cursor

BEGIN

  open c; -- opening of cursor

LOOP
  fetch c
  INTO c_fname, c_lname, c_mname, c_add1, c_add2, c_city, c_country, c_date_added, c_region; -- fetching of data
EXIT WHEN C%NOTFOUND;
  dbms_output.put_line('First Name: ' || c_fname);
  dbms_output.put_line('Last Name: ' || c_lname);
  dbms_output.put_line('Middle Name: ' || c_mname);
  dbms_output.put_line('Address Line1: ' || c_add1);
  dbms_output.put_line('Address Line2: ' || c_add2);
  dbms_output.put_line('City: ' || c_city);
  dbms_output.put_line('Country: ' || c_country);
  dbms_output.put_line('Date Added: ' || c_date_added);
  dbms_output.put_line('Region: ' || c_region);
END LOOP:
close c; -- closing of cursor

END;

execute process_customer(10);

# Using Records in Cursors

CREATE OR REPLACE PROCEDURE process_customer
(
c_id IN customer.customer_id%type;
)
IS

cursor c is SELECT first_name, last_name, middle_name, address_line1, address_line2, city, country, date_added, region
FROM customer
WHERE customer_id = c_id;  -- declaration of cursor

c_rec c%rowtype;

BEGIN

  open c; -- opening of cursor

LOOP
  fetch c
  INTO c_rec; -- fetching of data
EXIT WHEN C%NOTFOUND;
  dbms_output.put_line('First Name: ' || c_rec.first_fname);
  dbms_output.put_line('Last Name: ' || c_rec.last_lname);
  dbms_output.put_line('Middle Name: ' || c_rec.middle_mname);

END LOOP:
close c; -- closing of cursor

END;

execute process_customer(16);

# Cursor For Loop

The cursor FOR loop is just an extension of the numberic FOR loop in PL/SQL. With a cursor FOR loop, the body of the loop is executed for each row returned by the query.

#Advantages

1) Oracle Database opens the cursor, declares a record by using %ROWTYPE against the cursor, fetches each row into a record, and then closes the loop when all the rows have been fetched
2) Even though code loks as if you are fetching one row at a time, Oracle Database will actually fetch 100 rows at a time and enable you to work with each row individually.

FOR customer_rec IN
  (SELECT * FROM customer WHERE customer_id = 10)
LOOP
  dbms_output.put_line(customer_rec.last_name);
END LOOP;


#Example

CREATE OR REPLACE PROCEDURE process_customer
(
c_id IN customer.customer_id%type;
)
IS
BEGIN

 FOR c_rec IN (SELECT first_name, last_name, middle_name, address_line1, address_line2, city, country, date_added, region
 FROM customer
 WHERE customer_id = c_id)

LOOP
  dbms_output.put_line('First Name: ' || c_rec.first_fname);
  dbms_output.put_line('Last Name: ' || c_rec.last_lname);
  dbms_output.put_line('Middle Name: ' || c_rec.middle_mname);
END LOOP:

END;

execute process_customer(16);

# Cursor Variable and Reference Cursor

A cursor variable is a variable that points to a cursor or a result set. You can pass aa cursor variable as an argument to a procedure or a function.

Pass a cursor variable back to the host environment that called the program unit - the result set can be "consumed" for display or other processing.

c_variable SYS_REFCURSOR;

#Example

CREATE OR REPLACE FUNCTION get_names
(
custid IN number
)
RETURN SYS_REFCURSOR
IS
  l_return SYS_REFCURSOR;
BEGIN
  OPEN l_return FOR
    SELECT first_name, last_name
    FROm customer
    WHERE customer_id = custid;
RETURN l_return;

END get_names;

CREATE OR REPLACE PROCEDURE display_names
IS
c_rec SYS_REFCURSOR;
FNAME varchar2(50);
LNAME varcahr2(50);
BEGIN
c_rec := get_names(10);

LOOP
FETCH c_rec INTO FNAME, LNAME
EXIT WHEN c_rec%NOTFOUND;
dbms_output.put_line(FNAME);
dbms_output.put_line(LNAME);
END LOOP;

CLOSE c_rec;

END;

EXECUTE display_names;

# Exceptions for Cursors

CURSOR_ALREADY_OPEN

You get this exception when you try to open a cursor that is already open.

INVALID_CURSOR

You tried to reference a cursor that does not yet exist. This may have happened because you've executed a FETCH cursor or CLOSE cursor before opening the cursor.

# Collections

We learnt about a composite datatype called RECORD, which is composed of one or more fields.

1) In the same way Collection is another composite datatype
2) An Oracle PL/SQL collection is a single-dimensional array
3) It consists of one or more elements accessible through an index value
4) All the elements have the same data type

JOHN	TOM	JEFF	KIRAN	KHAN
1	2	3	4	5
![image](https://github.com/Deepak2k20/PL-SQL/assets/65231118/3d9a955c-87f5-40aa-97a7-43fc3fcdfde3)

# Advantages

Collections are used in some of the most important performance optimization features of PL/SQL, such as

1) BULK COLLECT : SELECT statements that retrieve multiple rows with a single fetch, incraesing the speed of data retrievel

2) FORALL: Inserts, updates, and deletes that use collections to change multiple rows of data very quickly

3) Table functions: PL/SQL functions that return collections and can be called in the FROM clause of a SELECT statement

You can also use collections to work with lists of data in your program that are not stored in database tables.

# Collection Terminology

Dense Collection				
JOHN	TOM	JEFF	KIRAN	KHAN
1	2	3	4	5
![image](https://github.com/Deepak2k20/PL-SQL/assets/65231118/bb0d40d2-2088-4cbc-87c6-bbc9203195e8)

Sparse Collection				
JOHN	TOM			KHAN
1	2	3	4	5
![image](https://github.com/Deepak2k20/PL-SQL/assets/65231118/13b5d8b8-d162-457a-9f36-7773db1e33fe)

#Index value

The location of the data in a collection. Index values are usually integers but for one type of collection can also be strings.

#Element

The data stored at a specific index value in a collection. Elements in a collection are always of the same type (all of them are strings, dates, or records). PL/SQL collections are homogeneous.

#Sparse

A collection is sparse if there is at least one index value between the lowest and heighest defined index values that is not defined. For example, a sparse collection has an element assigned to index value 1 and another to index value 10 but nothing in between. The opposite of a sparse collection is a dense one.

#Method

A collection method is a procedure or function that either provides information about the collection or changes the contents of the collection. Methods are attached to the collection variable with dot notation(object-oriented syntax), as in my_collection.FIRST

# Index by Tables (Associative Array)

1) As the name implies, the collection is indexed using BINARY_INTEGER values or VARCHAR2 values, which do not need to be consecutive.
2) We can not store this collection in a database
3) They were originally called as Pl/SQL Tables
4) You do not need to initialize a Associative Array collection

DECLARE
  TYPE customer_type IS TABLE OF varchar2(100) INDEX BY BINARY_INTEGER;
    customer_table customer_type
    v_idx number;
BEGIN

  customer_table(1) := 'MIKE';
  customer_table(2) := 'JEFF';
  customer_table(3) := 'JOHN';
  customer_table(6) := 'KING';

  -- Delete the third item of the collection
customer_table.DELETE(3);

-- Traverse sparse collection
  v_idx := customer_table.FIRST;

WHILE v_idx IS NOT NULL LOOP
  dbms_output.put_line('Customer Name ' || customer_table(v_idx));
  v_idx := customer_table.NEXT(v_idx);
END LOOP display_loop;
END;
/

# Nested Tables

1) Nested table can be stored in a database
2) Nested tables can be sparse but are almost always dense
3) They can be indexed only by integer
4) You can use the MULTISET operation to perform set operations and to perform equality comparisons on nested tables

DECLARE
  TYPE customer_type IS TABLE OF varchar2(100);
  customer_table customer_type := customer_type();  -- Initialize the collection
  v_idx number;
BEGIN
  customer_table.EXTEND(4); -- You have to extend before using the table

  customer_table(1) := 'MIKE';
  customer_table(2) := 'JEFF';
  customer_table(3) := 'JOHN';
--customer_table(6) := 'KING';  -- Throws an error
  customer_table(4) := 'KING';  -- It must be sequential

-- Delete the third item of the collection
customer_table.DELETE(3);

dbms_output.put_line('Customer Name ' || customer_table(customer_table.first));
dbms_output.put_line('Customer Name ' || customer_table(customer_table.last));

-- Traverse Dense collection
v_idx := customer_table.FIRST;

WHILE v_idx IS NOT NULL LOOP
  dbms_output.put_line('Customer Name ' || customer_table(v_idx));
  v_idx := customer_table.NEXT(v_idx);
END LOOP display_loop;
END;
/

# VARRAY

1) A VARRAY is similar to a nested table except you must specify an upper bound in the declaration
2) Like nested tables they can be stored in the database
3) Unlike nested tables individual elements cannot be deleted so they remain dense

DECLARE
  TYPE customer_type IS VARRAY(4) OF VARCHAR2(100);
  customer_table customer_type := customer_type();  -- Initialize the collection
  v_idx number;

BEGIN
  customer_table.EXTEND(4);  -- You have to extend before using the table
  customer_table(1) := 'MIKE';
  customer_table(2) := 'JEFF';
  customer_table(3) := 'JOHN';
  customer_table(6) := 'KING';  -- Throws an error
--customer_table(4) := 'KING';  -- It must be sequential

-- Can not Delete an item
-- customer_table.DELETE(3);

-- Traverse Dense collection
v_idx := customer_table.FIRST;

WHILE v_idx IS NOT NULL LOOP
  dbms_output.put_line('Customer Name ' || customer_table(v_idx));
  v_idx := customer_table.NEXT(v_idx);
END LOOP display_loop;
END;
/

# Collection Methods

1) EXISTS(n) - Returns TRUE if the specified element exists
2) COUNT - Returns the number of elements in the collection
3) LIMIT - Returns the maximum number of elements for a VARRAy, or NULL for nested tables
4) FIRST - Returns the index of the first element in the collection
5) LAST - Returns the index of the last element in the collection
6) PRIOR(n) - Returns the index of the element prior to the specified element
7) NEXT(n) - Returns the index of the next element after the specified element
8) EXTEND - Appends a single null element to the collection
9) EXTEND(n) - Appends n null elements to the collection
10) EXTEND(n1, n2) - Appends n1 copies to the n2th element to the collection
11) TRIM - Removes a single element from the end of the collection
12) TRIM(n) - Removes a elements from the end of the collection
13) DELETE - Removes all elements from the collection
14) DELETE(n1, n2) - Removes all elements from n1 to n2 from the collection

# Multiset Operators

-- MULTISET UNION
-- MULTISET UNION DISTINCT
-- MULTISET EXCEPT
-- MULTISET INTERSECT

DECLARE
  TYPE t_tab IS TABLE OF NUMBER;
  l_tab1 t_tab := t_tab(1,2,3,4,5,6);
  l_tab2 t_tab := t_tab(5,6,7,8,9,10);
BEGIN
  l_tab1 := l_tab1 MULTISET UNION l_tab2;

FOR i IN l_tab1.first .. l_tab1.last LOOP
  dbms_output.put_line(l_tab1(i));
END LOOP;
END;
/

# Collection Summary

1) If the type statement conatins an INEX BY clause, the collection type is an associative array
2) If the TYPE statement contains the VARRAY keyword, the collection type is a VARRAY
3) IF the TYPE statement does not contain an INDEX BY clause or a VARRAy keyword, the collection type is a nested table
4) Only the associative array offers a choice of indexing datatypes. Nested tables as well as VARRAYS are always indexed by integer
5) When you define a VARRAy type, you specify the maximum number of elements that can be defined in a collection of that type

#Conclusion

1) You will rarely encounter a need for a VARRAY(How many times do you know in advance the maximum number of elements you will define in your collection?)
2) The associative array is the most commonly used collection type
3) But nested tables have some poweful, unique features (such as MULTISET operators) that can simplify the code you need to write to use your collection

# Triggers

Triggers are stored programs, which are automatically executed or fired when some events occur. Triggers are, in fact, written to be executed in response to any of the following events:

1) A datatype manipulation (DML) statement (DELETE, INSERT, or UPDATE)
2) A database definition (DDL) statement (CREATE, ALTER, or DROP)
3) A database operation (SERVERERROR, LOGON, LOGOFF, STARTUP, or SHUTDOWN)

Triggers could be defined on the table, view, schema, or database with which the event is associated

# Benefits of Triggers

1) Generating some derived column values automatically
2) Enforcing referential integrity
3) Event logging and storing information on table access
4) Auditing
5) Synchronous replication of tables
6) Imposing security authorizations
7) Preventing invalid transactions
  


