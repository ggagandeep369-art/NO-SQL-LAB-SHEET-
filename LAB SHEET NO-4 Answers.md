# LAB SHEET – 4

## HBase and Cassandra – Answers

### Q1. Install and configure HBase and create a Students table.

HBase is a distributed, column-family-oriented NoSQL database built on Hadoop.

**HBase Shell commands:**

```ruby
# Create table
create 'Students', 'personal', 'academic'

# Insert student records
put 'Students', '1', 'personal:name', 'Rahul'
put 'Students', '1', 'personal:course', 'BCA'
put 'Students', '1', 'academic:marks', '85'

put 'Students', '2', 'personal:name', 'Priya'
put 'Students', '2', 'personal:course', 'BCA'
put 'Students', '2', 'academic:marks', '90'

put 'Students', '3', 'personal:name', 'Aman'
put 'Students', '3', 'personal:course', 'BBA'
put 'Students', '3', 'academic:marks', '78'

put 'Students', '4', 'personal:name', 'Neha'
put 'Students', '4', 'personal:course', 'BCA'
put 'Students', '4', 'academic:marks', '92'

put 'Students', '5', 'personal:name', 'Karan'
put 'Students', '5', 'personal:course', 'BBA'
put 'Students', '5', 'academic:marks', '75'

# Display all records
scan 'Students'
```

**Result:** A Students table was created with personal and academic column families.

---

### Q2. Create an Employee table and perform CRUD operations in HBase.

```ruby
# Create table
create 'Employee', 'personal', 'job'

# Insert records
put 'Employee', '101', 'personal:name', 'Rahul'
put 'Employee', '101', 'job:department', 'IT'
put 'Employee', '101', 'job:salary', '40000'

put 'Employee', '102', 'personal:name', 'Priya'
put 'Employee', '102', 'job:department', 'HR'
put 'Employee', '102', 'job:salary', '35000'

# Retrieve employee
get 'Employee', '101'

# Update salary
put 'Employee', '101', 'job:salary', '45000'

# Display all employees
scan 'Employee'

# Delete employee
deleteall 'Employee', '102'
```

**Result:** Employee records were inserted, retrieved, updated and deleted using HBase Shell.

---

### Q3. Create an Event Logging System using HBase.

```ruby
# Create table
create 'EventLogs', 'event', 'user'

# Insert event records
put 'EventLogs', 'E101', 'event:type', 'Login'
put 'EventLogs', 'E101', 'event:timestamp', '2026-11-02'
put 'EventLogs', 'E101', 'event:status', 'Success'
put 'EventLogs', 'E101', 'user:name', 'Rahul'

put 'EventLogs', 'E102', 'event:type', 'FileUpload'
put 'EventLogs', 'E102', 'event:timestamp', '2026-11-02'
put 'EventLogs', 'E102', 'event:status', 'Success'
put 'EventLogs', 'E102', 'user:name', 'Priya'

put 'EventLogs', 'E103', 'event:type', 'Login'
put 'EventLogs', 'E103', 'event:timestamp', '2026-11-02'
put 'EventLogs', 'E103', 'event:status', 'Failed'
put 'EventLogs', 'E103', 'user:name', 'Aman'

# Retrieve event
get 'EventLogs', 'E101'

# Display all events
scan 'EventLogs'
```

**Result:** Event details were stored and retrieved using HBase column families.

---

### Q4. Create a Content Management System using HBase.

```ruby
# Create table
create 'CMS', 'content', 'author', 'metadata'

# Insert article
put 'CMS', 'A1', 'content:title', 'MongoDB Basics'
put 'CMS', 'A1', 'content:description', 'Introduction to MongoDB'
put 'CMS', 'A1', 'author:name', 'Rahul'
put 'CMS', 'A1', 'metadata:category', 'Technology'
put 'CMS', 'A1', 'metadata:status', 'Published'

# Insert another article
put 'CMS', 'A2', 'content:title', 'Python Basics'
put 'CMS', 'A2', 'author:name', 'Priya'
put 'CMS', 'A2', 'metadata:category', 'Programming'

# Retrieve article
get 'CMS', 'A1'

# Update status
put 'CMS', 'A1', 'metadata:status', 'Draft'

# Display all articles
scan 'CMS'

# Delete article
deleteall 'CMS', 'A2'
```

**Result:** A CMS was created using separate column families for content, authors and metadata.

---

### Q5. Create a Blogging Platform using HBase.

```ruby
# Create table
create 'Blogs', 'blog', 'author', 'comments'

# Insert blog records
put 'Blogs', 'B1', 'blog:title', 'Learning HBase'
put 'Blogs', 'B1', 'author:name', 'Rahul'
put 'Blogs', 'B1', 'comments:c1', 'Very useful'

put 'Blogs', 'B2', 'blog:title', 'MongoDB Tutorial'
put 'Blogs', 'B2', 'author:name', 'Priya'
put 'Blogs', 'B2', 'comments:c1', 'Excellent'

put 'Blogs', 'B3', 'blog:title', 'Python Programming'
put 'Blogs', 'B3', 'author:name', 'Aman'
put 'Blogs', 'B3', 'comments:c1', 'Good article'

# Retrieve a blog
get 'Blogs', 'B1'

# Display all blogs
scan 'Blogs'
```

**Result:** Blog records, author details and comments were stored using HBase.

---

### Q6. Demonstrate column families and qualifiers in HBase.

A column family is a group of related columns. A qualifier identifies an individual column inside a family.

```ruby
# Create table
create 'StudentDetails', 'personal', 'academic'

# Insert values using qualifiers
put 'StudentDetails', '1', 'personal:name', 'Rahul'
put 'StudentDetails', '1', 'personal:city', 'Roorkee'
put 'StudentDetails', '1', 'academic:course', 'BCA'
put 'StudentDetails', '1', 'academic:marks', '85'

# Display record
get 'StudentDetails', '1'

# Display table
scan 'StudentDetails'
```

**Explanation:**

* `personal` and `academic` are column families.
* `name`, `city`, `course` and `marks` are column qualifiers.
* A row can contain different qualifiers within its column families.

**Result:** The HBase column-family model was demonstrated.

---

### Q7. Create a WebsiteCounter table in HBase.

HBase provides an increment command to increase numeric counter values.

```ruby
# Create table
create 'WebsiteCounter', 'visits'

# Increment page views
incr 'WebsiteCounter', 'home', 'visits:count', 1
incr 'WebsiteCounter', 'home', 'visits:count', 1
incr 'WebsiteCounter', 'about', 'visits:count', 1
incr 'WebsiteCounter', 'home', 'visits:count', 1

# Retrieve home page counter
get 'WebsiteCounter', 'home'

# Display all counters
scan 'WebsiteCounter'
```

**Expected result:**

* Home page views = 3
* About page views = 1

**Result:** Website page-view counts were maintained using HBase increment operations.

---

### Q8. Create a user-session system using HBase TTL.

TTL (Time-To-Live) automatically expires stored cell versions after a specified duration.

```ruby
# Create a table with TTL of 3600 seconds
create 'UserSessions',
  {NAME => 'session', TTL => 3600}

# Insert session data
put 'UserSessions', 'S101', 'session:user', 'Rahul'
put 'UserSessions', 'S101', 'session:status', 'Active'

put 'UserSessions', 'S102', 'session:user', 'Priya'
put 'UserSessions', 'S102', 'session:status', 'Active'

# Retrieve session
get 'UserSessions', 'S101'

# Display sessions
scan 'UserSessions'

# Change TTL to 1800 seconds
alter 'UserSessions',
  {NAME => 'session', TTL => 1800}
```

**Explanation:** TTL specifies the lifetime of stored cell versions. Expired data is removed during HBase cleanup and may not disappear immediately.

**Result:** User sessions were stored with an automatic expiration policy.

---

### Q9. Design an HBase application for large-scale storage.

HBase is suitable for storing huge amounts of distributed data.

**Row-key strategy:**

A row key should distribute records evenly across regions and avoid hotspotting.

Example:

```text
Row Key Format:
[Hash Prefix]_[Student ID]

01_1001
02_1002
03_1003
04_1004
```

**Explanation:**

1. Each record receives a unique row key.
2. A hash prefix helps distribute records across regions.
3. Regions can be split as data grows.
4. RegionServers manage different regions.
5. Data can be distributed across multiple servers.

**Advantages:**

* Horizontal scalability.
* Efficient random access using row keys.
* Supports large datasets.
* Distributed storage.

**Result:** A row-key strategy for large-scale HBase applications was studied.

---

### Q10. Demonstrate HBase horizontal scaling.

HBase distributes table data into regions. These regions are managed by RegionServers.

**Architecture:**

```text
             HBase Client
                  |
              HMaster
                  |
        -------------------
        |                 |
   RegionServer 1    RegionServer 2
        |                 |
     Region A          Region B
     Region C          Region D
```

**Explanation:**

1. HMaster manages the cluster.
2. RegionServers manage table regions.
3. Large regions can split into smaller regions.
4. Regions can be assigned to different RegionServers.
5. Multiple RegionServers increase storage and processing capacity.

**Result:** HBase horizontal scaling through regions and RegionServers was studied.

---

### Q11. Install Cassandra and create a Students table.

Cassandra is a distributed wide-column NoSQL database.

```sql
-- Create keyspace
CREATE KEYSPACE CollegeDB
WITH replication = {
  'class': 'SimpleStrategy',
  'replication_factor': 1
};

USE CollegeDB;

-- Create Students table
CREATE TABLE Students (
    Student_ID int PRIMARY KEY,
    Name text,
    Course text,
    Semester int,
    Marks int
);

-- Insert records
INSERT INTO Students VALUES (1, 'Rahul', 'BCA', 5, 85);
INSERT INTO Students VALUES (2, 'Priya', 'BCA', 5, 90);
INSERT INTO Students VALUES (3, 'Aman', 'BBA', 3, 78);
INSERT INTO Students VALUES (4, 'Neha', 'BCA', 5, 92);
INSERT INTO Students VALUES (5, 'Karan', 'BBA', 3, 75);

-- Display records
SELECT * FROM Students;
```

**Result:** A Cassandra keyspace and Students table were created successfully.

---

### Q12. Create an Employees table in Cassandra and perform CRUD operations.

```sql
USE CollegeDB;

-- Create table
CREATE TABLE Employees (
    Emp_ID int PRIMARY KEY,
    Name text,
    Department text,
    Salary int
);

-- Insert records
INSERT INTO Employees VALUES (101, 'Rahul', 'IT', 40000);
INSERT INTO Employees VALUES (102, 'Priya', 'HR', 35000);
INSERT INTO Employees VALUES (103, 'Aman', 'Finance', 45000);

-- Retrieve records
SELECT * FROM Employees;

-- Retrieve employee by primary key
SELECT * FROM Employees WHERE Emp_ID = 101;

-- Update salary
UPDATE Employees
SET Salary = 50000
WHERE Emp_ID = 101;

-- Delete employee
DELETE FROM Employees WHERE Emp_ID = 103;
```

**Result:** Employee CRUD operations were performed using Cassandra Query Language.

---

### Q13. Create an Event Logging System in Cassandra.

```sql
USE CollegeDB;

CREATE TABLE EventLogs (
    User_ID text,
    Event_Time timestamp,
    Event_ID int,
    Event_Type text,
    Status text,
    PRIMARY KEY (User_ID, Event_Time, Event_ID)
) WITH CLUSTERING ORDER BY (Event_Time DESC);

-- Insert events
INSERT INTO EventLogs VALUES
('Rahul', '2026-11-02 10:00:00', 1, 'Login', 'Success');

INSERT INTO EventLogs VALUES
('Rahul', '2026-11-02 10:30:00', 2, 'FileUpload', 'Success');

INSERT INTO EventLogs VALUES
('Priya', '2026-11-02 11:00:00', 3, 'Login', 'Failed');

-- Find events of Rahul
SELECT * FROM EventLogs
WHERE User_ID = 'Rahul';

-- Find Rahul's events after a particular time
SELECT * FROM EventLogs
WHERE User_ID = 'Rahul'
AND Event_Time >= '2026-11-02 10:00:00';
```

**Explanation:** The partition key is User_ID, and Event_Time and Event_ID are clustering columns. This design supports retrieving events for a particular user in time order.

**Result:** Event records were stored and retrieved by user and time period.

---

### Q14. Demonstrate Cassandra consistency levels.

Consistency level determines how many replicas must respond before a read or write operation is considered successful.

```sql
USE CollegeDB;

-- Write using ONE
CONSISTENCY ONE;

INSERT INTO Students VALUES
(10, 'Rohit', 'BCA', 5, 80);

-- Write using QUORUM
CONSISTENCY QUORUM;

INSERT INTO Students VALUES
(11, 'Simran', 'BBA', 3, 88);

-- Read using ALL
CONSISTENCY ALL;

SELECT * FROM Students;
```

**Explanation:**

| Consistency Level | Meaning                              |
| ----------------- | ------------------------------------ |
| ONE               | One replica must respond.            |
| QUORUM            | A majority of replicas must respond. |
| ALL               | All replicas must respond.           |

For a replication factor of 3:

* ONE requires 1 replica.
* QUORUM requires 2 replicas.
* ALL requires all 3 replicas.

Higher consistency generally requires more replica responses and can reduce availability when replicas are unavailable.

**Result:** Cassandra consistency levels were studied and demonstrated.

---

### Q15. Design a column-oriented application using Cassandra.

A student management system can be implemented using Cassandra.

```sql
CREATE KEYSPACE StudentApp
WITH replication = {
  'class': 'SimpleStrategy',
  'replication_factor': 1
};

USE StudentApp;

CREATE TABLE Students (
    Student_ID int PRIMARY KEY,
    Name text,
    Course text,
    Semester int,
    Marks int
);

-- Create
INSERT INTO Students VALUES
(1, 'Rahul', 'BCA', 5, 85);

-- Read
SELECT * FROM Students;

-- Update
UPDATE Students
SET Marks = 95
WHERE Student_ID = 1;

-- Delete
DELETE FROM Students
WHERE Student_ID = 1;
```



**Result:** A student management application was designed using Cassandra, demonstrating CRUD operations and important NoSQL characteristics.
