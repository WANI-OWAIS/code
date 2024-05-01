------------------------------------------------------------------------------------------------------------------------------------------------------------
order

CREATE DATABASE OrderDatabase;
USE OrderDatabase;


CREATE TABLE Customer (
cust_id int PRIMARY KEY,
cname varchar(15),
city varchar(15)
);

INSERT INTO Customer VALUES(1,'Akash','Moodbidri');
INSERT INTO Customer VALUES(2,'Rakesh','Mangalore');
INSERT INTO Customer VALUES(3,'Suresh','Karkal');

CREATE TABLE C_order (
orderid int PRIMARY KEY,
odate date,
cust_id int,
ordamt int,
FOREIGN KEY (cust_id) REFERENCES Customer(cust_id) ON DELETE CASCADE ON UPDATE CASCADE
);


INSERT INTO C_order VALUES(11,'2018-02-23',1,NULL); 
INSERT INTO C_order VALUES(12,'2016-06-23',2,NULL);
INSERT INTO C_order VALUES(13,'2015-03-02',3,NULL);
INSERT INTO C_order VALUES(14,'2018-03-02',3,NULL);

CREATE TABLE Item (
item_id int PRIMARY KEY,
price int
);

INSERT INTO Item VALUES(101,40);
INSERT INTO Item VALUES(102,70);
INSERT INTO Item VALUES(103,100);

CREATE TABLE Order_item (
orderid int,
item_id int,
qty int,
PRIMARY KEY (orderid,item_id),
FOREIGN KEY(orderid) REFERENCES C_order(orderid) ON DELETE CASCADE ON UPDATE CASCADE,
FOREIGN KEY(item_id) REFERENCES Item(item_id) ON DELETE CASCADE ON UPDATE CASCADE
);

INSERT INTO Order_item VALUES(11, 101, 4);
INSERT INTO Order_item VALUES(12, 102, 2);
INSERT INTO Order_item VALUES(13, 103, 1);
INSERT INTO Order_item VALUES(12, 101, 5);
INSERT INTO Order_item VALUES(13, 101, 3);
INSERT INTO Order_item VALUES(14, 102, 1);

CREATE TABLE Warehouse (
warehouseid int PRIMARY KEY,
city varchar(20)
);

SELECT * FROM Customer;
SELECT * FROM Item;
SELECT * FROM C_order;
SELECT * FROM Order_item;

INSERT INTO Warehouse VALUES(111,'Mangalore');
INSERT INTO Warehouse VALUES(112,'Udupi');
INSERT INTO Warehouse VALUES(113,'Karkal');

CREATE TABLE Shipment (
orderid int,
warehouseid int,
ship_dt date,
PRIMARY KEY (orderid,warehouseid),
FOREIGN KEY(orderid) REFERENCES C_order(orderid) ON DELETE CASCADE ON UPDATE CASCADE,
FOREIGN KEY (warehouseid) REFERENCES Warehouse(warehouseid) ON DELETE CASCADE ON UPDATE CASCADE
);

INSERT INTO Shipment VALUES(11,111,'2018-02-24');
INSERT INTO Shipment VALUES(12,112,'2016-06-24');
INSERT INTO Shipment VALUES(13,113,'2015-03-02');
INSERT INTO Shipment VALUES(11,113,'2018-02-23');
INSERT INTO Shipment VALUES(12,113,'2016-06-23');
INSERT INTO Shipment VALUES(13,111,'2015-03-03');
INSERT INTO Shipment VALUES(14,111,'2018-03-03');

SELECT * FROM Shipment;

UPDATE C_order SET ordamt=(SELECT SUM(O.qty * T.price) FROM Order_item O, Item T 
WHERE 
 O.item_id=T.item_id AND O.orderid = 11) WHERE orderid = 11;
UPDATE C_order SET ordamt=(SELECT SUM(O.qty * T.price) FROM Order_item O, Item T WHERE 
 O.item_id=T.item_id AND O.orderid = 12) WHERE orderid = 12;
UPDATE C_order SET ordamt=(SELECT SUM(O.qty * T.price) FROM Order_item O, Item T WHERE 
 O.item_id=T.item_id AND O.orderid = 13) WHERE orderid = 13;
UPDATE C_order SET ordamt=(SELECT SUM(O.qty * T.price) FROM Order_item O, Item T WHERE 
 O.item_id=T.item_id AND O.orderid = 14) WHERE orderid = 14;


 SELECT C.cname, COUNT(CO.orderid) as 'No of orders' , AVG(CO.ordamt) as 'Average'
FROM Customer C, C_order CO 
WHERE C.cust_id=CO.cust_id 
GROUP BY C.cname, C.cust_id;

SELECT item_id, COUNT(*) AS 'No of orders', SUM(qty) AS 'Total quantity'
FROM Order_item
WHERE orderid IN (SELECT orderid FROM Shipment GROUP BY orderid HAVING COUNT(*) >=2)
GROUP BY item_id
HAVING COUNT(*) > 2;


SELECT C.cname
FROM Customer C
WHERE NOT EXISTS (
SELECT item_id
FROM Item
WHERE item_id NOT IN (
SELECT (I.item_id)
FROM Order_item I, C_order O
WHERE O.orderid=I.orderid AND O.cust_id=C.cust_id
)
);

------------------------------------------------------------------------------------------------------------------------------------------------------------
Book dealers                        Book dealers

CREATE DATABASE book_dealer;
 USE book_dealer;

CREATE TABLE Author (
authorid int PRIMARY KEY,
aname varchar(20),
city varchar(20),
country varchar(20)
);
INSERT INTO Author VALUES(111,'Elmasri','Houston','Canada');
INSERT INTO Author VALUES(112,'Renuka','Delhi','India');
INSERT INTO Author VALUES(113,'Herbert','California','USA');
CREATE TABLE Publisher (
pubid int PRIMARY KEY,
pname varchar(20),
city varchar(20),
country varchar(20)
);

INSERT INTO Publisher VALUES(201,'McGRAW','Delhi','India');
INSERT INTO Publisher VALUES(202,'Pearson','Bangalore','India');

CREATE TABLE Category (
catid int PRIMARY KEY,
description varchar(30),
);
INSERT INTO Category VALUES(1,'Computer Science');
INSERT INTO Category VALUES(2,'Popular Novels');
CREATE TABLE Catalogue (
bookid int PRIMARY KEY,
title varchar(20),
authorid int,
pubid int,
catid int, 
yr int,
price int,
FOREIGN KEY(pubid) REFERENCES Publisher(pubid) ON DELETE CASCADE ON UPDATE CASCADE,
FOREIGN KEY(authorid) REFERENCES Author(authorid) ON DELETE CASCADE ON UPDATE 
CASCADE,
FOREIGN KEY(catid) REFERENCES Category(catid) ON DELETE CASCADE ON UPDATE CASCADE
);

INSERT INTO Catalogue VALUES(301,'Operating Systems',111,201,1,2008,300);
INSERT INTO Catalogue VALUES(302,'Unix Programming',112,201,1,2007,400);
INSERT INTO Catalogue VALUES(303,'Village Life',113,202,2,2016,100);

CREATE TABLE Order_details (
ordno int ,
bookid int,
qty int,
PRIMARY KEY (ordno,bookid),
FOREIGN KEY(bookid) REFERENCES Catalogue(bookid) ON DELETE CASCADE ON UPDATE 
CASCADE
);
SELECT * FROM Author;
SELECT * FROM Publisher;
SELECT * FROM Category;
SELECT * FROM Catalogue;


INSERT INTO Order_details VALUES(1,301,10);
INSERT INTO Order_details VALUES(1,302,6);
INSERT INTO Order_details VALUES(2,301,5);
INSERT INTO Order_details VALUES(2,303,1);
INSERT INTO Order_details VALUES(3,303,2);

SELECT A.authorid, A.aname, C.bookid, SUM(O.qty) AS 'Quantity'
FROM Author A, Catalogue C, Order_details O
WHERE A.authorid=C.authorid AND C.bookid=O.bookid
GROUP BY A.authorid, A.aname,C.bookid
HAVING SUM(O.qty) >= ALL (
SELECT SUM(qty)
FROM Order_details
GROUP BY bookid
);

UPDATE Catalogue SET price=price+(price*0.1) WHERE pubid IN (
SELECT pubid FROM Publisher WHERE pname='Pearson');

SELECT * FROM Catalogue;

SELECT COUNT(ordno) AS 'No. of orders',bookid 
FROM Order_details
GROUP BY bookid
HAVING SUM(qty) <=ALL (
SELECT SUM(qty)
FROM Order_details
GROUP BY bookid
);
------------------------------------------------------------------------------------------------------------------------------------------------------------
Student -Book Adaption                   Student -Book Adaption
------------------------------------------------------------------------------------------------------------------------------------------------------------


CREATE DATABASE StudentData;
 USE StudentData;

CREATE TABLE Student (
reg_no varchar(10) PRIMARY KEY,
fname varchar(15),
major varchar (20),
bdate date
);

INSERT INTO Student VALUES ('1','Akash','Academic','1999-11-22');
INSERT INTO Student VALUES ('2','Prakash','Academic','1999-03-04');
INSERT INTO Student VALUES ('3','Rajesh','Academic','2000-01-16');

CREATE TABLE Course (
course_id int PRIMARY KEY,
cname varchar(20),
dept varchar (20)
);
SELECT * FROM Student;

INSERT INTO Course VALUES (101,'RDBMS','CSE');
INSERT INTO Course VALUES (102,'Compiler Design','CSE');
INSERT INTO Course VALUES (103,'JAVA Programming','CSE');
INSERT INTO Course VALUES (104,'Signal Processing','ENC');
INSERT INTO Course VALUES (105,'Digital signals','ENC');

CREATE TABLE Enroll (
reg_no varchar(10),
course_id int,
sem int ,
marks int,
PRIMARY KEY(reg_no,course_id,sem),
FOREIGN KEY(reg_no) REFERENCES Student(reg_no) ON DELETE CASCADE ON UPDATE CASCADE,
FOREIGN KEY(course_id) REFERENCES Course(course_id) ON DELETE CASCADE ON UPDATE CASCADE
);

INSERT INTO Enroll VALUES (1,101,5,76);
INSERT INTO Enroll VALUES (1,102,6,89);
INSERT INTO Enroll VALUES (2,102,6,65);
INSERT INTO Enroll VALUES (2,103,7,49);
INSERT INTO Enroll VALUES (3,104,6,80);
INSERT INTO Enroll VALUES (3,105,7,100);

CREATE TABLE Textbook (
bookISBN int PRIMARY KEY,
title varchar(50),
publisher varchar(20),
author varchar(20)
);
INSERT INTO Textbook VALUES (201,'Fundamentals of DBMS','McGraw','Navathe');
INSERT INTO Textbook VALUES (202,'Database Design','McGraw','Raghu Rama');
INSERT INTO Textbook VALUES (203,'Database Concepts','Pearson','Rajagopal');
INSERT INTO Textbook VALUES (204,'Compiler design','Pearson','Ulman');
INSERT INTO Textbook VALUES (205,'JAVA complete Reference','McGraw','Balaguru');
INSERT INTO Textbook VALUES (206,'Signal Processing','McGraw','Nithin');

CREATE TABLE Book_adaption (
course_id int,
sem int,
bookISBN int,
PRIMARY KEY(course_id, sem, bookISBN),
FOREIGN KEY(course_id) REFERENCES Course(course_id) ON DELETE CASCADE ON UPDATE 
CASCADE,
 FOREIGN KEY(bookISBN) REFERENCES Textbook(bookISBN) ON DELETE CASCADE ON UPDATE 
CASCADE
);

INSERT INTO Book_adaption VALUES (101,5,201);
INSERT INTO Book_adaption VALUES (101,7,202);
INSERT INTO Book_adaption VALUES (101,7,203);
INSERT INTO Book_adaption VALUES (102,6,204);
INSERT INTO Book_adaption VALUES (103,7,205);
INSERT INTO Book_adaption VALUES (104,6,206);
INSERT INTO Book_adaption VALUES (105,7,206);

SELECT DISTINCT(T.bookISBN), T.title, C.course_id, C.cname
FROM Textbook T, Course C, Book_adaption B
WHERE B.bookISBN=T.bookISBN AND C.course_id=B.course_id AND C.dept='CSE'
AND C.course_id IN (
SELECT course_id
FROM Book_adaption
GROUP BY course_id
HAVING COUNT(DISTINCT(bookISBN))>2
)
ORDER BY T.title;

SELECT DISTINCT(C.dept)
FROM Course C
WHERE NOT EXISTS (
                  SELECT bookISBN
                  FROM Book_adaption
                  WHERE course_id IN (
                  SELECT course_id
FROM Course
WHERE dept=C.dept
)
AND bookISBN NOT IN (
SELECT T.bookISBN 
FROM TEXTBOOK T
WHERE T.publisher='McGraw'
)
);

SELECT DISTINCT(T.bookISBN), T.title, C.dept
FROM Textbook T,Book_adaption B, Course C
WHERE T.bookISBN=B.bookISBN AND B.course_id=C.course_id AND C.dept IN (
SELECT dept
FROM Course C1, Enroll E1
WHERE C1.course_id=E1.course_id
GROUP BY C1.dept
HAVING COUNT(DISTINCT(E1.reg_no)) > = ALL (
SELECT COUNT(DISTINCT(reg_no))
FROM Course C2, Enroll E2
WHERE C2.course_id=E2.course_id
GROUP BY C2.dept
) 
);
------------------------------------------------------------------------------------------------------------------------------------------------------------
  Bank Database                          Bank Database
CREATE DATABASE bank;
 USE bank;
------------------------------------------------------------------------------------------------------------------------------------------------------------

CREATE TABLE Branch (
bname varchar(20) PRIMARY KEY,
bcity varchar(20),
assets real
);
INSERT INTO Branch VALUES('Synd MNG','Mangalore',200000);
INSERT INTO Branch VALUES('CORP MNG','Mangalore',700000);
INSERT INTO Branch VALUES('Canera MNG','Mangalore',500000);
INSERT INTO Branch VALUES('SBI UDP','Udupi',600000);
INSERT INTO Branch VALUES('SYND NITTE','Karkal',800000);

CREATE TABLE Account (
accno int PRIMARY KEY,
bname varchar(20),
balance real,
FOREIGN KEY(bname) REFERENCES Branch(bname) ON DELETE CASCADE ON UPDATE 
CASCADE
);
INSERT INTO Account VALUES(1,'Synd MNG',20000);
INSERT INTO Account VALUES(2,'SBI UDP',300);
INSERT INTO Account VALUES(3,'SYND NITTE',5000);
INSERT INTO Account VALUES(4,'SYND NITTE',7000);
INSERT INTO Account VALUES(5,'SYND NITTE',30000);
INSERT INTO Account VALUES(6,'Synd MNG',9000);
INSERT INTO Account VALUES(7,'CORP MNG',11000);
CREATE TABLE Customer (
cname varchar(20) PRIMARY KEY,
cstreet varchar(10),
ccity varchar(20)
);
SELECT * FROM Branch;
SELECT * FROM Account;

INSERT INTO Customer VALUES('Ashok','S1','Udupi');
INSERT INTO Customer VALUES('Rajesh','S3','Mangalore');
INSERT INTO Customer VALUES('Sukhesh','S5','Karkala');
CREATE TABLE Depositor 
(
cname varchar(20),
accno int,
PRIMARY KEY(cname,accno),
FOREIGN KEY(cname) REFERENCES Customer(cname) ON DELETE CASCADE ON UPDATE CASCADE,
FOREIGN KEY(accno) REFERENCES Account(accno) ON DELETE CASCADE ON UPDATE CASCADE
);
INSERT INTO Depositor VALUES('Ashok',1);
INSERT INTO Depositor VALUES('Ashok',2);
INSERT INTO Depositor VALUES('Ashok',3);
INSERT INTO Depositor VALUES('Rajesh',4);
INSERT INTO Depositor VALUES('Rajesh',5);
INSERT INTO Depositor VALUES('Sukhesh',6);
INSERT INTO Depositor VALUES('Sukhesh',7);

CREATE TABLE Loan 
(
lno int PRIMARY KEY,
bname varchar(20),
amt real,
FOREIGN KEY(bname) REFERENCES Branch(bname) ON DELETE CASCADE ON UPDATE CASCADE
);
INSERT INTO Loan VALUES(123,'CORP MNG',10000);
CREATE TABLE Borrower 
(
cname varchar(20),
lno int,
PRIMARY KEY(cname,lno),
FOREIGN KEY(cname) REFERENCES Customer(cname) ON DELETE CASCADE ON UPDATE CASCADE,
FOREIGN KEY(lno)REFERENCES Loan(lno) ON DELETE CASCADE ON UPDATE CASCADE
);
INSERT INTO Borrower VALUES('Ashok',123);

SELECT C.cname 
FROM Customer C 
WHERE NOT EXISTS (
                  SELECT B.bname 
                  FROM Branch B 
                  WHERE B.bcity='Karkal' AND B.bname NOT IN (
SELECT A.bname                  
FROM Account A, Depositor D
WHERE A.accno=D.accno AND A.bname=B.bname AND D.cname=C.cname
GROUP BY A.bname HAVING COUNT(*)>=2
)
);

SELECT C.cname 
FROM Customer C 
WHERE NOT EXISTS (
                  SELECT DISTINCT(B.bcity) 
                  FROM Branch B 
                  WHERE B.bcity NOT IN (
SELECT B1.bcity 
FROM Account A, Depositor D, Branch B1 
WHERE A.accno=D.accno AND A.bname=B1.bname AND D.cname=C.cname 
GROUP BY B1.bcity HAVING COUNT(*)>=1 
)
);

SELECT C.cname 
FROM Customer C 
WHERE EXISTS (
               SELECT COUNT(B.bname)
               FROM Branch B, Account A, Depositor D
               WHERE B.bcity='Mangalore' AND A.accno=D.accno AND A.bname=B.bname AND
               D.cname=C.cname
               GROUP BY B.bcity HAVING COUNT(*)>=2
              );
------------------------------------------------------------------------------------------------------------------------------------------------------------
  Accident                           Accident
------------------------------------------------------------------------------------------------------------------------------------------------------------

CREATE DATABASE insurance;
 USE insurance;
Creating Tables and data insertion :
CREATE TABLE Person (
Driver_id varchar(10) PRIMARY KEY,
Fname varchar(30),
Address varchar(100)
);
INSERT INTO Person VALUES('1','Akash','Jyothi, Mangalore');
INSERT INTO Person VALUES('2','Suresh','Kadri, Mangalore');
INSERT INTO Person VALUES('3','Ramesh','Udyawar, Udupi');
CREATE TABLE Car (
Reg_no varchar(10) PRIMARY KEY,
model varchar(10),
cyear int
);
INSERT INTO Car VALUES('101','Alto 800',2012);
INSERT INTO Car VALUES('102','WagonR',2015);
INSERT INTO Car VALUES('103','Hyundai',2017);
INSERT INTO Car VALUES('104','Ford',2016);
INSERT INTO Car VALUES('105','Audi',2017);
CREATE TABLE Accident 
(
Rept_no int PRIMARY KEY,
Acc_date date,
Location varchar(10)
);
INSERT INTO Accident VALUES('11','12-FEB-2018','Udupi');
INSERT INTO Accident VALUES('12','31-OCT-2014','Mangalore');
INSERT INTO Accident VALUES('13','17-NOV-2018','Karkal');
CREATE TABLE Owns 
(
Driver_id varchar(10),
Reg_no varchar(10) UNIQUE,
PRIMARY KEY(Driver_id,Reg_no),
FOREIGN KEY(Driver_id) REFERENCES Person(Driver_id) ON DELETE CASCADE ON UPDATE CASCADE,
FOREIGN KEY(Reg_no) REFERENCES Car(Reg_no) ON DELETE CASCADE ON UPDATE CASCADE
);
SELECT * FROM Person;
SELECT * FROM Car;
SELECT * FROM Accident;

INSERT INTO Owns VALUES('1','102');
INSERT INTO Owns VALUES('1','103');
INSERT INTO Owns VALUES('3','101');
INSERT INTO Owns VALUES('2','104');
INSERT INTO Owns VALUES('2','105');
CREATE TABLE Participated (
Driver_id varchar(10),
Reg_no varchar(10),
Rept_no int UNIQUE,
damount float,
PRIMARY KEY(Driver_id,Reg_no,Rept_no),
FOREIGN KEY(Driver_id) REFERENCES Person(Driver_id) ON DELETE CASCADE ON UPDATE CASCADE,
FOREIGN KEY(Reg_no) REFERENCES Car(Reg_no) ON DELETE CASCADE ON UPDATE CASCADE,
FOREIGN KEY(Rept_no) REFERENCES Accident(Rept_no) ON DELETE CASCADE ON UPDATE CASCADE,
FOREIGN KEY(Driver_id,Reg_no) REFERENCES Owns(Driver_id,Reg_no)
);
INSERT INTO Participated VALUES('1','103','11',50500.00);
INSERT INTO Participated VALUES('1','102','12',20300.00);
INSERT INTO Participated VALUES('2','104','13',304899.00);

SELECT COUNT(DISTINCT(Driver_id)) AS 'No. of Owners
FROM Participated P, Accident A 
WHERE P.Rept_no=A.Rept_no AND YEAR(Acc_date)=2018;

SELECT COUNT(Pr.Rept_no) AS 'No. of Accidents'
FROM Participated Pr, Person P
WHERE P.Driver_id = Pr.Driver_id AND P.Fname='Suresh';

UPDATE PARTICIPATED SET damount=30000 WHERE reg_no='102' AND rept_no = '12';
------------------------------------------------------------------------------------------------------------------------------------------------------------
