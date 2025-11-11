# LibraryMate - MySQL Setup Guide

## Easy Alternative to MongoDB

Since MongoDB installation is difficult, we've converted LibraryMate to use **MySQL**, which is much easier to set up!

## Option 1: Use XAMPP (Easiest - Recommended)

### Step 1: Download and Install XAMPP
1. Download XAMPP from: https://www.apachefriends.org/
2. Install XAMPP (it includes MySQL/MariaDB)
3. Start XAMPP Control Panel
4. Click "Start" for **MySQL** module

### Step 2: Create Database
1. Open your browser and go to: `http://localhost/phpmyadmin`
2. Click "SQL" tab at the top
3. Copy and paste the entire content from `scripts/create_database.sql`
4. Click "Go" button
5. Done! Database and sample data are created!

### Step 3: Configure LibraryMate
The default settings in `config/database.properties` should work:
```properties
mysql.host=localhost
mysql.port=3306
mysql.database=librarymate_db
mysql.username=root
mysql.password=
```

**That's it! You're ready to run LibraryMate!**

---

## Option 2: Use MySQL Workbench

### Step 1: Download MySQL
1. Download MySQL Community Server: https://dev.mysql.com/downloads/mysql/
2. Download MySQL Workbench: https://dev.mysql.com/downloads/workbench/
3. Install both

### Step 2: Setup Database
1. Open MySQL Workbench
2. Connect to local MySQL server
3. File → Open SQL Script
4. Select `scripts/create_database.sql`
5. Click Execute (⚡ icon)
6. Database created with sample data!

### Step 3: Configure
Update `config/database.properties` with your MySQL password:
```properties
mysql.username=root
mysql.password=your_mysql_password
```

---

## Option 3: Command Line (Advanced)

### Windows
```cmd
# Navigate to MySQL bin directory
cd "C:\Program Files\MySQL\MySQL Server 8.0\bin"

# Login to MySQL
mysql -u root -p

# Run the script
source C:\path\to\LibraryMate\scripts\create_database.sql
```

### Linux
```bash
# Login to MySQL
mysql -u root -p

# Run the script
source /path/to/LibraryMate/scripts/create_database.sql
```

---

## Verify Installation

### Using phpMyAdmin (XAMPP)
1. Go to `http://localhost/phpmyadmin`
2. Click on `librarymate_db` database
3. You should see 3 tables:
   - students (5 rows)
   - books (14 rows)
   - book_issuances (5 rows)

### Using MySQL Workbench
1. Open MySQL Workbench
2. Connect to server
3. Expand `librarymate_db` schema
4. Click on each table to view data

### Using Command Line
```sql
mysql -u root -p

USE librarymate_db;

SELECT COUNT(*) FROM students;    -- Should return 5
SELECT COUNT(*) FROM books;       -- Should return 14
SELECT COUNT(*) FROM book_issuances; -- Should return 5

-- View sample data
SELECT * FROM students;
SELECT * FROM books WHERE category = 'Marketing';
SELECT * FROM book_issuances WHERE status = 'ISSUED';
```

---

## Sample Data Included

### Students (5)
- ST001 - John Doe (Computer Science, B.Tech)
- ST002 - Jane Smith (Business Administration, MBA)
- ST003 - Mike Johnson (Mechanical Engineering, B.E)
- ST004 - Sarah Williams (Commerce, B.Com)
- ST005 - David Brown (Physics, M.Sc)

### Books (14)
- **Marketing** (3 books): Marketing Management, Digital Marketing, Consumer Behavior
- **Accountancy** (3 books): Financial Accounting, Cost Accounting, Auditing
- **Science** (4 books): Physics, Chemistry, Biology, Calculus
- **Fiction** (4 books): The Great Gatsby, 1984, Pride and Prejudice, etc.

### Book Issuances (5)
- 2 active issuances
- 1 overdue (with ₹130 fine)
- 1 returned
- 1 active

---

## Troubleshooting

### MySQL won't start in XAMPP
- Check if port 3306 is already in use
- Click "Config" → "my.ini" and change port if needed
- Restart XAMPP

### Can't connect to database
- Verify MySQL is running (green in XAMPP)
- Check username/password in `config/database.properties`
- Try accessing phpMyAdmin to test connection

### "Access denied" error
- Default XAMPP MySQL has no password
- If you set a password, update `config/database.properties`

### Database not found
- Make sure you ran the `create_database.sql` script
- Check if database exists in phpMyAdmin

---

## Required JAR File

You need the MySQL JDBC driver:

### Download
- **File**: mysql-connector-java-8.0.33.jar
- **Link**: https://dev.mysql.com/downloads/connector/j/
- **Or**: https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.33/

### Installation
1. Download the JAR file
2. Copy to `LibraryMate/lib/` folder
3. In NetBeans: Right-click project → Properties → Libraries → Add JAR/Folder
4. Select the mysql-connector-java JAR
5. Click OK

---

## Quick Start Summary

1. **Install XAMPP** (5 minutes)
2. **Start MySQL** in XAMPP Control Panel
3. **Open phpMyAdmin** (`http://localhost/phpmyadmin`)
4. **Run SQL script** (copy/paste from `scripts/create_database.sql`)
5. **Add MySQL JAR** to NetBeans project
6. **Run LibraryMate!**

**Total time: 10-15 minutes**

---

## Next Steps

After MySQL is set up:

1. ✅ Database is ready with sample data
2. Add MySQL JDBC driver JAR to project
3. Open project in NetBeans
4. Clean and Build
5. Run on Tomcat
6. Access at: `http://localhost:8080/LibraryMate`

---

## Advantages of MySQL over MongoDB

✅ **Easier to install** - XAMPP includes everything
✅ **GUI tools** - phpMyAdmin is user-friendly
✅ **More familiar** - SQL is widely known
✅ **Better support** - More documentation available
✅ **Lightweight** - Uses less resources
✅ **Free** - Completely free and open source

---

## Need Help?

- **XAMPP not starting?** Check if ports 80 and 3306 are free
- **Can't access phpMyAdmin?** Make sure Apache and MySQL are both running
- **Database errors?** Verify the SQL script ran successfully
- **Connection issues?** Check `config/database.properties` settings

---

**You're all set! MySQL is much easier than MongoDB for this project. 🎉**
