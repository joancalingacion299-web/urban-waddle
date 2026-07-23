# CareFlow Hospital - Installation Guide

## Quick Start (5 minutes)

### Option A: Using PHP Built-in Server (Easiest)

1. **Start the PHP server:**
   ```bash
   cd "c:\Users\calin\OneDrive\Attachments\HospitalQueueingSystem"
   php -S localhost:8080
   ```

2. **Create the database:**
   - Open phpMyAdmin or MySQL console
   - Create database: `careflow_hospital`
   - Import file: `database.sql`
   
   Or via command line:
   ```bash
   mysql -u root < database.sql
   ```

3. **Update configuration:**
   - Edit `backend/config/config.php`
   - Set your MySQL username and password
   - Update `JWT_SECRET` for production

4. **Access the application:**
   - Open browser: `http://localhost:8080/index_new.html`
   - Login with: `admin` / `admin123`

---

## Detailed Installation Steps

### Prerequisites Check

Verify you have:
- ✅ PHP 7.4 or higher
- ✅ MySQL 5.7 or higher
- ✅ Modern web browser (Chrome, Firefox, Edge, Safari)

Check PHP version:
```bash
php --version
```

Check MySQL version:
```bash
mysql --version
```

### Step 1: Database Setup

#### Method 1: Using MySQL Command Line

```bash
# Connect to MySQL
mysql -u root -p

# Paste the following:
CREATE DATABASE careflow_hospital;
USE careflow_hospital;
SOURCE database.sql;
EXIT;
```

#### Method 2: Using phpMyAdmin

1. Open phpMyAdmin (usually at `http://localhost/phpmyadmin`)
2. Click "New" on the left sidebar
3. Enter database name: `careflow_hospital`
4. Click "Create"
5. Click on the new database
6. Go to "Import" tab
7. Select file: `database.sql`
8. Click "Import"

#### Verify Database

```bash
mysql -u root -p careflow_hospital
SHOW TABLES;
# Should display 9 tables
EXIT;
```

### Step 2: Configure Backend

Edit `backend/config/config.php`:

```php
// Line 5-9: Update with your database credentials
define('DB_HOST', 'localhost');
define('DB_USER', 'root');           // Your MySQL username
define('DB_PASS', '');                // Your MySQL password
define('DB_NAME', 'careflow_hospital');
define('DB_PORT', 3306);

// Line 17: For production, change to 'production'
define('APP_ENV', 'development');

// Line 20: IMPORTANT - Change this in production!
define('JWT_SECRET', 'change-this-secret-key-in-production');
```

### Step 3: Create Upload Directory

Create folder for file uploads:
```bash
mkdir uploads
mkdir logs
chmod 755 uploads
chmod 755 logs
```

Or via File Explorer:
1. Create folder `uploads` in project root
2. Create folder `logs` in project root

### Step 4: Start Web Server

#### Option A: PHP Built-in (Development)
```bash
cd "path/to/HospitalQueueingSystem"
php -S localhost:8080
```

Browser: `http://localhost:8080/index_new.html`

#### Option B: Apache (Requires Setup)

1. Configure Apache virtual host to point to project root
2. Enable mod_rewrite
3. Restart Apache
4. Navigate to your configured domain

#### Option C: Nginx (Requires Setup)

Configure Nginx server block with PHP-FPM

### Step 5: Verify Installation

1. **Check database:**
   ```bash
   mysql -u root careflow_hospital -e "SELECT COUNT(*) FROM users;"
   ```
   Should show: `1` (the admin user)

2. **Check PHP:**
   - Verify PHP extensions: `php -m | grep -i (pdo|mysql|json)`
   - Should include: PDO and JSON

3. **Test login:**
   - Navigate to login page
   - Login as admin / admin123
   - Should see admin dashboard

---

## First-Time Setup Tasks

After installation, do the following:

### 1. Create Receptionist Account (via Admin Panel)

1. Login as admin
2. Go to "Users" section
3. Click "Add User"
4. Fill form:
   - Full Name: `John Receptionist`
   - Email: `receptionist@hospital.local`
   - Username: `receptionist`
   - Password: (strong password)
   - Role: `receptionist`
5. Click "Create User"

### 2. Create Doctor Account

1. In Users section, click "Add User"
2. Fill form:
   - Full Name: `Dr. Jane Smith`
   - Email: `doctor@hospital.local`
   - Username: `doctor`
   - Password: (strong password)
   - Role: `doctor`
   - Department: `General Medicine`
3. Click "Create User"

### 3. Create Departments (if needed)

1. Go to "Departments" section
2. Click "Add Department"
3. Add departments: General Medicine, Pediatrics, Dentistry, etc.

---

## Troubleshooting

### "Connection refused" error

**Problem:** Cannot connect to database

**Solution:**
1. Check MySQL is running:
   ```bash
   # Windows
   net start MySQL80
   # or your MySQL version
   
   # macOS
   brew services start mysql
   
   # Linux
   sudo systemctl start mysql
   ```

2. Verify credentials in `config.php`

3. Test connection:
   ```bash
   mysql -h localhost -u root -p careflow_hospital -e "SELECT 1;"
   ```

### "Table doesn't exist" error

**Problem:** Database not imported properly

**Solution:**
1. Drop and recreate database:
   ```bash
   mysql -u root -p
   DROP DATABASE careflow_hospital;
   CREATE DATABASE careflow_hospital;
   SOURCE database.sql;
   EXIT;
   ```

2. Verify all tables:
   ```bash
   mysql -u root careflow_hospital -e "SHOW TABLES;"
   ```

### Login page shows but credentials don't work

**Problem:** User not created or password incorrect

**Solution:**
1. Verify admin user exists:
   ```bash
   mysql -u root careflow_hospital -e "SELECT username FROM users WHERE role='admin';"
   ```

2. Re-import database to reset admin password

### "Access Denied" for MySQL

**Problem:** Wrong password or user doesn't exist

**Solution:**
1. Try logging in directly:
   ```bash
   mysql -h localhost -u root -p
   ```

2. If successful, your credentials in `config.php` are wrong

3. Create MySQL user if needed:
   ```bash
   mysql -u root -p
   CREATE USER 'careflow'@'localhost' IDENTIFIED BY 'secure_password';
   GRANT ALL PRIVILEGES ON careflow_hospital.* TO 'careflow'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   ```

### 404 errors on pages

**Problem:** Pages not found

**Solution:**
1. Check file paths in your system
2. Ensure all directories were created properly
3. Verify web server document root is set to project folder

### Styles not loading

**Problem:** CSS not applied

**Solution:**
1. Check browser developer console for 404 errors
2. Verify relative paths in HTML files
3. Hard refresh page: `Ctrl+Shift+R` (Windows/Linux) or `Cmd+Shift+R` (Mac)

---

## Production Deployment

### Before Going Live:

1. **Change Secret Key:**
   ```php
   define('JWT_SECRET', 'generate-a-long-random-secure-string');
   ```

2. **Enable HTTPS:**
   - Get SSL certificate (Let's Encrypt is free)
   - Configure web server for SSL

3. **Set Environment to Production:**
   ```php
   define('APP_ENV', 'production');
   ```

4. **Database Security:**
   ```bash
   # Create dedicated database user
   CREATE USER 'careflow_user'@'localhost' IDENTIFIED BY 'strong_password';
   GRANT SELECT, INSERT, UPDATE, DELETE ON careflow_hospital.* TO 'careflow_user'@'localhost';
   FLUSH PRIVILEGES;
   ```

5. **File Permissions:**
   ```bash
   chmod 640 backend/config/config.php
   chmod 755 uploads logs
   ```

6. **Backup Regularly:**
   - Use admin panel backup feature
   - Store backups securely off-server

7. **Update PHP & MySQL:**
   - Keep to latest secure versions
   - Enable auto-updates if possible

---

## Support & Help

### Common Issues:

| Issue | Solution |
|-------|----------|
| PHP not found | Add PHP to PATH environment variable |
| Port 8080 in use | Use different port: `php -S localhost:9000` |
| Database too large | Archive old data, then backup |
| Slow performance | Add database indexes, enable caching |
| Login loop | Check session settings in PHP |

### Getting Help:

1. Check error logs: `logs/error.log`
2. Review browser console: F12 → Console tab
3. Test database connection directly
4. Verify file permissions
5. Check PHP error reporting is enabled

---

## Next Steps

After successful installation:

1. Create test user accounts for each role
2. Test complete workflow:
   - Register patient (receptionist)
   - Call next patient (receptionist)
   - Complete consultation (receptionist/doctor)
3. Review database with demo data
4. Customize theme colors if desired
5. Configure email for notifications (optional)
6. Set up regular backups

---

## Security Checklist

- [ ] Changed `JWT_SECRET` from default
- [ ] Set `APP_ENV` to 'production' if live
- [ ] Enabled HTTPS/SSL
- [ ] Created strong database user password
- [ ] Set proper file permissions (640/755)
- [ ] Disabled PHP error display in production
- [ ] Set up regular backups
- [ ] Reviewed and updated security headers
- [ ] Tested SQL injection protections
- [ ] Enabled audit logging

---

**Installation Complete!** 🎉

Your CareFlow Hospital System is now ready to use.

Access it at: `http://localhost:8080/index_new.html`

Default Login: `admin` / `admin123`
