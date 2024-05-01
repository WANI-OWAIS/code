1. Consider the Insurance database given below. 
PERSON (driver – id #: String, name: string, address: string)
CAR (regno: string, model: string, year: int)
ACCIDENT (report-number: int, accd-date: date, location: string)
OWNS (driver-id #: string, regno: string)
PARTICIPATED (driver-id: string, Regno: string, report-number: int, damage amount: int)
create database insurance
use insurance
create table PERSON(
driverid varchar(10),
pname char(15) not null,
paddress varchar(30),
primary key(driverid)
)
insert into PERSON values(111,'John Smith','SP road,Bangalore-12')
insert into PERSON values(112,'Ramesh Babu','KP Nagar,Udupi-13')
insert into PERSON values(113,'Raju SK ','KS circle,Mangalore-12')
insert into PERSON values(114,'Ramesh Babu','AS road,Bangalore-14')
insert into PERSON values(115,'Alica Wallace','SS road,Karkala-16')
select * from PERSON
create table CAR(
regno varchar(10),
model varchar(10) not null,
cyear int,
primary key(regno)
)
insert into CAR values('AP-10','Creta',2005)
insert into CAR values('KA-19','XUV 300',2001)
insert into CAR values('KA-13','Venue',2015)
insert into CAR values('AP-05','Creta',2001)
insert into CAR values('KA-12','Safari',2023)
select * from CAR
create table ACCIDENT(
reportno varchar(10),
accdate date,
alocation varchar(15),
primary key(reportno)
)
insert into ACCIDENT values(01,'1998-05-06','manipal')
insert into ACCIDENT values(02,'1998-08-03','mangalore')
insert into ACCIDENT values(03,'2000-04-05','udupi')
insert into ACCIDENT values(04,'1999-07-10','karkala')
insert into ACCIDENT values(05,'1989-11-21','manipal')
select * from ACCIDENT
create table OWNS(
driverid varchar(10),
regno varchar(10),
primary key (driverid,regno),
foreign key(driverid) references PERSON(driverid) on delete cascade on update cascade,
foreign key(regno) references CAR(regno) on delete cascade on update cascade,
unique(regno)
)
insert into OWNS values(111,'KA-13')
insert into OWNS values(112,'KA-12')
insert into OWNS values(114,'KA-19')
insert into OWNS values(111,'AP-05')
insert into OWNS values(111,'AP-10')
select * from PERSON
select * from CAR
SELECT * FROM OWNS
create table PARTICIPATED(
driverid varchar(10),
regno varchar(10),
reportno varchar(10),
damageamt int,
primary key(driverid,regno,reportno),
foreign key(driverid) references PERSON(driverid) on delete cascade on update
cascade,
foreign key(regno) references CAR(regno) on delete cascade on update cascade,
foreign key(reportno) references ACCIDENT(reportno) on delete cascade on update
cascade,
foreign key(driverid,regno) references OWNS(driverid,regno),
unique(reportno)
)
insert into PARTICIPATED values(111,'KA-13',1,5000)
insert into PARTICIPATED values(114,'KA-19',2,2000)
insert into PARTICIPATED values(111,'AP-05',3,6500)
insert into PARTICIPATED values(112,'KA-12',4,4850)
insert into PARTICIPATED values(111,'AP-10',5,7852)
SELECT * FROM PARTICIPATED
/*1.find the total number of people who owned cars that were invovled in accidents in 
1998*/
select count (distinct P.driverid) as NO_OF_PEOPLE
from ACCIDENT A, PARTICIPATED P
where A.reportno=P.reportno
and year(A.accdate)='1998'
select * from ACCIDENT
select * from PARTICIPATED
/*2.find the number of accidents in the car belonging to 'john smith' were involved*/
select count(reportno) as NO_OF_ACCIDENTS
from PARTICIPATED
where driverid in ( select driverid from PERSON where pname='John Smith')
/*3. Update the damage amount for the car with register number 'ka-12' in the accident 
with the report number 1 to $3000*/
update PARTICIPATED set damageamt=3000
where reportno=1 and regno='KA-13'
select * from PARTICIPATED






2. Consider the following database of student enrollment in courses & books adopted for each course:
STUDENT (regno: string, name: string, major: string, bdate: date)
COURSE (course #: int, cname: string, dept: string)
TEXT (book-ISBN: int, book-title: string, publisher: string, author: string)
BOOK _ ADOPTION (course#: int, sem: int, book-ISBN: int)
ENROLL (regno: string, course#: int, sem: int marks: int)
create database student
use student
create table STUDENT (
regno varchar(10),
fname char(15),
major char (20),
bdate datetime
primary key(regno)
 )
insert into STUDENT values ('111','ravi','academic','1989-11-09')
insert into STUDENT values ('112','sudha','academic','1979-07-04')
insert into STUDENT values ('113','kumar','academic','1979-01-06')
insert into STUDENT values ('114','raju','academic','1999-10-02')
insert into STUDENT values ('115','hemanth','academic','1988-11-04')
select * from STUDENT
create table COURSE (
course int,
cname varchar(15),
dept char (20),
primary key(course)
 )
insert into COURSE values (1,'DBMS','CS')
insert into COURSE values (2,'COMPILER','CS')
insert into COURSE values (3,'JAVA','CS')
insert into COURSE values (4,'SIG PROCESSING','ENC')
insert into COURSE values (5,'DIGTAL CIRCUITS','ENC')
insert into COURSE values (6,'MACHINE DESIGN','MECH')
insert into COURSE values (7,'THEMODYNAICS','MECH')
insert into COURSE values (8,'AUTOCAD','MECH')
select * from COURSE 
create table TEXTBOOK (
bookISBN int,
title varchar(50),
publisher varchar(20),
author char(20),
primary key (bookISBN)
 )
insert into TEXTBOOK values (201,'Fundamentals of DBMS','McGraw','NAVATHE')
insert into TEXTBOOK values (202,'Database Design','McGraw','Raghu Rama')
insert into TEXTBOOK values (203,'Compiler design','Pearson','Ulman')
insert into TEXTBOOK values (204,'JAVA complete Reference','McGraw','BALAGURU')
insert into TEXTBOOK values (205,'Singals and Fundumentals','McGraw','NITHIN')
insert into TEXTBOOK values (206,'Machine Theory','McGraw','Ragavan')
insert into TEXTBOOK values (208,'Circuit design','McGraw','Rajkamal')
insert into TEXTBOOK values (207,'Thermodynamics','McGraw','Alfred')
insert into TEXTBOOK values (209,'Electronic Circuits','McGraw','Alfred')
insert into TEXTBOOK values (210,'Circuits Theory','McGraw','Alfred')
select * from TEXTBOOK
create table BOOK_ADAPTION (
course int,
sem int,
bookISBN int,
primary key(course, sem,bookISBN),
foreign key(course) references COURSE(course) on delete cascade
on update cascade,
foreign key(bookISBN) references TEXTBOOK (bookISBN) on delete
cascade on update cascade,
 )
insert into BOOK_ADAPTION values (1,5,201)
insert into BOOK_ADAPTION values (1,7,202)
insert into BOOK_ADAPTION values (2,5,203)
insert into BOOK_ADAPTION values (2,6,203)
insert into BOOK_ADAPTION values (3,7,204)
insert into BOOK_ADAPTION values (4,3,205)
insert into BOOK_ADAPTION values (4,5,209)
insert into BOOK_ADAPTION values (5,5,210)
insert into BOOK_ADAPTION values (5,6,205)
insert into BOOK_ADAPTION values (5,2,208)
insert into BOOK_ADAPTION values (6,7,206)
insert into BOOK_ADAPTION values (7,3,207)
insert into BOOK_ADAPTION values (8,3,207)
select * from BOOK_ADAPTION
create table ENROLL (
regno varchar(10),
course int,
sem int ,
marks int,
primary key(regno,course,sem),
foreign key(regno) references STUDENT(regno)on delete cascade on
update cascade,
foreign key(course) references COURSE(course)on delete cascade on
update cascade,
 )
insert into ENROLL values (111,1,5,59)
insert into ENROLL values (111,2,5,70)
insert into ENROLL values (111,3,5,75)
insert into ENROLL values (112,1,5,49)
insert into ENROLL values (113,2,5,80)
insert into ENROLL values (114,3,7,79)
insert into ENROLL values (115,4,3,79)
select * from ENROLL
select * from ENROLL
select * from BOOK_ADAPTION
select * from STUDENT
select * from COURSE
select * from TEXTBOOK
 /* 1. Produce a list of text books (include Course #, Book-ISBN,Book-title) in the 
alphabetical order for courses offered by 
 the ‘CS’ department that use more than two books.*/
 select T.bookISBN,T.title,C.course,C.cname 
 from TEXTBOOK T,COURSE C,BOOK_ADAPTION B
 where T.bookISBN=B.bookISBN and B.course=C.course
 and C.dept='CS'
 and C.course in(select course 
 from BOOK_ADAPTION
 group by course having count(*)>=2)
 order by T.title;
 /* 2. List any department that has all its adopted books published by a specific 
publisher.*/
 select distinct(C.dept)
 from COURSE C
 where not exists (select bookISBN 
 from BOOK_ADAPTION where course in (select course from COURSE 
where dept=C.dept)and bookISBN not in (select bookISBN from TEXTBOOK where
publisher='McGraw')
)
 /* 3. List the bookISBNs and book titles of the department that has maximum number 
of students*/
 Select T.bookISBN,T.title
 from TEXTBOOK T,COURSE C,BOOK_ADAPTION B
 where B.course=C.course and T.bookISBN=B.bookISBN
 and C.dept in (select C.dept from COURSE C,ENROLL E
 where C.course=E.course
 group by C.dept
 having count(distinct E.regno)>=ALL
 (select COUNT(distinct F.regno)
 from ENROLL F, COURSE D
 where F.course=D.course
 group by D.dept)

 );  


 
3.The following tables are maintained by a book dealer:
AUTHOR (author-id: int, name: string, city: string, country: string)
PUBLISHER (publisher-id: int, name: string, city: string, country: string)
CATALOG (book-id: int, title: string, author-id: int, publisher-id: int, category-id: int, year: int, price: 
int)
CATEGORY (category-id: int, description: string)
ORDER-DETAILS (order-no: int, book-id: int, quantity: int)
create database bookshop
use bookshop
create table AUTHOR
(
authorid int primary key,
aname varchar(20),
city varchar(20),
country varchar(20)
)
insert into AUTHOR values(110,'Elmasri','Houston','Canada')
insert into AUTHOR values(111,'sebesta','mangalore','India')
insert into AUTHOR values(112,'Elmasri','Houston','Canada')
insert into AUTHOR values(113,'Bharath K','Bangalore','India')
insert into AUTHOR values(114,'Willy Z','California','USA')
insert into AUTHOR values(115,'Salma','Dakha','Bangladesh')
create table PUBLISHER
(
pubid int primary key,
pname varchar(20),
city varchar(20),
country varchar(20)
)
insert into PUBLISHER values(201,'McGRAW','mangalore','India')
insert into PUBLISHER values(202,'Pearson','Bangalore','India')
insert into PUBLISHER values(203,'GKP','Bangalore','India')
insert into PUBLISHER values(204,'MediTech','Delhi','India')
insert into PUBLISHER values(205,'Sun','Ahmadbad','India')
create table CATEGORY
(
catid int primary key ,
 descript varchar(30),
)
insert into CATEGORY values(1,'All children Books')
insert into CATEGORY values(2,'Cooking Books')
insert into CATEGORY values(3,'Popular Novels')
insert into CATEGORY values(4,'Small Story Books')
insert into CATEGORY values(5,'Medical Books')
create table CATALOGUE
(
bookid int primary key,
title varchar(20),
pubid int,
authorid int,
catid int,
yr int,
price int,
foreign key(pubid) references PUBLISHER(pubid) on delete cascade
on update cascade,
foreign key(authorid) references AUTHOR(authorid) on delete
cascade on update cascade,
foreign key(catid) references CATEGORY(catid) on delete cascade
on update cascade
)
insert into CATALOGUE values(301,'Panchatantra',201,111,1,2000,300)
insert into CATALOGUE values(302,'Vegetables',202,111,2,2000,400)
insert into CATALOGUE values(303,'Yogasana',203,112,5,2002,600)
insert into CATALOGUE values(304,'Stories of Village',204,113,4,2005,100)
insert into CATALOGUE values(305,'Triangle',205,114,3,2008,1000)
insert into CATALOGUE values(306,'Naughtiest Girl',201,110,3,2007,1500)
insert into CATALOGUE values(307,'Cookery',205,115,2,2006,100)
select * from CATALOGUE
create table ORDER_DET
(
ordno int ,
bookid int,
qty int,
primary key (ordno,bookid),
foreign key(bookid) references CATALOGUE(bookid) on delete
cascade on update cascade,
)
insert into ORDER_DET values(1,301,10)
insert into ORDER_DET values(1,302,6)
insert into ORDER_DET values(1,307,23)
insert into ORDER_DET values(2,301,15)
insert into ORDER_DET values(2,304,11)
insert into ORDER_DET values(3,304,15)
insert into ORDER_DET values(4,301,3)
insert into ORDER_DET values(4,305,8)
insert into ORDER_DET values(5,303,20)
insert into ORDER_DET values(5,306,6)
insert into ORDER_DET values(5,305,7)
select * from AUTHOR
select * from CATALOGUE
select * from CATEGORY
select * from ORDER_DET
select * from PUBLISHER
/*1. Find the author of the book which has maximum sales.*/
select A.authorid ,A.aname ,A.city ,C.bookid,
sum(O.qty) as QTY_SUM 
from author A, CATALOGUE C,order_det O 
where A.authorid = C.authorid 
and C.bookid = O.bookid 
group by A.authorid, A.aname,A.city,C.bookid
having sum(qty) >= all (select sum(qty)from order_det 
 group by bookid)
/*2. Increase the price of the books published by a specific publisher by 10%*/
update CATALOGUE set price = price * 1.1 
where pubid in ( select pubid from publisher where pname ='Pearson')
select * from CATALOGUE
/*3. Find the number of orders for the book that has minimum sales.*/
select * from ORDER_DET
select ordno, bookid, sum(qty) as Qty_ordered
from ORDER_DET
group by ordno,bookid
having sum(qty)<ALL(select sum(qty)from ORDER_DET
 group by bookid)



 
4.Consider the following relations for an order processing database application in a company:
CUSTOMER (cust #: int, cname: string, city: string)
ORDER (order #: int, odate: date, cust #: int, ord-Amt: int)
ORDER – ITEM (order #: int, item #: int, qty: int)
ITEM (item #: int, unit price: int)
SHIPMENT (order #: int, warehouse#: int, ship-date: date)
WAREHOUSE (warehouse #: int, city: string)
create database orderd
use orderd
CREATE TABLE CUSTOMER (
custid int,
cname char(15) not null,
city varchar(30),
primary key (custid) 
)
-
insert into CUSTOMER values (111,'John Smith', 'Karkala')
insert into CUSTOMER values (112,'Ramesh N', 'Nitte')
insert into CUSTOMER values (113,'Franklin', 'Karkala')
insert into CUSTOMER values (114,'Alica', 'mangalore')
insert into CUSTOMER values (115,'Raju', 'Udupi')
CREATE TABLE C_ORDER (
orderid int,
odate datetime,
custid int,
ordamt int,
primary key (orderid) ,
foreign key(custid) references CUSTOMER(custid)on delete cascade on update cascade
)
insert into C_ORDER values (201,'2001-08-03', 111,null)
insert into C_ORDER values (202,'2002-08-03', 111,null)
insert into C_ORDER values (203,'2001-08-04', 112,null)
insert into C_ORDER values (204,'2004-02-01', 113,null)
insert into C_ORDER values (205,'2001-04-02', 114,null)
insert into C_ORDER values (206,'2005-02-01', 115,null)
insert into C_ORDER values (207,'2008-04-01', 115,null)
insert into C_ORDER values (209,'2008-02-01', 114,null)
insert into C_ORDER values (208,'2008-12-01', 111,null)
insert into C_ORDER values (200,'2008-11-01', 111,null)
insert into C_ORDER values (210,'2008-10-01', 111,null)
select * from C_ORDER
CREATE TABLE ITEM (
itemid int,
price int,
primary key (itemid)
)
insert into ITEM values (301,2000)
insert into ITEM values (302,2000)
insert into ITEM values (303,1000)
insert into ITEM values (304,5000)
insert into ITEM values (305,4000)
CREATE TABLE ORDER_ITEM (
orderid int,
itemid int,
qty int,
primary key (orderid,itemid),
foreign key(orderid) references C_ORDER(orderid) on delete cascade on update cascade,
foreign key(itemid) references ITEM(itemid) on delete cascade on update cascade
)
insert into ORDER_ITEM values (200,301,1)
insert into ORDER_ITEM values (200,305,2)
insert into ORDER_ITEM values (201,301,2)
insert into ORDER_ITEM values (201,302,4)
insert into ORDER_ITEM values (201,303,4)
insert into ORDER_ITEM values (201,304,4)
insert into ORDER_ITEM values (201,305,3)
insert into ORDER_ITEM values (202,303,2)
insert into ORDER_ITEM values (202,305,4)
insert into ORDER_ITEM values (203,302,1)
insert into ORDER_ITEM values (204,305,2)
insert into ORDER_ITEM values (205,301,3)
insert into ORDER_ITEM values (206,301,5)
/* USE THIS TO UPDATE NULL VALUES IN THE TABLE C_ORDER*/
update C_ORDER set ordamt = (select sum(O.qty * T.price)
 from ORDER_ITEM O, ITEM T
 where O.itemid = T.itemid
and O.orderid = 207)
where orderid = 207
select * from C_ORDER
CREATE TABLE WAREHOUSE (
warehouseid int,
city varchar(20)not null,
primary key (warehouseid)
 )
insert into WAREHOUSE values (1,'MAGALORE')
insert into WAREHOUSE values (2,'MAGALORE')
insert into WAREHOUSE values (3,'MAGALORE')
insert into WAREHOUSE values (4,'UDUPI')
insert into WAREHOUSE values (5,'UDUPI')
insert into WAREHOUSE values (6,'KARKALA')
CREATE TABLE SHIPMENT (
orderid int,
warehouseid int,
ship_dt datetime,
primary key (orderid,warehouseid) ,
foreign key(orderid) references C_ORDER(orderid) on delete cascade on update cascade,
foreign key(warehouseid) references WAREHOUSE(warehouseid) on delete cascade on update
cascade
 )
insert into SHIPMENT values (201,1,'2001-04-02')
insert into SHIPMENT values (201,2,'2001-04-04')
insert into SHIPMENT values (201,4,'2001-04-04')
insert into SHIPMENT values (202,1,'2001-05-02')
insert into SHIPMENT values (202,2,'2002-05-12')
insert into SHIPMENT values (202,3,'2003-06-01')
insert into SHIPMENT values (202,4,'2003-06-01')
insert into SHIPMENT values (203,1,'2004-02-01')
insert into SHIPMENT values (203,2,'2004-02-01')
insert into SHIPMENT values (203,3,'2004-02-01')
insert into SHIPMENT values (204,4,'2004-06-02')
insert into SHIPMENT values (204,2,'2004-06-02')
select * from C_ORDER
select * from CUSTOMER
select * from ITEM
select * from ORDER_ITEM
select * from WAREHOUSE
SELECT * FROM SHIPMENT
 /*1. Produce a listing: CUSTNAME, #oforders, AVG_ORDER_AMT, where the middle 
column is the total numbers of orders
by the customer and the last column is the average order amount for that customer.*/
select C.cname,count(O.orderid) as NO_OF_ORDER,
avg(O.ordamt) as AVG_ORD_AMT,
sum(O.ordamt) as Total_amt
from CUSTOMER C,C_ORDER O
where C.custid=O.custid 
group by C.cname
 /* 2. For each item that has more than two orders , list the item, number of orders 
that are shipped from atleast two
 warehouses and total quantity of items shipped*/
 select OI.itemid AS item,
 count(distinct OI.orderid) AS num_order,
SUM(OI.qty) as total_qty_shipped
from order_item OI
JOIN SHIPMENT S on OI.orderid=S.orderid
group by OI.itemid
having count(distinct s.warehouseid) >=2
ORDER BY OI.itemid
 /* 3. List the customers who have ordered for every item that the company 
produces*/
 select C.cname as CUSTNAME
 from CUSTOMER C
 where NOT EXISTS(
 SELECT I.itemid
 from item I
 where NOT EXISTS(
 select OI.orderid
from ORDER_ITEM OI
join C_ORDER O on OI.orderid =O.orderid
where O.custid=C.custid
and OI.itemid=I.itemid
)
);









5. Bank Database
Database Schema:
BRANCH ( branch_name : string, branch_city : string, assets : real )
ACCOUNT ( accno : int, branch_name : string, balance : real )
CUSTOMER ( customer_name : string, customer_street : string, customer_city : string )
DEPOSITOR ( customer_name : string, accno : int )
LOAN ( loan_number : int, branch_name : string, amount : real )
BORROWER ( customer_name: string, loan_number : int )
Creating Database :
 CREATE DATABASE bank;
 USE bank;
Creating Tables and data insertion :
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
FOREIGN KEY(cname) REFERENCES Customer(cname) ON DELETE CASCADE ON UPDATE 
CASCADE,
FOREIGN KEY(accno) REFERENCES Account(accno) ON DELETE CASCADE ON UPDATE 
CASCADE
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
FOREIGN KEY(bname) REFERENCES Branch(bname) ON DELETE CASCADE ON UPDATE 
CASCADE
);
INSERT INTO Loan VALUES(123,'CORP MNG',10000);
CREATE TABLE Borrower 
(
cname varchar(20),
lno int,
PRIMARY KEY(cname,lno),
FOREIGN KEY(cname) REFERENCES Customer(cname) ON DELETE CASCADE ON UPDATE 
CASCADE,
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
)

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

SELECT C.cname 
FROM Customer C 
WHERE EXISTS (
SELECT COUNT(B.bname)
FROM Branch B, Account A, Depositor D
WHERE B.bcity='Mangalore' AND A.accno=D.accno AND A.bname=B.bname AND
D.cname=C.cname
GROUP BY B.bcity HAVING COUNT(*)>=2
);


