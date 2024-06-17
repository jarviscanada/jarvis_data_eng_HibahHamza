
 # Introduction

# SQL Quries

###### Table Setup (DDL)

###### Question 1: Show all members
> CREATE TABLE cd.members (
    memid INT PRIMARY KEY,
    surname VARCHAR(200) NOT NULL,
    firstname VARCHAR(200) NOT NULL,
    address VARCHAR(300),
    zipcode INT,
    telephone VARCHAR(20),
    recommendedby INT,
    joindate TIMESTAMP,
    FOREIGN KEY (recommendedby) REFERENCES cd.members(memid)
> ###### Question 2: Show all bookings
> CREATE TABLE cd.bookings (
    bookid INT PRIMARY KEY,
    facid INT NOT NULL,
    memid INT NOT NULL,
    starttime TIMESTAMP NOT NULL,
    slots INT NOT NULL,
    FOREIGN KEY (facid) REFERENCES cd.facilities(facid),
    FOREIGN KEY (memid) REFERENCES cd.members(memid)
);
>  ###### Question 3: Show all fascilities
> CREATE TABLE cd.facilities (
    facid INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    membercost NUMERIC NOT NULL,
    guestcost NUMERIC NOT NULL,
    initialoutlay NUMERIC NOT NULL,
    monthlymaintenance NUMERIC NOT NULL
);

 ###### Modifying Data
> ###### Question 1: insert into cd.facilities
(facid, Name, membercost, guestcost, initialoutlay, monthlymaintenance)
values (9, 'Spa', 20, 30, 100000, 800);
>  ###### Question 2: ^[[200~insert into cd.facilities
> (facid, name, membercost, guestcost, initialoutlay, monthlymaintenance)
> ###### Question 2:insert into cd.facilities
(facid, name, membercost, guestcost, initialoutlay, monthlymaintenance)
select (select max(facid) from cd.facilities)+1, 'Spa', 20, 30, 100000, 800;
> ##### Question 3: update cd.facilities
set initialoutlay = 10000
where facid = 1;
> ##### Question 4: update cd.facilities
set
membercost=(select membercost1.1 from cd.facilities where facid=0),
guestcost=(select guestcost1.1 from cd.facilities where facid=0)
where facid=1;
> ##### Question 5: delete from cd.bookings;
> ##### Question 6: delete from cd.members
where memid=37;
>  ###### Basics
> ###### Question 1: select bk.starttime
from cd.bookings bk
inner join cd.members mem on mem.memid=bk.memid
where mem.firstname='David' and mem.surname='Farrell';
> ###### Question 2: select bk.starttime as start, fs.name as name
from cd.facilities fs inner join cd.bookings bk
on fs.facid=bk.facid
where fs.name in ('Tennis Court 1','Tennis Court 2')
and bk.starttime >='2012-09-21' and bk.starttime <'2012-09-22'
order by start;
> ###### Question 3: select mem.firstname as memfname, mem.surname as memsname, rec.firstname as recfname, rec.surname as recsname
from
cd.members mem
left outer join cd.members rec
on rec.memid=mem.recommendedby
order by memsname, memfname;
>  ###### Question 4: select distinct rec.firstname as firstname, rec.surname as surname
from cd.members mem
inner join cd.members rec
on rec.memid=mem.recommendedby
order by surname, firstname;
> ###### Question 5: select distinct mems.firstname || ' ' || mems.surname as member,
(select recs.firstname || ' ' || recs.surname as recommender
from cd.members recs
where recs.memid=mems.recommendedby
)
from cd.members mems
order by member;
>  ###### Aggregation
>  ###### Question 1: select recommendedby, count(*)
from cd.members
where recommendedby is not null
group by recommendedby

order by recommendedby;
> ###### Question 2: select facid, sum(slots) as "Total Slots"
from cd.bookings
group by facid

order by facid;
> ###### Question 3:select facid, sum(slots) as "Total Slots"
from cd.bookings
where starttime >='2012-09-01' and starttime <'2012-10-01'
group by facid

order by "Total Slots";
>  ###### Question 4:select facid, extract(month from starttime) as month, sum(slots) as "Total Slots"
from cd.bookings
where extract(year from starttime) = 2012
group by facid, month
order by facid, month;
> ###### Question 5:select count(distinct memid) from cd.bookings
> ###### Question 6: select mem.surname, mem.firstname,mem.memid,min(bk.starttime) as starttime
from cd.bookings bk
inner join cd.members mem
on mem.memid=bk.memid
where starttime >= '2012-09-01'
group by mem.surname, mem.firstname, mem.memid
order by mem.memid;
> ###### Question 7: select count(*) over(), firstname, surname
from cd.members
order by joindate;
> ###### Question 8:select row_number() over(order by joindate), firstname, surname
from cd.members
order by joindate;
> ###### Question 9: select facid, total from (
select facid, sum(slots) total, rank() over (order by sum(slots) desc) rank
from cd.bookings
group by facid
) as ranked
where rank =1;
>  ###### STring
> ###### Question 1: select surname || ', ' || firstname as name from cd.members;
> ###### Question 2: select memid, telephone from cd.members where telephone ~ '[()]';
> ###### Question 3:select substr (mems.surname,1,1) as letter, count(*) as count
    from cd.members mems
    group by letter
    order by letter;
> EOF

