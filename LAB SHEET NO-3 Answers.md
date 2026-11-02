# LAB SHEET – 3

## MongoDB Application Design – Answers

### Q1. Student Activity Tracking System

```javascript
use CollegeDB

db.Activities.insertMany([
  {student_id: 1, name: "Rahul", course: "BCA", attendance: 90, skills: ["HTML", "CSS"], activities: ["Coding", "Sports"]},
  {student_id: 2, name: "Priya", course: "BCA", attendance: 95, skills: ["Python", "Java"], activities: ["Coding", "Reading"]},
  {student_id: 3, name: "Aman", course: "BBA", attendance: 80, skills: ["Excel"], activities: ["Sports", "Music"]},
  {student_id: 4, name: "Neha", course: "BCA", attendance: 92, skills: ["JavaScript"], activities: ["Coding", "Dance"]},
  {student_id: 5, name: "Karan", course: "BBA", attendance: 85, skills: ["Communication"], activities: ["Reading"]}
]);

// Display all students
db.Activities.find();

// Find students participating in coding
db.Activities.find({activities: "Coding"});

// Find students with attendance above 90
db.Activities.find({attendance: {$gt: 90}});
```

**Result:** Student information, attendance, skills and activities were stored and retrieved successfully.

---

### Q2. Event Logging System

```javascript
db.EventLogs.insertMany([
  {event_id: 1, user: "Rahul", type: "login", status: "success"},
  {event_id: 2, user: "Rahul", type: "login", status: "failed"},
  {event_id: 3, user: "Rahul", type: "login", status: "failed"},
  {event_id: 4, user: "Priya", type: "file_upload", status: "success"},
  {event_id: 5, user: "Aman", type: "logout", status: "success"},
  {event_id: 6, user: "Neha", type: "password_change", status: "success"}
]);

// Find failed login attempts
db.EventLogs.find({type: "login", status: "failed"});

// Find users with repeated failed logins
db.EventLogs.aggregate([
  {$match: {type: "login", status: "failed"}},
  {$group: {_id: "$user", attempts: {$sum: 1}}},
  {$match: {attempts: {$gt: 1}}}
]);

// Count each event type
db.EventLogs.aggregate([
  {$group: {_id: "$type", total: {$sum: 1}}}
]);
```

**Result:** Events were logged and repeated failed login attempts were identified using aggregation.

---

### Q3. Mini Blogging Platform

```javascript
db.Blogs.insertMany([
  {
    title: "Introduction to MongoDB",
    author: "Rahul",
    category: "Technology",
    tags: ["MongoDB", "Database"],
    comments: ["Very useful", "Good article"],
    published: true
  },
  {
    title: "Learning JavaScript",
    author: "Priya",
    category: "Programming",
    tags: ["JavaScript", "Web"],
    comments: ["Excellent"],
    published: true
  },
  {
    title: "Web Development Basics",
    author: "Aman",
    category: "Technology",
    tags: ["HTML", "CSS"],
    comments: ["Helpful"],
    published: false
  }
]);

// Find blogs by category
db.Blogs.find({category: "Technology"});

// Find blogs with MongoDB tag
db.Blogs.find({tags: "MongoDB"});

// Display published blogs
db.Blogs.find({published: true});

// Add a comment
db.Blogs.updateOne(
  {title: "Introduction to MongoDB"},
  {$push: {comments: "Great explanation"}}
);
```

**Result:** A blogging platform was created with authors, categories, tags and comments.

---

### Q4. Flexible Product Catalog

```javascript
db.Products.insertMany([
  {name: "Dell Laptop", category: "Laptop", RAM: "8GB", storage: "512GB SSD", price: 55000},
  {name: "HP Laptop", category: "Laptop", RAM: "16GB", storage: "1TB SSD", price: 65000},
  {name: "Samsung Mobile", category: "Mobile", RAM: "8GB", camera: "50MP", price: 25000},
  {name: "iPhone", category: "Mobile", RAM: "8GB", camera: "48MP", price: 70000},
  {name: "Canon Camera", category: "Camera", resolution: "24MP", lens: "18-55mm", price: 45000}
]);

// Find all laptops
db.Products.find({category: "Laptop"});

// Find mobiles below 50000
db.Products.find({
  category: "Mobile",
  price: {$lt: 50000}
});

// Find cameras
db.Products.find({category: "Camera"});
```

**Result:** Products with different attributes were stored in a flexible MongoDB collection.

---

### Q5. E-Commerce Order Management

```javascript
db.Customers.insertMany([
  {_id: 1, name: "Rahul"},
  {_id: 2, name: "Priya"}
]);

db.Products.insertMany([
  {_id: 101, name: "Laptop", price: 50000},
  {_id: 102, name: "Mouse", price: 500}
]);

db.CustomerOrders.insertMany([
  {order_id: 1, customer_id: 1, products: [{name: "Laptop", price: 50000, quantity: 1}]},
  {order_id: 2, customer_id: 1, products: [{name: "Mouse", price: 500, quantity: 2}]},
  {order_id: 3, customer_id: 2, products: [{name: "Laptop", price: 50000, quantity: 1}]}
]);

// Purchase history of Rahul
db.CustomerOrders.find({customer_id: 1});

// Calculate total order amount
db.CustomerOrders.aggregate([
  {$unwind: "$products"},
  {$group: {
    _id: "$order_id",
    total: {$sum: {$multiply: ["$products.price", "$products.quantity"]}}
  }}
]);
```

**Result:** Customer purchase history and order totals were managed using MongoDB.

---

### Q6. Website Analytics System

```javascript
db.PageVisits.insertMany([
  {user: "Rahul", page: "Home", device: "Mobile", browser: "Chrome", location: "Delhi"},
  {user: "Priya", page: "Products", device: "Laptop", browser: "Firefox", location: "Roorkee"},
  {user: "Aman", page: "Home", device: "Mobile", browser: "Chrome", location: "Haridwar"},
  {user: "Neha", page: "Home", device: "Laptop", browser: "Chrome", location: "Delhi"},
  {user: "Karan", page: "About", device: "Mobile", browser: "Edge", location: "Meerut"}
]);

// Most visited pages
db.PageVisits.aggregate([
  {$group: {_id: "$page", visits: {$sum: 1}}},
  {$sort: {visits: -1}}
]);

// Popular devices
db.PageVisits.aggregate([
  {$group: {_id: "$device", total: {$sum: 1}}}
]);
```

**Result:** Website traffic and device statistics were calculated using aggregation.

---

### Q7. Content Management System

```javascript
db.Articles.insertMany([
  {title: "MongoDB Basics", author: "Rahul", category: "Technology", tags: ["Database"], status: "Published"},
  {title: "JavaScript Guide", author: "Priya", category: "Programming", tags: ["JS"], status: "Published"},
  {title: "HTML Tutorial", author: "Aman", category: "Web", tags: ["HTML"], status: "Draft"},
  {title: "CSS Guide", author: "Neha", category: "Web", tags: ["CSS"], status: "Published"},
  {title: "Python Basics", author: "Karan", category: "Programming", tags: ["Python"], status: "Draft"}
]);

// Published articles
db.Articles.find({status: "Published"});

// Unpublished articles
db.Articles.find({status: "Draft"});

// Find articles by author
db.Articles.find({author: "Rahul"});

// Update article status
db.Articles.updateOne(
  {title: "HTML Tutorial"},
  {$set: {status: "Published"}}
);
```

**Result:** Articles were stored and managed using publication status, categories and tags.

---

### Q8. Shopping Cart Application

```javascript
db.Carts.insertOne({
  user: "Rahul",
  products: [
    {name: "Laptop", price: 50000, quantity: 1},
    {name: "Mouse", price: 500, quantity: 2}
  ]
});

// Add a product
db.Carts.updateOne(
  {user: "Rahul"},
  {$push: {products: {name: "Keyboard", price: 1000, quantity: 1}}}
);

// Update quantity
db.Carts.updateOne(
  {user: "Rahul", "products.name": "Mouse"},
  {$set: {"products.$.quantity": 3}}
);

// Remove a product
db.Carts.updateOne(
  {user: "Rahul"},
  {$pull: {products: {name: "Keyboard"}}}
);

// Calculate cart value
db.Carts.aggregate([
  {$match: {user: "Rahul"}},
  {$unwind: "$products"},
  {$group: {
    _id: "$user",
    total: {$sum: {$multiply: ["$products.price", "$products.quantity"]}}
  }}
]);
```

**Result:** Products were added, updated and removed from the shopping cart, and the total cart value was calculated.

---

### Q9. Product Review and Rating System

```javascript
db.Reviews.insertMany([
  {product: "Laptop", user: "Rahul", rating: 5, comment: "Excellent"},
  {product: "Laptop", user: "Priya", rating: 4, comment: "Good"},
  {product: "Mobile", user: "Aman", rating: 5, comment: "Great"},
  {product: "Mobile", user: "Neha", rating: 3, comment: "Average"},
  {product: "Laptop", user: "Karan", rating: 4, comment: "Nice"}
]);

// Average rating and review count
db.Reviews.aggregate([
  {$group: {
    _id: "$product",
    averageRating: {$avg: "$rating"},
    totalReviews: {$sum: 1}
  }}
]);
```

**Result:** Average product ratings and review counts were calculated using aggregation.

---

### Q10. Inventory Monitoring System

```javascript
db.Inventory.insertMany([
  {product: "Laptop", stock: 5, minimum: 10, supplier: "ABC Ltd"},
  {product: "Mouse", stock: 50, minimum: 20, supplier: "XYZ Ltd"},
  {product: "Keyboard", stock: 8, minimum: 15, supplier: "ABC Ltd"},
  {product: "Monitor", stock: 20, minimum: 10, supplier: "PQR Ltd"},
  {product: "Headphones", stock: 3, minimum: 10, supplier: "XYZ Ltd"}
]);

// Find products requiring restocking
db.Inventory.find({
  $expr: {$lte: ["$stock", "$minimum"]}
});

// Update stock after restocking
db.Inventory.updateOne(
  {product: "Laptop"},
  {$set: {stock: 25}}
);
```

**Result:** Products with low stock were identified for restocking.

---

### Q11. Bank Wallet Transfer Using Transactions

MongoDB transactions allow multiple database operations to succeed or fail together. Transactions require a replica set or a sharded cluster.

```javascript
db.Wallets.insertMany([
  {_id: 1, name: "Rahul", balance: 10000},
  {_id: 2, name: "Priya", balance: 5000}
]);

const session = db.getMongo().startSession();
const walletDB = session.getDatabase("CollegeDB");

session.startTransaction();

try {
  walletDB.Wallets.updateOne(
    {_id: 1},
    {$inc: {balance: -1000}}
  );

  walletDB.Wallets.updateOne(
    {_id: 2},
    {$inc: {balance: 1000}}
  );

  session.commitTransaction();
  print("Transfer successful");
} catch (error) {
  session.abortTransaction();
  print("Transfer failed");
} finally {
  session.endSession();
}

db.Wallets.find();
```

**Result:** Wallet balances were updated together using a MongoDB transaction.

---

### Q12. User Notification System

```javascript
db.Notifications.insertMany([
  {user: "Rahul", message: "Welcome", status: "Unread"},
  {user: "Rahul", message: "New assignment", status: "Unread"},
  {user: "Priya", message: "Exam notice", status: "Read"},
  {user: "Aman", message: "New message", status: "Unread"},
  {user: "Neha", message: "Result declared", status: "Read"}
]);

// Find unread notifications
db.Notifications.find({status: "Unread"});

// Find Rahul's notifications
db.Notifications.find({user: "Rahul"});

// Mark a notification as read
db.Notifications.updateOne(
  {user: "Rahul", message: "Welcome"},
  {$set: {status: "Read"}}
);
```

**Result:** Notifications were stored, retrieved and updated according to their read status.

---

### Q13. Course Learning Platform

```javascript
db.LearningProgress.insertMany([
  {student: "Rahul", course: "Web Development", instructor: "Amit", modules: 5, completed: 3, progress: 60},
  {student: "Priya", course: "Python", instructor: "Neha", modules: 4, completed: 4, progress: 100},
  {student: "Aman", course: "Web Development", instructor: "Amit", modules: 5, completed: 2, progress: 40},
  {student: "Neha", course: "Python", instructor: "Neha", modules: 4, completed: 3, progress: 75},
  {student: "Karan", course: "Java", instructor: "Raj", modules: 6, completed: 6, progress: 100}
]);

// Find completed courses
db.LearningProgress.find({progress: 100});

// Find students with progress above 50%
db.LearningProgress.find({progress: {$gt: 50}});

// Calculate average progress by course
db.LearningProgress.aggregate([
  {$group: {
    _id: "$course",
    averageProgress: {$avg: "$progress"}
  }}
]);
```

**Result:** Student learning progress and course completion were tracked successfully.

---

### Q14. Real-Time Order Tracking System

```javascript
db.OrderTracking.insertMany([
  {
    order_id: 101,
    customer: "Rahul",
    current_status: "Shipped",
    history: ["Order Placed", "Packed", "Shipped"]
  },
  {
    order_id: 102,
    customer: "Priya",
    current_status: "Delivered",
    history: ["Order Placed", "Packed", "Shipped", "Delivered"]
  }
]);

// Find an order
db.OrderTracking.find({order_id: 101});

// Update current status
db.OrderTracking.updateOne(
  {order_id: 101},
  {
    $set: {current_status: "Delivered"},
    $push: {history: "Delivered"}
  }
);

// Display order history
db.OrderTracking.find(
  {order_id: 101},
  {history: 1, current_status: 1}
);
```

**Result:** Order status and tracking history were maintained using MongoDB documents.

---

### Q15. Flexible News Database

```javascript
db.News.insertMany([
  {
    title: "Technology Update",
    category: "Technology",
    author: "Rahul",
    content: "New technology developments.",
    tags: ["AI", "Technology"]
  },
  {
    title: "Sports News",
    category: "Sports",
    author: "Priya",
    content: "Latest sports updates.",
    teams: ["India", "Australia"]
  },
  {
    title: "Business Report",
    category: "Business",
    author: "Aman",
    content: "Market updates.",
    company: "ABC Ltd"
  },
  {
    title: "Education News",
    category: "Education",
    author: "Neha",
    content: "Latest education updates.",
    institution: "COER University"
  }
]);

// Display all news
db.News.find();

// Search by category
db.News.find({category: "Technology"});

// Search by author
db.News.find({author: "Priya"});

// Search using tags
db.News.find({tags: "AI"});

// Update news content
db.News.updateOne(
  {title: "Technology Update"},
  {$set: {content: "Updated technology developments."}}
);
```

**Explanation:** MongoDB supports flexible documents, so different news articles can contain different fields without requiring every document to follow an identical structure.

**Result:** A flexible news database was created and searched using MongoDB queries.
