create database LIBRARY;
use LIBRARY;
create table Branch ( Branch_no int primary key, 
Manager_id int, 
Branch_address varchar(50), 
contact_no int not null);
insert into Branch values ( 101, 11, "056_street newtown",456-2278),
(102,12,"123_captown",678-52678),
(103,13,"345_street treehouse",988-22756);
select * from branch;
update branch set contact_no = 4562278
where branch_no= 101;
update branch set contact_no = 67852678
where branch_no= 102;
update branch set contact_no = 498822756
where branch_no= 103;

create table Employee
( Emp_id int primary key,
 Emp_name varchar(25),
 position varchar(25),
 salary int,
 branch_no int,
foreign key (branch_no) references branch (branch_no));

alter table employee rename column branch_no to emp_branch_no;

insert into Employee values ( 11, "JOHN", "MANAGER", 50000, 101),
(12,"LILLY","MANAGER",45000,103),
(13,"DIM","MANAGER",55000,102);
select * from employee;

create table Books 
( ISBN int primary key,
book_title varchar(50),
Category varchar(25),
Rental_price decimal(10,2),
Status varchar(15),
Author varchar(50),
Publisher varchar(50));

insert into Books values ( 001, "pride_and_prejudice","romance", 30, "yes", "Jane_Austen", "Dover"),
(002,"Great_Expectations","history", 24,"no","charles_Dickens", "Simon_&_Schuster"),
(003,"Mean_Spirit","thriller", 25, "yes", "Linda_Hogan","Simon_&_Schuster");
select * from books;

create table Customer
(Customer_id int primary key,
customer_name varchar (25),
customer_address varchar (50),
reg_date date);

insert into Customer values ( 1, "david","charles_street newyork", '2021-12-31'),
(2,"Minnu", "246_street captown", '2022-02-24'),
(3,"Fedri","alamsquare_2nd stree",'2023-12-30');
select * from customer;

create table Issuestatus
(issue_id int primary key,
issued_cust_id int,
foreign key(issued_cust_id) references customer(customer_id),
Issued_book_name varchar(50),
issue_date date,
ISBN_book int,
foreign key(ISBN_BOOK) references Books (ISBN));

insert into Issuestatus values (1001,1,"great_expectation", '2023-06-2',002),
(1002,2,"pride_and_prejudice", '2022-03-25',001),
(1003,3,"Mean_spirit",'2024-05-5',003),
(1004,1,"prider_and_prejudice",'2023-05-22',001);
select * from issuestatus;

create table Returnstatus
(Return_id int primary key,
Return_cust varchar(25),
Return_book_name varchar(25),
Return_date date,
ISBN_BOOK2 int,
foreign key (ISBN_BOOK2) references Books(ISBN));

insert into Returnstatus values
(2001,"minnu","pride_and_prejudice",'2022-04-10',001),
(2002,"fedri","mean_spirit",'2024-06-22',003),
(2003,"david","great_expectation",'2023-10-1',002);
select * from returnstatus;


-- Retrieve the book title, category, and rental price of all available books --
select book_title,category,rental_price from books where status = 'yes';

-- List the employee names and their respective salaries in descending order of salary -- 
select emp_name,salary from employee order by salary desc;

-- Retrieve the book titles and the corresponding customers who have issued those books --
select issued_cust_id,issued_book_name,customer_name 
from issuestatus 
inner join
customer on issuestatus.issued_cust_id = customer.customer_id;

-- Display the total count of books in each category --
select category,count(*) from books group by category;

-- Retrieve the employee names and their positions for the employees whose salaries are above Rs.50,000 --
select emp_name,position from employee where salary < 50000;

-- List the customer names who registered before 2022-01-01 and have not issued any books yet --

select customer_id, customer_name,issued_book_name from customer left join 
issuestatus on customer.customer_id = issuestatus.issued_cust_id where 
reg_date < 2022-01-01 and issued_cust_id is not null;

-- Display the branch numbers and the total count of employees in each branch --
select branch_no, count(emp_id) as total_emp from employee group  by branch_no;

-- Display the names of customers who have issued books in the month of June 2023 --
select customer_name from issuestatus  left join
customer on issuestatus.issued_cust_id = customer.Customer_id where
issue_date between '2023-06-02' and '2023-06-30';

-- Retrieve book_title from book table containing history --
select book_title from books where Category = 'history';

-- Retrieve the branch numbers along with the count of employees for branches having more than 5 employees --
select emp_branch_no,count(emp_id) from employee group by emp_branch_no having count(emp_id) > 5;

--  Retrieve the names of employees who manage branches and their respective branch addresses --
select Branch_address,emp_name from branch left join employee on branch.branch_no = employee.emp_branch_no
where position = 'manager';

-- Display the names of customers who have issued books with a rental price higher than Rs. 25 --
select rental_price,issued_cust_id,customer_name from issuestatus 
left join books on issuestatus.isbn_book = books.isbn
left join customer on issuestatus.issued_cust_id = customer.customer_id
where rental_price > 25;
