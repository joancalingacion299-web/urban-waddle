# CareFlow Hospital - Full-Stack Queueing System

## Overview

CareFlow is a premium, enterprise-grade Hospital Queueing Management System with role-based access control. It features separate dashboards for Administrators, Receptionists, Doctors, and Patients with complete CRUD operations, real-time queue management, and comprehensive reporting.

## Features

### 🔐 Security Features
- Password hashing with bcrypt
- Role-based authentication (RBAC)
- CSRF token protection
- SQL injection prevention via prepared statements
- Session management with timeout
- Rate limiting on login attempts
- Audit logging for all actions
- Input validation and sanitization

### 👥 User Roles

#### Administrator
- User management (create, edit, delete accounts)
- Department management
- Doctor management
- Analytics and reporting
- Database backup/restore
- Audit log viewing
- System settings

#### Receptionist
- Patient registration
- Queue generation
- Appointment scheduling
- Call next patient
- Skip patient
- Mark consultation complete
- Print queue tickets
- Daily reports

#### Doctor
- View today's queue
- View patient information
- Complete consultations
- Add medical records
- View medical history
- Manage appointments

#### Patient
- Online registration
- Login to portal
- Book appointments
- View queue status
- Receive notifications
- View appointment history
- Access medical records

### 📊 Database Tables
- `users` - User accounts with roles
- `patients` - Patient information
- `doctors` - Doctor profiles
- `departments` - Hospital departments
- `appointments` - Appointment scheduling
- `queue` - Queue management
- `medical_records` - Patient medical history
- `notifications` - System notifications
- `audit_logs` - Audit trail

## Installation & Setup

### Prerequisites
- PHP 7.4+ (built-in server or Apache/Nginx)
- MySQL 5.7+ or MariaDB
- Modern web browser

### Step 1: Database Setup

1. Create MySQL database:
```bash
mysql -u root -p
CREATE DATABASE careflow_hospital;
USE careflow_hospital;
SOURCE database.sql;
EXIT;
```

Or through phpMyAdmin:
1. Create new database: `careflow_hospital`
2. Import file: `database.sql`

### Step 2: Configure Backend

Edit `backend/config/config.php`:
```php
define('DB_HOST', 'localhost');
define('DB_USER', 'root');
define('DB_PASS', '');      // Your MySQL password
define('DB_NAME', 'careflow_hospital');
define('JWT_SECRET', 'your-secret-key'); // Change in production
```

### Step 3: Start Web Server

Using PHP built-in server:
```bash
cd HospitalQueueingSystem
php -S localhost:8080
```

Or configure Apache/Nginx to serve the directory.

### Step 4: Access Application

Open browser and navigate to:
```
http://localhost:8080/index_new.html
```

### Demo Credentials

**Administrator:**
- Username: `admin`
- Password: `admin123`

Other roles (Receptionist, Doctor, Patient) can be created through the Admin panel.

## Project Structure

```
HospitalQueueingSystem/
├── backend/
│   ├── config/
│   │   ├── config.php          # Configuration settings
│   │   └── Database.php        # Database connection
│   ├── includes/
│   │   ├── Authentication.php  # Auth & login logic
│   │   ├── Middleware.php      # Permission & middleware
│   │   └── Utils.php           # Security & utilities
│   └── api/
│       ├── auth.php            # Authentication endpoint
│       ├── admin.php           # Admin operations
│       ├── receptionist.php    # Receptionist operations
│       ├── doctor.php          # Doctor operations
│       └── patient.php         # Patient operations
│
├── frontend/
│   ├── admin/
│   │   └── dashboard.html      # Admin dashboard
│   ├── receptionist/
│   │   └── dashboard.html      # Receptionist dashboard
│   ├── doctor/
│   │   └── dashboard.html      # Doctor dashboard
│   ├── patient/
│   │   └── dashboard.html      # Patient dashboard
│   ├── css/
│   │   └── common.css          # Shared styling
│   └── js/
│       ├── common.js           # Shared JS utilities
│       ├── admin.js            # Admin-specific logic
│       ├── receptionist.js     # Receptionist logic
│       ├── doctor.js           # Doctor logic
│       └── patient.js          # Patient logic
│
├── index_new.html              # Login page
├── database.sql                # Database schema
└── README.md                   # This file
```

## API Endpoints

### Authentication
- `POST /backend/api/auth.php?action=login` - User login
- `GET /backend/api/auth.php?action=logout` - User logout
- `POST /backend/api/auth.php?action=register` - Register user (admin only)
- `POST /backend/api/auth.php?action=change-password` - Change password

### Admin
- `GET /backend/api/admin.php?action=stats` - Get system statistics
- `GET /backend/api/admin.php?action=users` - List users
- `POST /backend/api/admin.php?action=create_user` - Create user
- `POST /backend/api/admin.php?action=delete_user&id=X` - Delete user
- `GET /backend/api/admin.php?action=departments` - List departments
- `GET /backend/api/admin.php?action=doctors` - List doctors
- `POST /backend/api/admin.php?action=backup` - Database backup

### Receptionist
- `GET /backend/api/receptionist.php?action=stats` - Dashboard statistics
- `GET /backend/api/receptionist.php?action=departments` - Get departments
- `POST /backend/api/receptionist.php?action=register_patient` - Register patient
- `GET /backend/api/receptionist.php?action=queue` - Get queue list
- `POST /backend/api/receptionist.php?action=call_next` - Call next patient
- `POST /backend/api/receptionist.php?action=skip_patient` - Skip patient
- `POST /backend/api/receptionist.php?action=complete_patient` - Complete consultation

### Doctor
- `GET /backend/api/doctor.php?action=stats` - Dashboard statistics
- `GET /backend/api/doctor.php?action=queue` - Get assigned queue

### Patient
- `GET /backend/api/patient.php?action=data` - Get patient profile
- `GET /backend/api/patient.php?action=appointments` - Get appointments
- `GET /backend/api/patient.php?action=queue` - Get queue status
- `POST /backend/api/patient.php?action=book_appointment` - Book appointment

## Usage Guide

### As Administrator
1. Login with admin credentials
2. Navigate to "Users" to manage staff accounts
3. Create receptionists, doctors with proper departments
4. View "Analytics" for system insights
5. Use "Settings" for database backup

### As Receptionist
1. Login with receptionist credentials
2. "Register Patient" to add new patients
3. Monitor "Queue Management" for real-time queue
4. "Call Next Patient" to serve patients
5. "Reports" for daily performance

### As Doctor
1. Login with doctor credentials
2. View "Today's Queue" for appointments
3. Access patient information when serving

### As Patient
1. Register online in "Patient Registration"
2. Login to access personal portal
3. Book appointments online
4. View queue status in real-time

## Customization

### Change Theme Colors
Edit `:root` variables in `frontend/css/common.css`:
```css
:root {
    --primary: #2563eb;      /* Blue */
    --success: #10b981;      /* Green */
    --warning: #f59e0b;      /* Orange */
    --danger: #ef4444;       /* Red */
    /* ... more variables */
}
```

### Add New Role
1. Add role to `ENUM` in `users` table
2. Create API endpoint in `backend/api/`
3. Create dashboard in `frontend/[role]/`
4. Add permission checks in `Middleware.php`

## Security Considerations

- **Production**: Change `JWT_SECRET` in config
- Use HTTPS in production
- Set proper file permissions (644 for PHP, 755 for directories)
- Regularly backup database
- Keep passwords strong (8+ chars, mix of upper/lower/numbers)
- Review audit logs periodically
- Update PHP and MySQL to latest versions

## Troubleshooting

### Database Connection Error
- Check MySQL is running
- Verify database name, user, password in config
- Ensure database exists

### Login Issues
- Clear browser cookies/cache
- Check user account is active (not suspended)
- Verify password is correct

### Queue Not Updating
- Refresh browser page
- Check receptionist is calling next patient
- Verify JavaScript console for errors

## Performance Tips

- Use indexes on frequently queried columns (done in schema)
- Monitor audit_logs size, archive old logs
- Use database backup for recovery
- Consider pagination for large datasets

## Future Enhancements

- SMS/Email notifications
- Advanced reporting with charts
- Multi-language support
- Mobile app
- Video consultation integration
- Telemedicine features
- Prescription management
- Insurance integration

## Support

For issues or questions:
1. Check browser console for JavaScript errors
2. Review server error logs
3. Verify database setup
4. Check user permissions

## License

CareFlow Hospital System - Premium Healthcare SaaS

---

**Created**: 2026
**Version**: 1.0.0
**Status**: Production Ready
