# sales_followup
Problem statement- In every sales there is immense need of sales followup management system as max deals are closed after 4 followup calls.
To remember priority wise customers and start day using all good followups generated previous day , this sql code will help

create schema Sales_F;
use sales_F;

CREATE TABLE emp_data (
    emp_name VARCHAR(20) NOT NULL,
    emp_id VARCHAR(20) PRIMARY KEY,
    designation VARCHAR(10) NOT NULL,
    manager_name VARCHAR(12) NOT NULL
); 
SELECT 
    *
FROM
    emp_data;

CREATE TABLE std_data (
    std_name VARCHAR(20) NOT NULL,
    school VARCHAR(10) NOT NULL,
    board VARCHAR(6) NOT NULL,
    phone_no BIGINT(20) PRIMARY KEY
); 
SELECT 
    *
FROM
    std_data;

CREATE TABLE prnt_data (
    prnt_id INT(5) PRIMARY KEY AUTO_INCREMENT,
    prnt_name VARCHAR(20) NOT NULL,
    occupation VARCHAR(10) NOT NULL,
    phone_no BIGINT(20) NOT NULL,
    FOREIGN KEY (phone_no)
        REFERENCES std_data (phone_no)
); 
SELECT 
    *
FROM
    prnt_data;

CREATE TABLE new_call (
    Ncall_id INT(5) PRIMARY KEY AUTO_INCREMENT,
    date DATE NOT NULL,
    status VARCHAR(20) NOT NULL,
    Phone_no BIGINT(20) NOT NULL,
    Nxt_call DATE,
    Priority VARCHAR(10),
    FOREIGN KEY (Phone_no)
        REFERENCES std_data (phone_no)
);

SELECT 
    *
FROM
    new_call;

CREATE TABLE foll_call (
    foll_id INT(5) PRIMARY KEY AUTO_INCREMENT,
    Phone_no BIGINT(20) NOT NULL,
    status VARCHAR(20) NOT NULL,
    nxt_call DATE NOT NULL,
    FOREIGN KEY (phone_no)
        REFERENCES std_data (phone_no)
);

SELECT 
    *
FROM
    foll_call;

CREATE TABLE owner (
    owner_id INT(5) PRIMARY KEY AUTO_INCREMENT,
    emp_id VARCHAR(20) NOT NULL,
    Phone_no BIGINT(20) NOT NULL,
    FOREIGN KEY (emp_id)
        REFERENCES emp_data (emp_id),
        foreign key (phone_no) references std_data(Phone_no)
);


SELECT 
    *
FROM
    owner;
    
Insert into emp_data values ('Ashutosh','TNL21846182','GM','Ashutosh');
Insert into emp_data values ('Vipul','TNL21846183','SM','Ashutosh');
Insert into emp_data values ('Vishal','TNL21846184','M','Vipul');
Insert into emp_data values ('Mahesh','TNL21846185','Associate','Vishal');
Insert into emp_data values ('Sohan','TNL21846186','Associate','Vishal');

Insert into std_data values ('Kartik','KV','CBSE', 8668265078);
Insert into std_data values ('Sahil','DPS','ICSE', 9098350926);
Insert into std_data values ('Prakash','DPS','CBSE', 8668265079);
Insert into std_data values ('Suresh','KV','ICSE', 8668265077);
Insert into std_data values ('Ganesh','KV','CBSE', 8668265065);

Insert into prnt_data values (1,'Sunil','Business', '8668265078');
Insert into prnt_data values (2,'Anil','Govt Job', '9098350926');
Insert into prnt_data values (3,'Prabhakar','Pvt. Job', '8668265079');
Insert into prnt_data values (4,'Tularam','Business', '8668265077');
Insert into prnt_data values (5,'Gangaram','Business', '8668265065');

Insert into new_call values (1, '2023-04-26', 'followup', 8668265078, '2023-04-27','Good');
Insert into new_call values (2, '2023-04-26', 'Disqualify', 9098350926,'2023-04-27','Bad'); 
Insert into new_call values (3, '2023-04-26', 'followup', 8668265079, '2023-04-28','Avg'); 
Insert into new_call values (4, '2023-04-26', 'Not interested', 8668265077, '2023-04-27','Bad'); 
Insert into new_call values (5, '2023-04-26', 'Sale done', 8668265065, '2023-04-27','No need');  

Insert into foll_call values (1, 8668265078, 'followup', '2023-04-27');
Insert into foll_call values (2, 9098350926, 'Disqualify', '2023-04-27');
Insert into foll_call values (3, 8668265079, 'followup', '2023-04-28');
Insert into foll_call values (4, 8668265077, 'Not interested', '2023-04-27');
Insert into foll_call values (5, 8668265065, 'sale done', '2023-04-27');

insert into owner values (1,'TNL21846182', 8668265078);
insert into owner values (2,'TNL21846183', 9098350926);
insert into owner values (3,'TNL21846182', 8668265079);
insert into owner values (4,'TNL21846183', 8668265077);
insert into owner values (5,'TNL21846182', 8668265065);



------------------------------------queries-------------------------------------------------------------

1.Retrieve the names of all students assigned to employees

SELECT std_name
FROM std_data;

2.Retrieve the number of employees in organisation whose position is associate

SELECT COUNT(*) as Total_associates
FROM emp_data
WHERE designation = 'ASSOCIATE';

3.Retrieve the names of all employees in organization

SELECT emp_name from emp_data;

4.Display the names of all students who are studying in DPS and KV school

SELECT std_name,school 
FROM std_data
WHERE  school in ('KV', 'DPS')
order by school;

5.display the employee name and assigned student name to him

SELECT emp_data.emp_name,std_data.std_name from emp_data 
join owner
on emp_data.emp_id=owner.emp_id 
join std_data on owner.phone_no=std_data.phone_no
order by emp_name;


6.Find the total followups for today whose priority is good

Select count(*) as Today_good_followup from new_call
where priority='Good' and Nxt_call='2023-04-27';


7.List all good followups for today with std_name and contact number

Select std_data.std_name,New_Call.phone_no from std_data
join New_call
on std_data.phone_no=New_call.phone_no
where priority='Good' and Nxt_call='2023-04-27';


8.Find the name of the employee and student name with contact number who has good priority followup for today
 
 Select emp_data.emp_name,std_data.std_name,New_call.phone_no from emp_data
 join owner
 on emp_data.emp_id=owner.emp_id
 join std_data
 on std_data.phone_no=owner.phone_no
 join New_call
 on std_data.phone_no=New_call.phone_no
 where Priority='Good' and Nxt_call='2023-04-27';
 


9.List the name of employee who done sale

Select emp_name from emp_data
join owner
on emp_data.emp_id=owner.emp_id
join New_call
on owner.phone_no=New_call.phone_no
where status='Sale done';




10.Find the name and contact of the student whose followps date is for today (27-04-2023)

Select std_data.std_name,New_call.phone_no from std_data
join New_call
on std_data.phone_no=New_call.phone_no
where Nxt_call='2023-04-27';

11. Retrieve student and parent details for calling

SELECT 
    std_data.std_name,
    std_data.school,
    prnt_data.prnt_name,
    prnt_data.occupation,
    std_data.phone_no
FROM
    std_data
        INNER JOIN
    prnt_data ON std_data.phone_no = prnt_data.phone_no;





