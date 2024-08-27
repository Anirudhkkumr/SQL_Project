# SQL_Project
Library Management System_SQL
##
CREATE database LIBRARY;
USE LIBRARY;
CREATE TABLE BRANCH(
Branch_no  INT PRIMARY KEY,
Manager_Id INT,
Branch_address VARCHAR(300),
Contact_no varchar(20)
);
INSERT INTO BRANCH (Branch_no, Manager_Id, Branch_address, Contact_no)
VALUES
(1, 201, '12 MG Road, Bengaluru', '08012345678'),
(2, 202, '56 Anna Salai, Chennai', '04487654321'),
(3, 203, '89 Park Street, Kolkata', '03311223344');

CREATE TABLE EMPLOYEE(
Emp_Id INT PRIMARY KEY,  
Emp_name VARCHAR(25),
Position VARCHAR(100),
Salary DECIMAL(10,2),
Branch_no INT,
	FOREIGN KEY (Branch_no) REFERENCES Branch(Branch_no));
    
INSERT INTO EMPLOYEE (Emp_Id, Emp_name, Position, Salary, Branch_no)
VALUES
(101, 'Ravi Kumar', 'Manager', 60000, 1),
(102, 'Priya Sharma', 'Assistant Manager', 50000, 1),
(103, 'Amit Singh', 'Clerk', 40000, 2),
(104, 'Suman Gupta', 'Manager', 60000, 2),
(105, 'Neha Verma', 'Clerk', 40000, 3);
SELECT * FROM EMPLOYEE;

CREATE TABLE BOOKS(
ISBN INT PRIMARY KEY,
Book_title VARCHAR(300), 
Category VARCHAR(100),
Rental_Price DECIMAL(10,2),
Status VARCHAR(5),
Author VARCHAR(100),
Publisher VARCHAR(100)
);

INSERT INTO BOOKS (ISBN, Book_title, Category, Rental_Price, Status, Author, Publisher)
VALUES
(9788190, 'The White Tiger', 'Fiction', 200.00, 'Yes', 'Aravind Adiga', 'HarperCollins India'),
(9780153, 'The Immortals of Meluha', 'Mythology', 250.00, 'No', 'Amish Tripathi', 'Westland'),
(9780143, 'Midnight\'s Children', 'Historical', 300.00, 'Yes', 'Salman Rushdie', 'Vintage Books'),
(9780670, 'The Ministry of Utmost Happiness', 'Fiction', 350.00, 'Yes', 'Arundhati Roy', 'Penguin India'),
(9780163, 'Malgudi Days', 'Short Stories', 150.00, 'No', 'R.K. Narayan', 'Penguin India');
INSERT INTO Books (ISBN, Book_title, Category, Rental_Price, Status, Author, Publisher)
VALUES 
('978812', 'Ancient India: A History', 'History', 220.00, 'yes', 'Bipin Chandra', 'Vikas Publishing House'),
('978813', 'History of the Indian Freedom Struggle', 'History', 300.00, 'yes', 'K. K. Aziz', 'Oxford University Press');

CREATE TABLE CUSTOMER(
Customer_Id INT PRIMARY KEY,  
Customer_name VARCHAR(100),
Customer_address VARCHAR(200),
Reg_date DATE
);

INSERT INTO CUSTOMER (Customer_Id, Customer_name, Customer_address, Reg_date)
VALUES
(101, 'Rahul Mehta', '34, Indiranagar, Bengaluru', '2021-12-15'),
(102, 'Anita Desai', '78, T. Nagar, Chennai', '2023-02-20'),
(103, 'Vikas Khanna', '23, Salt Lake, Kolkata', '2022-11-05'),
(104, 'Sunita Rao', '56, Vashi, Navi Mumbai', '2020-06-18'),
(105, 'Kiran Bedi', '11, Connaught Place, New Delhi', '2023-05-12');

CREATE TABLE ISSUESTATUS(
Issue_Id INT PRIMARY KEY,
Issued_cust INT,
	FOREIGN KEY(Issued_cust) REFERENCES CUSTOMER(Customer_Id),
Issued_book_name VARCHAR(200),
Issue_date DATE,
Isbn_book INT,
	FOREIGN KEY(Isbn_book) REFERENCES BOOKS(ISBN)
);

INSERT INTO ISSUESTATUS (Issue_Id, Issued_cust, Issued_book_name, Issue_date, Isbn_book)
VALUES
(1, 101, 'The White Tiger', '2023-08-01', 9788190),
(2, 102, 'The Immortals of Meluha', '2023-08-02', 9780153),
(3, 103, 'Midnight\'s Children', '2023-08-03', 9780143),
(4, 104, 'The Ministry of Utmost Happiness', '2023-08-04', 9780670),
(5, 105, 'Malgudi Days', '2023-08-05', 9780163);

CREATE TABLE RETURN_STATUS(
Return_Id INT PRIMARY KEY,
Return_cust INT,
	FOREIGN KEY(Return_cust) REFERENCES CUSTOMER(Customer_Id),
Return_book_name VARCHAR(200),  
Return_date DATE,
Isbn_book2 INT,
	FOREIGN KEY(Isbn_book2) REFERENCES BOOKS(ISBN)
);

INSERT INTO RETURN_STATUS (Return_Id, Return_cust, Return_book_name, Return_date, Isbn_book2)
VALUES 
(1, 101, 'The White Tiger', '2023-08-01', 9788190),
(2, 102, 'The Immortals of Meluha', '2023-08-02', 9780153),
(3, 103, 'Midnight\'s Children', '2023-08-03', 9780143),
(4, 104, 'The Ministry of Utmost Happiness', '2023-08-04', 9780670),
(5, 105, 'Malgudi Days', '2023-08-05', 9780163);

SELECT Book_title, Category, Rental_Price 
FROM BOOKS WHERE STATUS ='YES';

SELECT Emp_name, Salary FROM EMPLOYEE ORDER BY Salary DESC;

SELECT BOOKS.Book_title, CUSTOMER.Customer_name
FROM ISSUESTATUS
JOIN BOOKS ON ISSUESTATUS.Isbn_book = BOOKS.ISBN
JOIN CUSTOMER ON ISSUESTATUS.Issued_cust = CUSTOMER.Customer_Id; 

SELECT Category, COUNT(*) AS TOTALBOOKS
FROM BOOKS GROUP BY Category;

SELECT Emp_name, Position
FROM EMPLOYEE WHERE Salary>50000;

SELECT Customer_name FROM CUSTOMER
WHERE Reg_date < '2022-01-01' 
AND Customer_Id NOT IN (SELECT Issued_cust FROM IssueStatus);

SELECT BRANCH.Branch_no, COUNT(EMPLOYEE.Emp_id) AS TOTAL_EMPLOYEE FROM BRANCH
LEFT JOIN EMPLOYEE ON BRANCH.Branch_no= EMPLOYEE.Branch_no
GROUP BY BRANCH.Branch_no;

SELECT CUSTOMER.Customer_name FROM ISSUESTATUS
JOIN CUSTOMER ON ISSUESTATUS.Issued_cust = CUSTOMER.Customer_id
WHERE MONTH(ISSUESTATUS.Issue_date) = 6
  AND YEAR(ISSUESTATUS.Issue_date)=2023;
  
SELECT BOOKS.Book_title FROM BOOKS
WHERE BOOKS.Book_title LIKE '%History%';

SELECT BRANCH.Branch_no, COUNT(EMPLOYEE.Emp_id) FROM BRANCH
JOIN EMPLOYEE ON BRANCH.Branch_no = EMPLOYEE.Branch_no
GROUP BY BRANCH.Branch_no HAVING COUNT(EMPLOYEE.Emp_id)>5;

SELECT EMPLOYEE.Emp_id, BRANCH.Branch_address FROM EMPLOYEE
JOIN BRANCH ON EMPLOYEE.Branch_no = BRANCH.Branch_no
WHERE EMPLOYEE.Position = 'Manager';

SELECT DISTINCT CUSTOMER.Customer_name
FROM ISSUESTATUS
JOIN BOOKS ON ISSUESTATUS.Isbn_book = BOOKS.ISBN
JOIN CUSTOMER ON ISSUESTATUS.Issued_cust = CUSTOMER.Customer_Id
WHERE BOOKS.Rental_Price > 25;
##
