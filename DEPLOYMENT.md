# LibraryMate Deployment Guide

## Prerequisites

### 1. Software Requirements
- JDK 8 or higher
- NetBeans IDE 12+ (or any Java IDE)
- Apache Tomcat 9.0+
- MongoDB 4.4+
- Maven 3.6+ (optional, for dependency management)

### 2. Hardware Requirements
- Minimum 4GB RAM
- 2GB free disk space
- Stable internet connection (for SMS features)

## Installation Steps

### Step 1: Install MongoDB

#### Windows:
1. Download MongoDB from https://www.mongodb.com/try/download/community
2. Run the installer
3. Install as a Windows Service
4. MongoDB will run on default port 27017

#### Linux:
```bash
sudo apt-get update
sudo apt-get install -y mongodb-org
sudo systemctl start mongod
sudo systemctl enable mongod
```

#### Verify Installation:
```bash
mongo --version
```

### Step 2: Setup Database

1. Open MongoDB shell:
```bash
mongo
```

2. Create database:
```javascript
use librarymate_db
```

3. Create collections (optional, will be auto-created):
```javascript
db.createCollection("students")
db.createCollection("books")
db.createCollection("book_issuances")
```

4. Create indexes for better performance:
```javascript
db.students.createIndex({ "studentId": 1 }, { unique: true })
db.books.createIndex({ "bookId": 1 }, { unique: true })
db.book_issuances.createIndex({ "issuanceId": 1 }, { unique: true })
db.book_issuances.createIndex({ "studentId": 1 })
db.book_issuances.createIndex({ "status": 1 })
```

### Step 3: Configure Twilio SMS (Optional)

1. Sign up at https://www.twilio.com/
2. Get your Account SID and Auth Token
3. Get a Twilio phone number
4. Update `config/sms.properties`:
```properties
twilio.account.sid=YOUR_ACTUAL_ACCOUNT_SID
twilio.auth.token=YOUR_ACTUAL_AUTH_TOKEN
twilio.phone.number=YOUR_TWILIO_PHONE_NUMBER
sms.enabled=true
```

### Step 4: Install Apache Tomcat

1. Download Tomcat 9 from https://tomcat.apache.org/
2. Extract to a directory (e.g., C:\tomcat or /opt/tomcat)
3. Set CATALINA_HOME environment variable
4. Start Tomcat:
   - Windows: `bin\startup.bat`
   - Linux: `bin/startup.sh`

### Step 5: Setup Project in NetBeans

1. Open NetBeans IDE
2. File → Open Project
3. Navigate to LibraryMate folder
4. Right-click project → Resolve Project Problems (if any)
5. Add required libraries:
   - MongoDB Java Driver (4.8.0+)
   - Twilio SDK (9.2.0+)
   - Servlet API (4.0.1)
   - GSON (2.10.1)

### Step 6: Configure Database Connection

1. Edit `config/database.properties`:
```properties
mongodb.host=localhost
mongodb.port=27017
mongodb.database=librarymate_db
mongodb.username=
mongodb.password=
```

2. If MongoDB has authentication enabled:
```properties
mongodb.username=your_username
mongodb.password=your_password
```

### Step 7: Build the Project

#### Using NetBeans:
1. Right-click project → Clean and Build
2. Check for compilation errors
3. WAR file will be created in `dist/LibraryMate.war`

#### Using Maven:
```bash
mvn clean package
```

### Step 8: Deploy to Tomcat

#### Method 1: NetBeans Deployment
1. Right-click project → Properties
2. Run → Server: Select Apache Tomcat
3. Right-click project → Run
4. Application will deploy automatically

#### Method 2: Manual Deployment
1. Copy `dist/LibraryMate.war` to Tomcat's `webapps` folder
2. Restart Tomcat
3. Tomcat will auto-extract the WAR file

#### Method 3: Tomcat Manager
1. Access Tomcat Manager: http://localhost:8080/manager
2. Login with admin credentials
3. Deploy WAR file through web interface

### Step 9: Verify Deployment

1. Open browser and navigate to:
   ```
   http://localhost:8080/LibraryMate
   ```

2. You should see the LibraryMate homepage

3. Check Tomcat logs for any errors:
   - Windows: `logs\catalina.out`
   - Linux: `logs/catalina.out`

## Configuration

### Application Settings

Edit `config/app.properties`:
```properties
# Fine Configuration
fine.per.day=10
fine.max.amount=500

# Book Issuance Settings
book.issue.days=14
book.max.per.student=3
```

### SMS Settings

Edit `config/sms.properties`:
```properties
sms.enabled=true
sms.overdue.reminder.days=1
```

## Testing

### 1. Add Sample Data

Use MongoDB shell to add test data:

```javascript
// Add sample student
db.students.insertOne({
    studentId: "ST001",
    name: "John Doe",
    email: "john@example.com",
    phone: "+919876543210",
    department: "Computer Science",
    course: "B.Tech",
    semester: 6,
    registrationDate: new Date(),
    isActive: true
})

// Add sample book
db.books.insertOne({
    bookId: "BK001",
    title: "Marketing Management",
    author: "Philip Kotler",
    isbn: "978-0134236933",
    category: "Marketing",
    totalCopies: 5,
    availableCopies: 5,
    publisher: "Pearson",
    addedDate: new Date(),
    isActive: true
})
```

### 2. Test Features

1. Add a student through the web interface
2. Add a book through the web interface
3. Issue a book to a student
4. Return a book
5. Check overdue books
6. Test SMS functionality (if configured)

## Troubleshooting

### MongoDB Connection Issues
- Verify MongoDB is running: `systemctl status mongod`
- Check connection string in `database.properties`
- Ensure firewall allows port 27017

### Tomcat Deployment Issues
- Check Tomcat logs in `logs/catalina.out`
- Verify JAVA_HOME is set correctly
- Ensure port 8080 is not in use

### SMS Not Working
- Verify Twilio credentials
- Check account balance
- Ensure phone numbers are in E.164 format (+[country code][number])
- Check `sms.enabled=true` in config

### Application Errors
- Check browser console for JavaScript errors
- Verify all JAR files are in `WEB-INF/lib`
- Check servlet mappings in `web.xml`

## Production Deployment

### Security Considerations

1. Change default admin credentials
2. Enable HTTPS/SSL
3. Configure MongoDB authentication
4. Use environment variables for sensitive data
5. Enable Tomcat security manager
6. Regular backups of MongoDB

### Performance Optimization

1. Enable MongoDB connection pooling
2. Configure Tomcat thread pool
3. Enable GZIP compression
4. Use CDN for static assets
5. Implement caching strategies

### Monitoring

1. Setup MongoDB monitoring
2. Configure Tomcat JMX monitoring
3. Implement application logging
4. Setup SMS delivery tracking
5. Monitor disk space and memory

## Backup and Recovery

### Database Backup
```bash
mongodump --db librarymate_db --out /backup/$(date +%Y%m%d)
```

### Database Restore
```bash
mongorestore --db librarymate_db /backup/20241111/librarymate_db
```

### Automated Backup Script
Create a cron job (Linux) or Task Scheduler (Windows) to run daily backups.

## Support

For issues and questions:
- Check logs in `logs/` directory
- Review MongoDB logs
- Check Tomcat logs
- Verify configuration files

## License

Educational Use Only - For College Library Management
