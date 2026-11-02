# LAB SHEET – 5

## Riak and Neo4j – Answers

### Q1. Install Riak KV and perform CRUD operations.

Riak KV is a distributed key-value NoSQL database. It stores data using buckets, keys and values.

**Example HTTP operations using curl:**

```bash
# Insert a student record
curl -X PUT http://localhost:8098/buckets/students/keys/1 \
-H "Content-Type: application/json" \
-d '{"name":"Rahul","course":"BCA","marks":85}'

# Retrieve a student
curl http://localhost:8098/buckets/students/keys/1

# Update student details
curl -X PUT http://localhost:8098/buckets/students/keys/1 \
-H "Content-Type: application/json" \
-d '{"name":"Rahul","course":"BCA","marks":95}'

# Delete student
curl -X DELETE http://localhost:8098/buckets/students/keys/1
```

**Result:** Student records were inserted, retrieved, updated and deleted using Riak KV.

---

### Q2. Create a user-session management system using Riak.

```bash
# Store user sessions
curl -X PUT http://localhost:8098/buckets/user_sessions/keys/S101 \
-H "Content-Type: application/json" \
-d '{"user":"Rahul","status":"Active","device":"Laptop"}'

curl -X PUT http://localhost:8098/buckets/user_sessions/keys/S102 \
-H "Content-Type: application/json" \
-d '{"user":"Priya","status":"Active","device":"Mobile"}'

# Retrieve a session
curl http://localhost:8098/buckets/user_sessions/keys/S101

# Delete a session
curl -X DELETE http://localhost:8098/buckets/user_sessions/keys/S102
```

**Result:** User session information was stored and retrieved using unique keys.

---

### Q3. Store user profiles and preferences in Riak.

```bash
# Store user profile
curl -X PUT http://localhost:8098/buckets/users/keys/101 \
-H "Content-Type: application/json" \
-d '{"name":"Rahul","age":21,"city":"Roorkee","preferences":["Music","Sports"]}'

# Retrieve profile
curl http://localhost:8098/buckets/users/keys/101

# Update preferences
curl -X PUT http://localhost:8098/buckets/users/keys/101 \
-H "Content-Type: application/json" \
-d '{"name":"Rahul","age":21,"city":"Roorkee","preferences":["Music","Coding"]}'
```

**Result:** User profiles and preferences were stored as JSON values in Riak.

---

### Q4. Create a shopping cart using Riak.

```bash
# Create shopping cart
curl -X PUT http://localhost:8098/buckets/carts/keys/C101 \
-H "Content-Type: application/json" \
-d '{"user":"Rahul","products":[{"name":"Laptop","price":50000},{"name":"Mouse","price":500}]}'

# Retrieve cart
curl http://localhost:8098/buckets/carts/keys/C101

# Update cart
curl -X PUT http://localhost:8098/buckets/carts/keys/C101 \
-H "Content-Type: application/json" \
-d '{"user":"Rahul","products":[{"name":"Laptop","price":50000},{"name":"Keyboard","price":1000}]}'

# Delete cart
curl -X DELETE http://localhost:8098/buckets/carts/keys/C101
```

**Result:** Shopping cart information was stored, retrieved, updated and deleted using Riak.

---

### Q5. Demonstrate Riak data types.

Riak stores data using buckets, keys and values.

**Examples:**

```text
Bucket: students
Key: 101
Value: Rahul

Bucket: users
Key: 102
Value: {"name":"Priya","course":"BCA"}

Bucket: products
Key: 103
Value: ["Laptop","Mouse","Keyboard"]
```

**Explanation:**

1. String: Stores simple text.
2. JSON: Stores structured information.
3. Lists: Store multiple values.
4. Bucket: Groups related keys.
5. Key: Uniquely identifies a stored value.

**Result:** Different data formats and Riak storage components were studied.

---

### Q6. Demonstrate read and write quorum in Riak.

Quorum determines how many replicas must participate in a read or write operation.

**Example:**

```bash
# Write with quorum settings
curl -i -X PUT \
"http://localhost:8098/buckets/students/keys/101?w=2&dw=2" \
-H "Content-Type: application/json" \
-d '{"name":"Rahul","course":"BCA"}'

# Read with quorum settings
curl -i \
"http://localhost:8098/buckets/students/keys/101?r=2"
```

**Explanation:**

* `w=2`: Two replicas must acknowledge the write.
* `dw=2`: Two replicas must confirm durable storage.
* `r=2`: Two replicas must respond to the read.

Higher quorum requirements can improve confidence in data consistency but may reduce availability when nodes are unavailable.

**Result:** Read and write quorum concepts were demonstrated.

---

### Q7. Demonstrate horizontal scaling in Riak.

Riak uses a distributed architecture in which data is partitioned across multiple nodes.

**Architecture:**

```text
          Application
               |
          Riak Cluster
        /      |      \
     Node 1  Node 2  Node 3
        |      |      |
      Data   Data   Data
```

**Explanation:**

1. Data is distributed across nodes.
2. Each node handles part of the database workload.
3. Replication maintains copies of data.
4. New nodes can be added to increase capacity.
5. The cluster redistributes data as its membership changes.

**Result:** Horizontal scaling and data distribution in Riak were studied.

---

### Q8. Perform transaction-like operations in Riak.

Riak KV supports atomic operations on individual objects, but it does not provide general multi-object ACID transactions.

**Example:**

```bash
# Create two account records
curl -X PUT http://localhost:8098/buckets/accounts/keys/A1 \
-H "Content-Type: application/json" \
-d '{"name":"Rahul","balance":10000}'

curl -X PUT http://localhost:8098/buckets/accounts/keys/A2 \
-H "Content-Type: application/json" \
-d '{"name":"Priya","balance":5000}'

# Retrieve account details
curl http://localhost:8098/buckets/accounts/keys/A1
curl http://localhost:8098/buckets/accounts/keys/A2
```

**Explanation:**

1. Each account is stored as a separate object.
2. Individual objects can be updated.
3. Multiple updates are not automatically combined into one atomic transaction.
4. An application may need additional logic to handle partial failures.

**Result:** Transaction-like operations and Riak's transaction limitations were studied.

---

### Q9. Implement query-by-data using Riak JSON records.

Riak KV primarily retrieves data using keys. Searching JSON fields generally requires application-side filtering or an appropriately configured search/indexing feature.

**Example records:**

```bash
curl -X PUT http://localhost:8098/buckets/users/keys/1 \
-H "Content-Type: application/json" \
-d '{"name":"Rahul","city":"Roorkee","course":"BCA"}'

curl -X PUT http://localhost:8098/buckets/users/keys/2 \
-H "Content-Type: application/json" \
-d '{"name":"Priya","city":"Delhi","course":"BCA"}'
```

**Python application-side filtering example:**

```python
import requests

base_url = "http://localhost:8098/buckets/users/keys/"

keys = ["1", "2"]

for key in keys:
    response = requests.get(base_url + key)

    if response.status_code == 200:
        user = response.json()

        if user.get("course") == "BCA":
            print(user)
```

**Result:** JSON user records were retrieved by key and filtered using application logic.

---

### Q10. Demonstrate set operations in Riak.

Riak supports distributed set data types through its data-type API when configured appropriately.

**Example conceptual operations:**

```text
Set A = {Music, Coding, Sports}
Set B = {Coding, Reading}

Add:
Set A = {Music, Coding, Sports, Reading}

Remove:
Set A = {Music, Sports, Reading}

Membership:
Check whether Coding exists in Set A.

Intersection:
Set A ∩ Set B = {Coding}
```

**Explanation:**

1. Add inserts an element into a set.
2. Remove deletes an element.
3. Membership checks whether an element exists.
4. Intersection finds common elements between sets.

**Result:** Distributed set operations were studied using Riak data types.

---

### Q11. Install Neo4j and create Student-Course relationships.

Neo4j is a graph-based NoSQL database that stores data as nodes and relationships.

**Cypher queries:**

```cypher
// Create student nodes
CREATE
(s1:Student {name:'Rahul'}),
(s2:Student {name:'Priya'}),
(s3:Student {name:'Aman'});

// Create course nodes
CREATE
(c1:Course {name:'BCA'}),
(c2:Course {name:'BBA'});

// Create department
CREATE (d:Department {name:'Computer Science'});

// Create relationships
MATCH (s:Student {name:'Rahul'}), (c:Course {name:'BCA'})
CREATE (s)-[:ENROLLED_IN]->(c);

MATCH (s:Student {name:'Priya'}), (c:Course {name:'BCA'})
CREATE (s)-[:ENROLLED_IN]->(c);

MATCH (c:Course {name:'BCA'}), (d:Department {name:'Computer Science'})
CREATE (c)-[:BELONGS_TO]->(d);

// Display graph
MATCH (n) RETURN n;
```

**Result:** Student, course and department nodes were created and connected using relationships.

---

### Q12. Create a social network using Neo4j.

```cypher
// Create people
CREATE
(p1:Person {name:'Rahul'}),
(p2:Person {name:'Priya'}),
(p3:Person {name:'Aman'}),
(p4:Person {name:'Neha'});

// Create relationships
MATCH (a:Person {name:'Rahul'}), (b:Person {name:'Priya'})
CREATE (a)-[:FRIENDS_WITH]->(b);

MATCH (a:Person {name:'Rahul'}), (b:Person {name:'Aman'})
CREATE (a)-[:FOLLOWS]->(b);

MATCH (a:Person {name:'Priya'}), (b:Person {name:'Neha'})
CREATE (a)-[:WORKS_AT]->(b);

// Find Rahul's friends
MATCH (p:Person {name:'Rahul'})-[:FRIENDS_WITH]->(f:Person)
RETURN f;

// Find Rahul's followers
MATCH (f:Person)-[:FOLLOWS]->(p:Person {name:'Rahul'})
RETURN f;

// Display graph
MATCH (n)-[r]->(m)
RETURN n,r,m;
```

**Result:** A social network was created using Person nodes and different relationships.

---

### Q13. Create an e-commerce graph using Neo4j.

```cypher
// Create nodes
CREATE
(c:Customer {name:'Rahul'}),
(p1:Product {name:'Laptop', price:50000}),
(p2:Product {name:'Mouse', price:500}),
(cat:Category {name:'Electronics'}),
(o:Order {order_id:101});

// Create relationships
MATCH (c:Customer {name:'Rahul'}), (o:Order {order_id:101})
CREATE (c)-[:PLACED]->(o);

MATCH (o:Order {order_id:101}), (p:Product {name:'Laptop'})
CREATE (o)-[:CONTAINS]->(p);

MATCH (p:Product {name:'Laptop'}), (cat:Category {name:'Electronics'})
CREATE (p)-[:BELONGS_TO]->(cat);

// Find products purchased by Rahul
MATCH (c:Customer {name:'Rahul'})-[:PLACED]->(o:Order)-[:CONTAINS]->(p:Product)
RETURN p.name, p.price;

// Find products in Electronics
MATCH (p:Product)-[:BELONGS_TO]->(c:Category {name:'Electronics'})
RETURN p;
```

**Result:** An e-commerce graph was created and customer purchase details were retrieved.

---

### Q14. Demonstrate transactions in Neo4j.

Neo4j supports transactions to ensure that a group of database operations is executed consistently.

**Cypher transaction example:**

```cypher
// Begin a transaction
BEGIN;

// Create customer
CREATE (c:Customer {name:'Rahul', city:'Roorkee'});

// Create product
CREATE (p:Product {name:'Laptop', price:50000});

// Update product price
MATCH (p:Product {name:'Laptop'})
SET p.price = 55000;

// Retrieve customer
MATCH (c:Customer)
RETURN c;

// Commit transaction
COMMIT;
```

**Delete example:**

```cypher
MATCH (p:Product {name:'Laptop'})
DELETE p;
```

**Explanation:**

1. CREATE adds nodes and relationships.
2. MATCH finds existing graph data.
3. SET updates properties.
4. DELETE removes nodes or relationships.
5. Transactions help maintain database consistency.

**Note:** `BEGIN` and `COMMIT` are supported in appropriate Neo4j transaction interfaces such as the Cypher shell. In application drivers, transactions are generally managed through the driver's transaction API.

**Result:** Neo4j transaction concepts and graph operations were studied.

---

### Q15. Develop a simple Neo4j application using Python.

The Neo4j Python driver allows Python applications to communicate with a Neo4j database.

**Install driver:**

```bash
pip install neo4j
```

**Python program:**

```python
from neo4j import GraphDatabase

URI = "bolt://localhost:7687"
USERNAME = "neo4j"
PASSWORD = "your_password"

driver = GraphDatabase.driver(
    URI,
    auth=(USERNAME, PASSWORD)
)

def create_student(tx):
    tx.run(
        "CREATE (:Student {name: $name, course: $course})",
        name="Rahul",
        course="BCA"
    )

def display_students(tx):
    result = tx.run(
        "MATCH (s:Student) RETURN s.name AS name, s.course AS course"
    )

    for record in result:
        print(record["name"], record["course"])

def update_student(tx):
    tx.run(
        "MATCH (s:Student {name: $name}) SET s.course = $course",
        name="Rahul",
        course="MCA"
    )

def delete_student(tx):
    tx.run(
        "MATCH (s:Student {name: $name}) DETACH DELETE s",
        name="Rahul"
    )

with driver.session() as session:
    session.execute_write(create_student)
    session.execute_read(display_students)
    session.execute_write(update_student)
    session.execute_write(delete_student)

driver.close()
```


**Result:** A Python application was developed to perform CRUD operations using Neo4j.

---

## CONCLUSION

Riak KV and Neo4j demonstrate two different NoSQL database models. Riak stores data using key-value pairs, while Neo4j stores information using nodes and relationships. Both databases are useful for applications requiring flexible and scalable data storage.
