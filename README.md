# 📚 LibraryMate - University Library Management System

<div align="center">

**A comprehensive, automated library management system for colleges and universities**

[![Java](https://img.shields.io/badge/Java-8%2B-orange.svg)](https://www.java.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-4.4%2B-green.svg)](https://www.mongodb.com/)
[![License](https://img.shields.io/badge/License-Educational-blue.svg)](LICENSE)

[Features](#-features) • [Quick Start](#-quick-start) • [Documentation](#-documentation) • [Screenshots](#-screenshots)

</div>

---

## 🎯 Overview

LibraryMate is a production-ready library management system designed specifically for college environments. It automates book inventory management, student tracking, and overdue notifications with intelligent fine calculation and SMS alerts.

### Why LibraryMate?

- ✅ **Fully Automated**: Automatic fine calculation and SMS reminders
- ✅ **Easy to Deploy**: Complete setup in 15 minutes
- ✅ **Production Ready**: Built for long-term use in educational institutions
- ✅ **Well Documented**: Comprehensive guides for deployment and usage
- ✅ **Scalable**: MongoDB-based architecture supports growth

## ✨ Features

### Core Functionality
- 📖 **Book Management**: Add, update, categorize books (Marketing, Accountancy, Science, Fiction)
- 👨‍🎓 **Student Records**: Complete student profile and borrowing history
- 📤 **Book Issuance**: Issue books with automatic due date calculation (14 days)
- 📥 **Returns Processing**: Automatic fine calculation on late returns
- 💰 **Fine Management**: ₹10/day fine (max ₹500 per book)
- ⚠️ **Overdue Tracking**: Real-time overdue monitoring with statistics
- 📱 **SMS Alerts**: Automated reminders via Twilio integration
- 📊 **Reports**: Comprehensive inventory and activity reports

### Technical Highlights
- **Backend**: Core Java with Servlets
- **Frontend**: HTML5, CSS3, JavaScript (no frameworks)
- **Database**: MySQL (easy setup with XAMPP)
- **Server**: Apache Tomcat 9+
- **SMS**: Twilio API integration
- **Build**: Maven/Ant support

## 🚀 Quick Start

### Prerequisites
```bash
✓ JDK 8 or higher
✓ NetBeans IDE 12+
✓ MySQL (via XAMPP - easiest) or MySQL Server
✓ Apache Tomcat 9+
```

### Installation (3 Steps)

**1. Setup Database**
```bash
# Option A: Using XAMPP (Recommended - Easiest!)
# 1. Install XAMPP from https://www.apachefriends.org/
# 2. Start MySQL in XAMPP Control Panel
# 3. Open http://localhost/phpmyadmin
# 4. Go to SQL tab and run scripts/create_database.sql

# Option B: Using MySQL Command Line
mysql -u root -p < scripts/create_database.sql
```

**2. Configure Application**
```properties
# Edit config/database.properties (default settings work with XAMPP)
mysql.host=localhost
mysql.port=3306
mysql.database=librarymate_db
mysql.username=root
mysql.password=
```

**3. Deploy**
```bash
# Open in NetBeans
# Right-click project → Run
# Access at: http://localhost:8080/LibraryMate
```

📖 **Detailed Setup**: See [MYSQL_SETUP.md](MYSQL_SETUP.md) for complete MySQL instructions

## 📁 Project Structure

```
LibraryMate/
├── src/com/librarymate/          # Java source code
│   ├── models/                   # Data models (Student, Book, BookIssuance)
│   ├── dao/                      # Database access layer
│   ├── services/                 # Business logic (Library, SMS, Scheduler)
│   ├── servlets/                 # HTTP request handlers
│   └── utils/                    # Utilities (Config, DB Connection)
├── web/                          # Frontend files
│   ├── css/style.css            # Styling
│   ├── js/*.js                  # JavaScript files
│   ├── index.html               # Dashboard
│   └── *.html                   # Feature pages
├── config/                       # Configuration files
│   ├── app.properties           # Application settings
│   ├── database.properties      # MongoDB config
│   └── sms.properties           # Twilio SMS config
├── scripts/                      # Utility scripts
│   ├── sample_data.js           # Sample data for testing
│   ├── backup.sh                # Backup script (Linux)
│   └── backup.bat               # Backup script (Windows)
├── lib/                          # External JAR files
├── nbproject/                    # NetBeans project files
├── pom.xml                       # Maven configuration
└── build.xml                     # Ant build file
```

## 📚 Documentation

| Document | Description |
|----------|-------------|
| [QUICKSTART.md](QUICKSTART.md) | Get started in 15 minutes |
| [DEPLOYMENT.md](DEPLOYMENT.md) | Complete deployment guide |
| [USAGE_GUIDE.md](USAGE_GUIDE.md) | User manual for librarians |
| [DATABASE_SCHEMA.md](DATABASE_SCHEMA.md) | Database structure and queries |
| [PROJECT_OVERVIEW.md](PROJECT_OVERVIEW.md) | Architecture and design |
| [FEATURES.md](FEATURES.md) | Complete feature list |

## 🎨 Screenshots

### Dashboard
Clean, intuitive interface with quick access to all features

### Book Management
- Add/update books with categorization
- Real-time availability tracking
- Filter by category (Marketing, Accountancy, Science, Fiction)

### Student Management
- Complete student profiles
- Borrowing history
- Contact information for SMS alerts

### Overdue Management
- Real-time overdue tracking
- Automatic fine calculation
- Bulk SMS reminder sending

## 🔧 Configuration

### Fine Settings
```properties
fine.per.day=10              # Daily fine in rupees
fine.max.amount=500          # Maximum fine per book
```

### Issuance Settings
```properties
book.issue.days=14           # Days before due date
book.max.per.student=3       # Maximum books per student
```

### SMS Settings (Optional)
```properties
twilio.account.sid=YOUR_SID
twilio.auth.token=YOUR_TOKEN
twilio.phone.number=+1234567890
sms.enabled=true
```

## 📊 Database Schema

### Collections

**students** - Student information
```javascript
{
  studentId: "ST001",
  name: "John Doe",
  email: "john@college.edu",
  phone: "+919876543210",
  department: "Computer Science",
  course: "B.Tech",
  semester: 6
}
```

**books** - Book inventory
```javascript
{
  bookId: "BK001",
  title: "Marketing Management",
  author: "Philip Kotler",
  category: "Marketing",
  totalCopies: 5,
  availableCopies: 3
}
```

**book_issuances** - Issuance tracking
```javascript
{
  issuanceId: "ISS001",
  studentId: "ST001",
  bookId: "BK001",
  issueDate: ISODate("2024-11-01"),
  dueDate: ISODate("2024-11-15"),
  status: "ISSUED",
  fineAmount: 0.0
}
```

## 🔐 Security Features

- ✅ Input validation on all forms
- ✅ SQL injection prevention (MongoDB parameterized queries)
- ✅ Secure session management
- ✅ Configuration file security
- ✅ Role-based access control (future)

## 📈 Performance

- **Database Indexing**: All frequently queried fields indexed
- **Connection Pooling**: Efficient MongoDB connection management
- **Optimized Queries**: SQL-like queries optimized for performance
- **Caching**: Static data cached for faster access

## 🛠️ Development

### Build with Maven
```bash
mvn clean package
```

### Build with Ant
```bash
ant clean
ant war
```

### Run Tests
```bash
mvn test
```

## 📦 Dependencies

### Core
- MongoDB Java Driver 4.8.0+
- Servlet API 4.0.1
- GSON 2.10.1

### Optional
- Twilio SDK 9.2.0+ (for SMS)
- SLF4J 2.0.7 (for logging)

See [lib/README.md](lib/README.md) for download links

## 🔄 Backup & Maintenance

### Automated Backup
```bash
# Linux
./scripts/backup.sh

# Windows
scripts\backup.bat
```

### Manual Backup
```bash
mongodump --db librarymate_db --out /backup/$(date +%Y%m%d)
```

### Restore
```bash
mongorestore --db librarymate_db /backup/20241111/librarymate_db
```

## 🚀 Deployment Options

### Local Development
- Local Tomcat + MongoDB
- Test Twilio account

### Production
- Dedicated server
- MongoDB replica set
- SSL/TLS enabled
- Automated backups

### Cloud
- AWS EC2 / Azure VM
- MongoDB Atlas
- CloudWatch monitoring

## 🎓 Use Cases

Perfect for:
- ✅ College libraries
- ✅ University libraries
- ✅ School libraries
- ✅ Department libraries
- ✅ Research libraries

## 🤝 Support

### Getting Help
1. Check [USAGE_GUIDE.md](USAGE_GUIDE.md) for feature documentation
2. Review [DEPLOYMENT.md](DEPLOYMENT.md) for technical issues
3. See [DATABASE_SCHEMA.md](DATABASE_SCHEMA.md) for database queries

### Common Issues
- **MongoDB connection**: Check if MongoDB is running
- **Port conflicts**: Ensure port 8080 is available
- **Build errors**: Verify all JAR files in lib/ directory
- **SMS not working**: Check Twilio credentials and balance

## 🔮 Future Enhancements

### Planned Features
- 📱 Mobile application (Android/iOS)
- 🔍 Advanced search and filtering
- 📧 Email notifications
- 📊 Advanced analytics dashboard
- 🎯 Book reservation system
- 📷 QR code/Barcode scanning
- 🌐 Multi-language support
- 🔗 University ERP integration

## 📄 License

Educational Use Only - Designed for college library management

## 👥 Contributing

This project is designed for educational purposes. Feel free to fork and customize for your institution's needs.

## 🙏 Acknowledgments

Built with focus on:
- Ease of use for librarians
- Automation to reduce manual work
- Reliability for long-term use
- Scalability for growing libraries
- Comprehensive documentation

---

<div align="center">

**Made with ❤️ for Educational Institutions**

[Documentation](QUICKSTART.md) • [Report Bug](issues) • [Request Feature](issues)

</div>
