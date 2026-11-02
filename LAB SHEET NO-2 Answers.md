# LAB SHEET – 2

## NoSQL Database Design – Answers

### Q1. Create student data in RDBMS and MongoDB. Compare both approaches.

**MySQL (Relational Database):**

```sql
CREATE DATABASE CollegeDB;
USE CollegeDB;

CREATE TABLE Students (
    Student_ID INT PRIMARY KEY,
    Name VARCHAR(50),
    Course VARCHAR(30),
    Semester INT,
    Marks INT
);

INSERT INTO Students VALUES
(1, 'Rahul', 'BCA', 5, 85),
(2, 'Priya', 'BCA', 5, 90),
(3, 'Aman', 'BBA', 3, 78),
(4, 'Neha', 'BCA', 5, 92),
(5, 'Karan', 'BBA', 3, 75);

SELECT * FROM Students;
```

**MongoDB (Document Database):**

```javascript
use CollegeDB

db.Students.insertMany([
  {Student_ID: 1, Name: "Rahul", Course: "BCA", Semester: 5, Marks: 85},
  {Student_ID: 2, Name: "Priya", Course: "BCA", Semester: 5, Marks: 90},
  {Student_ID: 3, Name: "Aman", Course: "BBA", Semester: 3, Marks: 78},
  {Student_ID: 4, Name: "Neha", Course: "BCA", Semester: 5, Marks: 92},
  {Student_ID: 5, Name: "Karan", Course: "BBA", Semester: 3, Marks: 75}
]);

db.Students.find();
```

**Comparison:**

| Feature       | RDBMS             | MongoDB                       |
| ------------- | ----------------- | ----------------------------- |
| Data model    | Tables            | Documents                     |
| Schema        | Fixed             | Flexible                      |
| Storage       | Rows and columns  | BSON documents                |
| Relationships | Foreign keys      | Embedded documents/references |
| Scalability   | Commonly vertical | Supports horizontal scaling   |

**Result:** The same student data was represented using relational and document database models.

---

### Q2. Perform CRUD operations in MongoDB.

```javascript
use CollegeDB

db.Students.insertMany([
  {Student_ID: 1, Name: "Rahul", Course: "BCA", Semester: 5, Marks: 85},
  {Student_ID: 2, Name: "Priya", Course: "BCA", Semester: 5, Marks: 90},
  {Student_ID: 3, Name: "Aman", Course: "BBA", Semester: 3, Marks: 78},
  {Student_ID: 4, Name: "Neha", Course: "BCA", Semester: 5, Marks: 92},
  {Student_ID: 5, Name: "Karan", Course: "BBA", Semester: 3, Marks: 75}
]);

// Display all students
db.Students.find();

// Find BCA students
db.Students.find({Course: "BCA"});

// Update marks
db.Students.updateOne(
  {Student_ID: 1},
  {$set: {Marks: 95}}
);

// Delete a student
db.Students.deleteOne({Student_ID: 5});

// Count students
db.Students.countDocuments();
```

**Result:** Insertion, retrieval, updating, deletion and counting were performed successfully.

---

### Q3. Create an Orders collection using embedded documents.

```javascript
use CollegeDB

db.Orders.insertMany([
  {
    Order_ID: 101,
    Customer: {Name: "Rahul", City: "Delhi"},
    Products: [
      {Name: "Laptop", Price: 50000, Quantity: 1},
      {Name: "Mouse", Price: 500, Quantity: 2}
    ]
  },
  {
    Order_ID: 102,
    Customer: {Name: "Priya", City: "Roorkee"},
    Products: [
      {Name: "Mobile", Price: 20000, Quantity: 1}
    ]
  },
  {
    Order_ID: 103,
    Customer: {Name: "Aman", City: "Haridwar"},
    Products: [
      {Name: "Keyboard", Price: 1000, Quantity: 2}
    ]
  },
  {
    Order_ID: 104,
    Customer: {Name: "Neha", City: "Delhi"},
    Products: [
      {Name: "Monitor", Price: 12000, Quantity: 1}
    ]
  },
  {
    Order_ID: 105,
    Customer: {Name: "Karan", City: "Meerut"},
    Products: [
      {Name: "Headphones", Price: 1500, Quantity: 2}
    ]
  }
]);

// Find orders of Rahul
db.Orders.find({"Customer.Name": "Rahul"});

// Update product quantity in Order 101
db.Orders.updateOne(
  {Order_ID: 101, "Products.Name": "Mouse"},
  {$set: {"Products.$.Quantity": 3}}
);

// Calculate total amount of each order
db.Orders.aggregate([
  {$unwind: "$Products"},
  {$group: {
    _id: "$Order_ID",
    TotalAmount: {
      $sum: {$multiply: ["$Products.Price", "$Products.Quantity"]}
    }
  }}
]);

// Display orders above Rs. 10000
db.Orders.aggregate([
  {$unwind: "$Products"},
  {$group: {
    _id: "$Order_ID",
    TotalAmount: {
      $sum: {$multiply: ["$Products.Price", "$Products.Quantity"]}
    }
  }},
  {$match: {TotalAmount: {$gt: 10000}}}
]);
```

**Result:** Orders were stored using embedded documents, and aggregation was used to calculate order amounts.

---

### Q4. Create a student information system using Redis.

Redis is a key-value NoSQL database. The following commands use Redis CLI.

```redis
SET student:1 "Rahul"
SET student:2 "Priya"
SET student:3 "Aman"
SET student:4 "Neha"
SET student:5 "Karan"

# Retrieve a student
GET student:1

# Update student name
SET student:1 "Rahul Sharma"

# Delete a student
DEL student:5

# Display all student keys
KEYS student:*
```

**Result:** Student records were stored, retrieved, updated and deleted using Redis key-value commands.

---

### Q5. Create a Students table in Cassandra.

```sql
CREATE KEYSPACE CollegeDB
WITH replication = {
  'class': 'SimpleStrategy',
  'replication_factor': 1
};

USE CollegeDB;

CREATE TABLE Students (
    Student_ID int PRIMARY KEY,
    Name text,
    Course text,
    Semester int,
    Marks int
);

INSERT INTO Students VALUES (1, 'Rahul', 'BCA', 5, 85);
INSERT INTO Students VALUES (2, 'Priya', 'BCA', 5, 90);
INSERT INTO Students VALUES (3, 'Aman', 'BBA', 3, 78);
INSERT INTO Students VALUES (4, 'Neha', 'BCA', 5, 92);
INSERT INTO Students VALUES (5, 'Karan', 'BBA', 3, 75);

SELECT * FROM Students;

// Retrieve student by primary key
SELECT * FROM Students WHERE Student_ID = 1;

// Update marks
UPDATE Students SET Marks = 95 WHERE Student_ID = 1;

// Delete student
DELETE FROM Students WHERE Student_ID = 5;
```

**Note:** Cassandra does not automatically support filtering by any column. Queries such as searching by Course require an appropriate secondary index or a separately designed table.

**Result:** Student records were inserted, retrieved, updated and deleted using Cassandra Query Language (CQL).

---

### Q6. Create a Students table using HBase.

HBase Shell commands:

```ruby
# Create table with two column families
create 'Students', 'personal', 'academic'

# Insert records
put 'Students', '1', 'personal:name', 'Rahul'
put 'Students', '1', 'personal:course', 'BCA'
put 'Students', '1', 'academic:semester', '5'
put 'Students', '1', 'academic:marks', '85'

put 'Students', '2', 'personal:name', 'Priya'
put 'Students', '2', 'personal:course', 'BCA'
put 'Students', '2', 'academic:semester', '5'
put 'Students', '2', 'academic:marks', '90'

put 'Students', '3', 'personal:name', 'Aman'
put 'Students', '3', 'personal:course', 'BBA'
put 'Students', '3', 'academic:semester', '3'
put 'Students', '3', 'academic:marks', '78'

put 'Students', '4', 'personal:name', 'Neha'
put 'Students', '4', 'personal:course', 'BCA'
put 'Students', '4', 'academic:semester', '5'
put 'Students', '4', 'academic:marks', '92'

put 'Students', '5', 'personal:name', 'Karan'
put 'Students', '5', 'personal:course', 'BBA'
put 'Students', '5', 'academic:semester', '3'
put 'Students', '5', 'academic:marks', '75'

# Retrieve a student
get 'Students', '1'

# Update marks
put 'Students', '1', 'academic:marks', '95'

# Display all records
scan 'Students'

# Delete a student
deleteall 'Students', '5'
```

**Result:** An HBase table was created with personal and academic column families, and basic operations were performed.

---

### Q7. Create Student–Course relationships using Neo4j.

Cypher queries:

```cypher
// Create student nodes
CREATE
(s1:Student {id: 1, name: 'Rahul'}),
(s2:Student {id: 2, name: 'Priya'}),
(s3:Student {id: 3, name: 'Aman'}),
(s4:Student {id: 4, name: 'Neha'}),
(s5:Student {id: 5, name: 'Karan'});

// Create course nodes
CREATE
(c1:Course {name: 'BCA'}),
(c2:Course {name: 'BBA'}),
(c3:Course {name: 'BSc'});

// Create relationships
MATCH (s:Student {id: 1}), (c:Course {name: 'BCA'})
CREATE (s)-[:ENROLLED_IN]->(c);

MATCH (s:Student {id: 2}), (c:Course {name: 'BCA'})
CREATE (s)-[:ENROLLED_IN]->(c);

MATCH (s:Student {id: 3}), (c:Course {name: 'BBA'})
CREATE (s)-[:ENROLLED_IN]->(c);

MATCH (s:Student {id: 4}), (c:Course {name: 'BCA'})
CREATE (s)-[:ENROLLED_IN]->(c);

MATCH (s:Student {id: 5}), (c:Course {name: 'BSc'})
CREATE (s)-[:ENROLLED_IN]->(c);

// Display all students
MATCH (s:Student)
RETURN s;

// Find students enrolled in BCA
MATCH (s:Student)-[:ENROLLED_IN]->(c:Course {name: 'BCA'})
RETURN s.name;

// Find courses of Rahul
MATCH (s:Student {name: 'Rahul'})-[:ENROLLED_IN]->(c:Course)
RETURN c.name;

// Add a relationship
MATCH (s:Student {name: 'Karan'}), (c:Course {name: 'BCA'})
CREATE (s)-[:ENROLLED_IN]->(c);

// Remove a relationship
MATCH (s:Student {name: 'Karan'})-[r:ENROLLED_IN]->(c:Course {name: 'BCA'})
DELETE r;
```

**Result:** Student and course nodes were created and connected using ENROLLED_IN relationships.

---

### Q8. Compare MongoDB, Cassandra and Neo4j.

| Feature         | MongoDB                | Cassandra                      | Neo4j                   |
| --------------- | ---------------------- | ------------------------------ | ----------------------- |
| Model           | Document               | Wide-column                    | Graph                   |
| Storage         | BSON documents         | Rows and columns               | Nodes and relationships |
| Query           | MongoDB Query Language | CQL                            | Cypher                  |
| Scalability     | Horizontal scaling     | Distributed horizontal scaling | Supports clustering     |
| Best suited for | Content management     | Large-scale distributed data   | Connected data          |

**Explanation:**

* MongoDB stores data as flexible documents.
* Cassandra stores data in distributed tables and is designed for high availability.
* Neo4j stores data as nodes and relationships, making it suitable for highly connected information.

**Result:** Different NoSQL databases were compared based on their data models and applications.

---

### Q9. Deploy MongoDB on a local machine.

**Steps:**

1. Download and install MongoDB Community Server.
2. Install MongoDB Compass for graphical database management.
3. Start the MongoDB service.
4. Open MongoDB Shell (mongosh).
5. Create a database and collection.

Commands:

```javascript
use CollegeDB

db.createCollection("Students")

db.Students.insertOne({
  Student_ID: 1,
  Name: "Rahul",
  Course: "BCA",
  Marks: 85
});

db.Students.find();

db.Students.updateOne(
  {Student_ID: 1},
  {$set: {Marks: 95}}
);

db.Students.deleteOne({Student_ID: 1});
```

**Result:** MongoDB was deployed locally and basic database operations were demonstrated.

---

### Q10. Demonstrate Sharding in MongoDB.

Sharding is a method of distributing data across multiple servers called shards. It helps manage large datasets and increases horizontal scalability.

**Example configuration:**

```javascript
// Enable sharding on database
sh.enableSharding("CollegeDB");

// Enable sharding on Students collection
sh.shardCollection(
  "CollegeDB.Students",
  {Student_ID: "hashed"}
);

// Insert sample records
use CollegeDB

db.Students.insertMany([
  {Student_ID: 1, Name: "Rahul"},
  {Student_ID: 2, Name: "Priya"},
  {Student_ID: 3, Name: "Aman"},
  {Student_ID: 4, Name: "Neha"},
  {Student_ID: 5, Name: "Karan"}
]);

// Check sharding status
sh.status();
```

**Note:** These commands demonstrate sharding configuration. A working sharded cluster requires multiple shard servers, configuration servers and a mongos router. The collection must also have the required shard-key index.

**Advantages:**

1. Distributes large datasets.
2. Supports horizontal scaling.
3. Improves storage capacity.
4. Can improve query performance when queries target the shard key.

**Result:** The concept and configuration of MongoDB sharding were studied.

---

### Q11. Demonstrate Master-Slave Replication.

Replication maintains copies of data on multiple database servers. In MongoDB, this is implemented through a Replica Set with a primary and secondary members.

**Configuration example:**

Start multiple MongoDB instances using different ports and data directories. Then connect to one instance and run:

```javascript
rs.initiate({
  _id: "CollegeReplica",
  members: [
    {_id: 0, host: "localhost:27017"},
    {_id: 1, host: "localhost:27018"}
  ]
});
```

Insert data on the primary:

```javascript
use CollegeDB

db.Students.insertOne({
  Student_ID: 1,
  Name: "Rahul",
  Course: "BCA"
});
```

Check replica status:

```javascript
rs.status();
```

On the secondary, enable reading:

```javascript
rs.secondaryOk();

db.Students.find();
```

**Explanation:**

* The primary accepts writes.
* Secondary members replicate the primary's data.
* If the primary fails, an eligible secondary can be elected as the new primary.

**Result:** The primary-secondary replication concept was studied using MongoDB Replica Sets.

---

### Q12. Demonstrate Peer-to-Peer Replication using Cassandra.

Cassandra uses a peer-to-peer architecture in which all nodes can participate in handling requests.

**Create a keyspace:**

```sql
CREATE KEYSPACE CollegeDB
WITH replication = {
  'class': 'SimpleStrategy',
  'replication_factor': 3
};

USE CollegeDB;
```

Create a table:

```sql
CREATE TABLE Students (
    Student_ID int PRIMARY KEY,
    Name text,
    Course text,
    Marks int
);
```

Insert sample data:

```sql
INSERT INTO Students VALUES (1, 'Rahul', 'BCA', 85);
INSERT INTO Students VALUES (2, 'Priya', 'BCA', 90);
INSERT INTO Students VALUES (3, 'Aman', 'BBA', 78);

SELECT * FROM Students;
```

Check cluster nodes:

```bash
nodetool status
```

**Explanation:**

* Data is distributed among multiple nodes.
* Replication factor determines the number of copies maintained.
* Nodes communicate with each other without a permanent master.
* The system can continue operating even if some nodes become unavailable, depending on replication and consistency settings.

**Result:** Cassandra's peer-to-peer architecture and replication concept were studied.

---

### Q13. Combine Sharding and Replication.

Sharding divides data into multiple parts, while replication creates copies of those parts.

**Architecture:**

```text
                 Application
                      |
                 Query Router
                      |
          -------------------------
          |                       |
        Shard 1                 Shard 2
          |                       |
     ------------            ------------
     |          |            |          |
  Primary    Secondary     Primary    Secondary
  Replica     Replica      Replica     Replica
```

**Explanation:**

1. Data is divided between multiple shards.
2. Each shard stores a different part of the dataset.
3. Replicas maintain copies of each shard's data.
4. If one server fails, another replica may continue serving requests.
5. More shards can be added to increase storage and processing capacity.

**Advantages:**

* Horizontal scalability.
* Better data distribution.
* Fault tolerance.
* Improved availability.

**Result:** The combined architecture of sharding and replication was studied.

---

### Q14. Implement MapReduce on a student dataset.

MapReduce processes large datasets using two main functions: Map and Reduce.

**Example using MongoDB:**

```javascript
use CollegeDB

db.Students.insertMany([
  {Student_ID: 1, Name: "Rahul", Course: "BCA", Marks: 85},
  {Student_ID: 2, Name: "Priya", Course: "BCA", Marks: 90},
  {Student_ID: 3, Name: "Aman", Course: "BBA", Marks: 78},
  {Student_ID: 4, Name: "Neha", Course: "BCA", Marks: 92},
  {Student_ID: 5, Name: "Karan", Course: "BBA", Marks: 75}
]);

// Map function: emit course and marks
var mapFunction = function() {
  emit(this.Course, {
    count: 1,
    totalMarks: this.Marks
  });
};

// Reduce function: combine values
var reduceFunction = function(key, values) {
  var result = {count: 0, totalMarks: 0};

  values.forEach(function(value) {
    result.count += value.count;
    result.totalMarks += value.totalMarks;
  });

  return result;
};

// Execute MapReduce
db.Students.mapReduce(
  mapFunction,
  reduceFunction,
  {out: "CourseStatistics"}
);

// Display results
db.CourseStatistics.find();
```

**Expected output:**

```javascript
{_id: "BCA", value: {count: 3, totalMarks: 267}}
{_id: "BBA", value: {count: 2, totalMarks: 153}}
```

Average marks:

* BCA = 267 / 3 = 89
* BBA = 153 / 2 = 76.5

**Note:** MongoDB's `mapReduce()` command is deprecated in modern MongoDB versions. The aggregation framework is recommended for new applications. The above code demonstrates the traditional MapReduce concept.

**Result:** MapReduce was used to calculate student counts and average marks for each course.

---

### Q15. Study different database distribution models.

**1. Single Server**

All data is stored on one server.

```text
Application
     |
Database Server
     |
All Data
```

* Advantage: Simple to manage.
* Limitation: Single point of failure.
* Application: Small applications.

**2. Sharding**

Data is divided and distributed across multiple servers.

```text
       Application
            |
       Query Router
        /       \
    Shard 1   Shard 2
```

* Advantage: Horizontal scalability.
* Limitation: More complex management.
* Application: Large e-commerce platforms.

**3. Master-Slave Replication**

One master handles writes, and slave servers maintain copies.

```text
       Master
       /    \
   Slave 1  Slave 2
```

* Advantage: Read availability and backup copies.
* Limitation: Master can become a bottleneck.
* Application: Read-heavy applications.

**4. Peer-to-Peer Replication**

All nodes participate in the distributed system.

```text
      Node 1
      /    \
  Node 2 -- Node 3
```

* Advantage: No permanent master node.
* Limitation: Consistency management can be complex.
* Application: Distributed databases such as Cassandra.

**5. Sharding + Replication**

Data is divided into shards, and each shard has replicas.

```text
          Application
               |
          Query Router
          /         \
       Shard 1     Shard 2
       /    \      /    \
    Replica Replica Replica Replica
```

* Advantage: Scalability and fault tolerance.
* Limitation: Complex configuration and maintenance.
* Application: Large-scale distributed applications.

**Result:** Different database distribution models were studied and compared based on architecture, scalability, availability and limitations.
