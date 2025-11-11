# LibraryMate - Quick Start Guide

Get LibraryMate up and running in 15 minutes!

## Prerequisites Checklist

- [ ] JDK 8 or higher installed
- [ ] NetBeans IDE installed
- [ ] MongoDB installed and running
- [ ] Apache Tomcat 9+ installed

## Quick Setup (5 Steps)

### Step 1: Install MongoDB (5 minutes)

**Windows:**
```bash
# Download from https://www.mongodb.com/try/download/community
# Run installer, install as Windows Service
# MongoDB will start automatically
```

**Linux:**
```bash
sudo apt-get update
sudo apt-get install -y mongodb-org
sudo systemctl start mongod
sudo systemctl enable mongod
```

**Verify:**
```bash
mongo --version
```

### Step 2: Setup Database (2 minutes)

```bash
# Open MongoDB shell
mongo

# Create database and load sample data
use librarymate_db

# Exit mongo shell
exit

# Load sample data
mongo librarymate_db < scripts/sample_data.js
```

### Step 3: Configure Application (2 minutes)

Edit `config/database.properties`:
```properties
mongodb.host=localhost
mongodb.port=27017
mongodb.database=librarymate_db
mongodb.username=
mongodb.password=
```

**Optional - SMS Configuration:**
Edit `config/sms.properties`:
```properties
sms.enabled=false  # Set to true if you have Twilio account
```

### Step 4: Open in NetBeans (3 minutes)

1. Open NetBeans IDE
2. File → Open Project
3. Navigate to LibraryMate folder
4. Select the project
5. Wait for NetBeans to load dependencies

**Add Libraries (if needed):**
1. Right-click project → Properties
2. Libraries → Add JAR/Folder
3. Add JARs from `lib/` folder (see lib/README.md)

### Step 5: Build and Run (3 minutes)

1. Right-click project → Clean and Build
2. Wait for build to complete
3. Right-click project → Run
4. NetBeans will deploy to Tomcat automatically
5. Browser opens at: `http://localhost:8080/LibraryMate`

## Verify Installation

### Test the Application

1. **Homepage**: Should see LibraryMate dashboard
2. **Books**: Click "Books" → Should see sample books
3. **Students**: Click "Students" → Should see sample students
4. **Issue Book**: Try issuing a book (use ST001 and BK001)
5. **Return Book**: Try returning a book

### Check Database

```bash
mongo librarymate_db
db.students.count()  # Should return 5
db.books.count()     # Should return 14
db.book_issuances.count()  # Should return 5
```

## Default Test Data

### Sample Students
- ST001 - John Doe
- ST002 - Jane Smith
- ST003 - Mike Johnson
- ST004 - Sarah Williams
- ST005 - David Brown

### Sample Books
- BK001 - Marketing Management (Marketing)
- BK004 - Financial Accounting (Accountancy)
- BK007 - Physics Fundamentals (Science)
- BK011 - The Great Gatsby (Fiction)

## Common Issues & Solutions

### Issue: MongoDB not running
```bash
# Windows
net start MongoDB

# Linux
sudo systemctl start mongod
```

### Issue: Port 8080 already in use
```bash
# Change Tomcat port in server.xml
# Or stop the process using port 8080
```

### Issue: Build fails - Missing libraries
```bash
# Use Maven to download dependencies
mvn clean install

# Or manually download JARs (see lib/README.md)
```

### Issue: Cannot connect to database
```bash
# Check MongoDB is running
mongo --eval "db.version()"

# Check connection settings in config/database.properties
```

## Next Steps

### For Development
1. Read **USAGE_GUIDE.md** for feature details
2. Review **DATABASE_SCHEMA.md** for database structure
3. Check **PROJECT_OVERVIEW.md** for architecture

### For Production
1. Follow **DEPLOYMENT.md** for production setup
2. Configure SSL/TLS
3. Enable MongoDB authentication
4. Setup automated backups
5. Configure Twilio for SMS

## Quick Commands Reference

### MongoDB
```bash
# Start MongoDB
sudo systemctl start mongod

# Stop MongoDB
sudo systemctl stop mongod

# MongoDB shell
mongo

# Backup database
mongodump --db librarymate_db --out /backup/$(date +%Y%m%d)

# Restore database
mongorestore --db librarymate_db /backup/20241111/librarymate_db
```

### Maven
```bash
# Clean and build
mvn clean package

# Run tests
mvn test

# Install dependencies
mvn install
```

### Tomcat
```bash
# Start Tomcat (Windows)
bin\startup.bat

# Start Tomcat (Linux)
bin/startup.sh

# Stop Tomcat (Windows)
bin\shutdown.bat

# Stop Tomcat (Linux)
bin/shutdown.sh
```

## Testing Workflows

### Test Book Issuance
1. Go to "Issue Book"
2. Enter Student ID: ST001
3. Enter Book ID: BK002
4. Click "Issue Book"
5. Verify success message

### Test Book Return
1. Go to "Return Book"
2. Enter Issuance ID: ISS001
3. Click "Return Book"
4. Check if fine is calculated (if overdue)

### Test Overdue Management
1. Go to "Overdue"
2. View overdue books list
3. Check fine calculations
4. (Optional) Click "Send SMS Reminders"

## Configuration Quick Reference

### Fine Settings
```properties
fine.per.day=10          # Daily fine in rupees
fine.max.amount=500      # Maximum fine per book
```

### Issuance Settings
```properties
book.issue.days=14       # Days before due date
book.max.per.student=3   # Max books per student
```

### SMS Settings
```properties
sms.enabled=false        # Enable/disable SMS
```

## Getting Help

### Documentation
- **README.md** - Project introduction
- **DEPLOYMENT.md** - Detailed deployment guide
- **USAGE_GUIDE.md** - Complete user manual
- **DATABASE_SCHEMA.md** - Database documentation
- **PROJECT_OVERVIEW.md** - Architecture overview

### Troubleshooting
1. Check Tomcat logs: `logs/catalina.out`
2. Check MongoDB logs: `/var/log/mongodb/mongod.log`
3. Check browser console for JavaScript errors
4. Verify all configuration files

### Sample Data
- Located in: `scripts/sample_data.js`
- Reload anytime: `mongo librarymate_db < scripts/sample_data.js`

## Success Checklist

- [ ] MongoDB running and accessible
- [ ] Database created with sample data
- [ ] Application builds without errors
- [ ] Application runs on Tomcat
- [ ] Can access homepage
- [ ] Can view books and students
- [ ] Can issue a book
- [ ] Can return a book
- [ ] Can view overdue books

## Congratulations! 🎉

You now have LibraryMate running! Start managing your library efficiently.

For detailed usage instructions, see **USAGE_GUIDE.md**
