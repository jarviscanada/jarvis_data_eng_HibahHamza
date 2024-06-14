
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

###### Question 2: Show all bookings
CREATE TABLE cd.bookings (
    bookid INT PRIMARY KEY,
    facid INT NOT NULL,
    memid INT NOT NULL,
    starttime TIMESTAMP NOT NULL,
    slots INT NOT NULL,
    FOREIGN KEY (facid) REFERENCES cd.facilities(facid),
    FOREIGN KEY (memid) REFERENCES cd.members(memid)
);

 ###### Question 3: Show all fascilities
CREATE TABLE cd.facilities (
    facid INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    membercost NUMERIC NOT NULL,
    guestcost NUMERIC NOT NULL,
    initialoutlay NUMERIC NOT NULL,
    monthlymaintenance NUMERIC NOT NULL
);

