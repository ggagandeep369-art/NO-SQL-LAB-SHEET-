# LAB SHEET – 1

## Introduction to MongoDB

### AIM:

To understand the fundamentals of MongoDB and perform basic database operations such as database creation, insertion, retrieval, updating, deletion, sorting and aggregation.

---

### Q1. Install MongoDB and MongoDB Compass and configure the database environment.

**Answer:**

1. Download MongoDB Community Server from the official MongoDB website.
2. Install MongoDB on the system.
3. Download and install MongoDB Compass.
4. Start the MongoDB service.
5. Open MongoDB Compass.
6. Connect using the connection string:

```javascript
mongodb://localhost:27017
```

7. The database environment is now ready.

---

### Q2. Create a database named StudentDB and a collection named Students.

**Answer:**

```javascript
use StudentDB

db.createCollection("Students")
```

**Output:**

```text
switched to db StudentDB
{ ok: 1 }
```

---

### Q3. Insert at least five student documents into the Students collection.

**Answer:**

```javascript
db.Students.insertMany([
    {
        Student_ID: 1,
        Name: "Rahul",
        Course: "BCA",
        Semester: 5,
        Marks: 85
    },
    {
        Student_ID: 2,
        Name: "Priya",
        Course: "BCA",
        Semester: 5,
        Marks: 90
    },
    {
        Student_ID: 3,
        Name: "Aman",
        Course: "BBA",
        Semester: 3,
        Marks: 78
    },
    {
        Student_ID: 4,
        Name: "Neha",
        Course: "BCA",
        Semester: 5,
        Marks: 92
    },
    {
        Student_ID: 5,
        Name: "Karan",
        Course: "BBA",
        Semester: 3,
        Marks: 75
    }
])
```

**Output:**

```text
{
    acknowledged: true,
    insertedIds: {
        '0': ObjectId(...),
        '1': ObjectId(...),
        '2': ObjectId(...),
        '3': ObjectId(...),
        '4': ObjectId(...)
    }
}
```

---

### Q4. Display all documents using the find() command.

**Answer:**

```javascript
db.Students.find()
```

To display documents in a readable format:

```javascript
db.Students.find().pretty()
```

**Output:**

Displays all five student documents stored in the Students collection.

---

### Q5. Retrieve students belonging to a particular course.

**Answer:**

To retrieve all students enrolled in BCA:

```javascript
db.Students.find({ Course: "BCA" })
```

**Output:**

```javascript
[
    { Student_ID: 1, Name: "Rahul", Course: "BCA", Semester: 5, Marks: 85 },
    { Student_ID: 2, Name: "Priya", Course: "BCA", Semester: 5, Marks: 90 },
    { Student_ID: 4, Name: "Neha", Course: "BCA", Semester: 5, Marks: 92 }
]
```

---

### Q6. Update the details of an existing student using updateOne().

**Answer:**

To update Rahul's marks from 85 to 88:

```javascript
db.Students.updateOne(
    { Student_ID: 1 },
    { $set: { Marks: 88 } }
)
```

**Output:**

```javascript
{
    acknowledged: true,
    matchedCount: 1,
    modifiedCount: 1
}
```

To verify:

```javascript
db.Students.find({ Student_ID: 1 })
```

---

### Q7. Delete a student record using deleteOne().

**Answer:**

To delete the student whose Student_ID is 5:

```javascript
db.Students.deleteOne({ Student_ID: 5 })
```

**Output:**

```javascript
{
    acknowledged: true,
    deletedCount: 1
}
```

To verify:

```javascript
db.Students.find()
```

---

### Q8. Demonstrate MongoDB query operators such as $gt, $lt, $gte, $in and $and.

**Answer:**

**1. $gt (Greater Than)**

Displays students whose marks are greater than 80.

```javascript
db.Students.find({ Marks: { $gt: 80 } })
```

**2. $lt (Less Than)**

Displays students whose marks are less than 80.

```javascript
db.Students.find({ Marks: { $lt: 80 } })
```

**3. $gte (Greater Than or Equal To)**

Displays students whose marks are greater than or equal to 85.

```javascript
db.Students.find({ Marks: { $gte: 85 } })
```

**4. $in (Matches Any Value in an Array)**

Displays students belonging to either BCA or BBA.

```javascript
db.Students.find({
    Course: { $in: ["BCA", "BBA"] }
})
```

**5. $and (Logical AND)**

Displays BCA students whose marks are greater than 85.

```javascript
db.Students.find({
    $and: [
        { Course: "BCA" },
        { Marks: { $gt: 85 } }
    ]
})
```

---

### Q9. Perform sorting, limiting and skipping operations on student records.

**Answer:**

**1. Sorting**

Sort students according to marks in ascending order:

```javascript
db.Students.find().sort({ Marks: 1 })
```

For descending order:

```javascript
db.Students.find().sort({ Marks: -1 })
```

**2. Limiting**

Display only the first three student records:

```javascript
db.Students.find().limit(3)
```

**3. Skipping**

Skip the first two student records:

```javascript
db.Students.find().skip(2)
```

**4. Combining Operations**

Display the top two students according to marks:

```javascript
db.Students.find()
    .sort({ Marks: -1 })
    .limit(2)
```

---

### Q10. Use aggregation to calculate the average marks of students in different courses.

**Answer:**

MongoDB aggregation is used to process documents and calculate results using different stages.

**Program:**

```javascript
db.Students.aggregate([
    {
        $group: {
            _id: "$Course",
            Average_Marks: { $avg: "$Marks" },
            Total_Students: { $sum: 1 }
        }
    }
])
```

**Output (based on the original five records):**

```javascript
[
    {
        _id: "BCA",
        Average_Marks: 89,
        Total_Students: 3
    },
    {
        _id: "BBA",
        Average_Marks: 76.5,
        Total_Students: 2
    }
]
```



---

### RESULT:

Thus, the basic operations of MongoDB were studied and implemented successfully, including database creation, collection creation, insertion, retrieval, updating, deletion, query operators, sorting, limiting, skipping and aggregation.
