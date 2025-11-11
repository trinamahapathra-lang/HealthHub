# LibraryMate Database Schema

## Database: librarymate_db

MongoDB database with three main collections for managing library operations.

## Collections

### 1. students

Stores student information and registration details.

```javascript
{
    _id: ObjectId,
    studentId: String (unique, indexed),
    name: String,
    email: String,
    phone: String,
    department: String,
    course: String,
    semester: Integer,
    registrationDate: Date,
    isActive: Boolean
}
```

**Indexes:**
- `studentId`: Unique index for fast lookups
- `email`: Index for email-based searches
- `isActive`: Index for filtering active students

**Sample Document:**
```javascript
{
    _id: ObjectId("507f1f77bcf86cd799439011"),
    studentId: "ST001",
    name: "John Doe",
    email: "john.doe@college.edu",
    phone: "+919876543210",
    department: "Computer Science",
    course: "B.Tech",
    semester: 6,
    registrationDate: ISODate("2024-01-15T00:00:00Z"),
    isActive: true
}
```

### 2. books

Stores book inventory with categorization and availability tracking.

```javascript
{
    _id: ObjectId,
    bookId: String (unique, indexed),
    title: String,
    author: String,
    isbn: String,
    category: String, // Marketing, Accountancy, Science, Fiction
    totalCopies: Integer,
    availableCopies: Integer,
    publisher: String,
    addedDate: Date,
    isActive: Boolean
}
```

**Indexes:**
- `bookId`: Unique index for fast lookups
- `category`: Index for category-based filtering
- `isbn`: Index for ISBN searches
- `isActive`: Index for filtering active books

**Sample Document:**
```javascript
{
    _id: ObjectId("507f1f77bcf86cd799439012"),
    bookId: "BK001",
    title: "Marketing Management",
    author: "Philip Kotler",
    isbn: "978-0134236933",
    category: "Marketing",
    totalCopies: 5,
    availableCopies: 3,
    publisher: "Pearson Education",
    addedDate: ISODate("2024-01-10T00:00:00Z"),
    isActive: true
}
```

### 3. book_issuances

Tracks book issuance, returns, and fine calculations.

```javascript
{
    _id: ObjectId,
    issuanceId: String (unique, indexed),
    studentId: String (indexed),
    bookId: String (indexed),
    issueDate: Date,
    dueDate: Date,
    returnDate: Date (nullable),
    status: String, // ISSUED, RETURNED, OVERDUE
    fineAmount: Double,
    finePaid: Boolean,
    daysOverdue: Integer
}
```

**Indexes:**
- `issuanceId`: Unique index for fast lookups
- `studentId`: Index for student-based queries
- `bookId`: Index for book-based queries
- `status`: Index for filtering by status
- `dueDate`: Index for overdue calculations

**Sample Document:**
```javascript
{
    _id: ObjectId("507f1f77bcf86cd799439013"),
    issuanceId: "ISS20241111001",
    studentId: "ST001",
    bookId: "BK001",
    issueDate: ISODate("2024-11-01T10:00:00Z"),
    dueDate: ISODate("2024-11-15T23:59:59Z"),
    returnDate: null,
    status: "ISSUED",
    fineAmount: 0.0,
    finePaid: false,
    daysOverdue: 0
}
```

## SQL-Like Queries in MongoDB

### Common Queries

#### 1. Get All Active Students
```javascript
db.students.find({ isActive: true })
```

#### 2. Get Books by Category
```javascript
db.books.find({ category: "Marketing", isActive: true })
```

#### 3. Get Available Books
```javascript
db.books.find({ 
    availableCopies: { $gt: 0 },
    isActive: true 
})
```

#### 4. Get Student's Active Issuances
```javascript
db.book_issuances.find({ 
    studentId: "ST001",
    status: "ISSUED"
})
```

#### 5. Get Overdue Books
```javascript
db.book_issuances.find({
    status: "ISSUED",
    dueDate: { $lt: new Date() }
})
```

#### 6. Calculate Total Fines for a Student
```javascript
db.book_issuances.aggregate([
    { $match: { studentId: "ST001" } },
    { $group: { 
        _id: "$studentId",
        totalFine: { $sum: "$fineAmount" }
    }}
])
```

#### 7. Get Most Borrowed Books
```javascript
db.book_issuances.aggregate([
    { $group: {
        _id: "$bookId",
        count: { $sum: 1 }
    }},
    { $sort: { count: -1 } },
    { $limit: 10 }
])
```

#### 8. Get Students with Overdue Books
```javascript
db.book_issuances.aggregate([
    { $match: { status: "OVERDUE" } },
    { $lookup: {
        from: "students",
        localField: "studentId",
        foreignField: "studentId",
        as: "student"
    }},
    { $unwind: "$student" },
    { $project: {
        studentId: 1,
        studentName: "$student.name",
        phone: "$student.phone",
        fineAmount: 1,
        daysOverdue: 1
    }}
])
```

#### 9. Get Book Availability Report
```javascript
db.books.aggregate([
    { $group: {
        _id: "$category",
        totalBooks: { $sum: "$totalCopies" },
        availableBooks: { $sum: "$availableCopies" }
    }},
    { $project: {
        category: "$_id",
        totalBooks: 1,
        availableBooks: 1,
        issuedBooks: { $subtract: ["$totalBooks", "$availableBooks"] }
    }}
])
```

#### 10. Get Monthly Issuance Statistics
```javascript
db.book_issuances.aggregate([
    { $group: {
        _id: {
            year: { $year: "$issueDate" },
            month: { $month: "$issueDate" }
        },
        count: { $sum: 1 }
    }},
    { $sort: { "_id.year": -1, "_id.month": -1 } }
])
```

## Data Validation Rules

### Students Collection
- `studentId`: Required, unique, alphanumeric
- `name`: Required, minimum 2 characters
- `email`: Required, valid email format
- `phone`: Required, valid phone format
- `semester`: Integer between 1-8

### Books Collection
- `bookId`: Required, unique, alphanumeric
- `title`: Required, minimum 2 characters
- `isbn`: Required, valid ISBN format
- `category`: Required, one of: Marketing, Accountancy, Science, Fiction
- `totalCopies`: Required, positive integer
- `availableCopies`: Required, non-negative, <= totalCopies

### Book Issuances Collection
- `issuanceId`: Required, unique
- `studentId`: Required, must exist in students collection
- `bookId`: Required, must exist in books collection
- `issueDate`: Required, cannot be future date
- `dueDate`: Required, must be after issueDate
- `status`: Required, one of: ISSUED, RETURNED, OVERDUE
- `fineAmount`: Non-negative, max 500

## Backup and Maintenance

### Daily Backup
```bash
mongodump --db librarymate_db --out /backup/$(date +%Y%m%d)
```

### Restore from Backup
```bash
mongorestore --db librarymate_db /backup/20241111/librarymate_db
```

### Compact Database
```javascript
db.runCommand({ compact: 'students' })
db.runCommand({ compact: 'books' })
db.runCommand({ compact: 'book_issuances' })
```

### Rebuild Indexes
```javascript
db.students.reIndex()
db.books.reIndex()
db.book_issuances.reIndex()
```

## Performance Optimization

1. **Indexing Strategy**: All frequently queried fields are indexed
2. **Connection Pooling**: Configured in MongoDBConnection.java
3. **Query Optimization**: Use projection to limit returned fields
4. **Aggregation Pipeline**: For complex reporting queries
5. **Caching**: Implement application-level caching for static data

## Security

1. **Authentication**: Enable MongoDB authentication in production
2. **Authorization**: Create role-based access control
3. **Encryption**: Enable encryption at rest and in transit
4. **Audit Logging**: Enable MongoDB audit logs
5. **Backup Encryption**: Encrypt backup files
