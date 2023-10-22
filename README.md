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


  
