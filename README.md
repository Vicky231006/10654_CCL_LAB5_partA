# Cloud Computing Lab 5 – Assignment 1

## Joomla EC2 Application Connected to Amazon RDS MySQL

### Assignment

Cloud Computing Lab 5 – Assignment 1

---

## 1. Objective

The objective of this assignment is to deploy the same database type used by the Lab 4 EC2 application on AWS and connect the existing application to the deployed database.
The Lab 4 application is a Joomla-based web application running on an Amazon EC2 instance. For this assignment, the Joomla database was migrated from the local MariaDB instance to Amazon RDS MySQL.

The application was then validated by performing all four required CRUD operations:

- Create
- Read
- Update
- Delete

---

## 2. Architecture

The final architecture consists of:

```text
Internet / Client
       |
       | HTTP :80
       v
+----------------------+
|      EC2 Instance    |
|      t3.micro        |
|                      |
|  NGINX :80           |
|      |               |
|  Joomla + PHP        |
+------|---------------+
       |
       | TCP :3306
       | EC2 Security Group only
       v
+----------------------+
|    Amazon RDS MySQL  |
|    db.t3.micro       |
|                      |
|    joomla_db         |
|    jwavf_* tables    |
+----------------------+
```

RDS public access is disabled. Database access is restricted through the EC2 Security Group.

---

## 3. AWS Configuration

### EC2

| Setting | Value |
|---|---|
| Instance type | t3.micro |
| Region | eu-north-1 |
| Application | Joomla |
| Web server | NGINX |
| HTTP port | 80 |

### RDS

| Setting | Value |
|---|---|
| Engine | MySQL |
| Engine version | 8.4.9 |
| Instance class | db.t3.micro |
| Port | 3306 |
| Database | joomla_db |
| Username | joomla_user |
| Endpoint | rds-mysql-tutorial.cdg6mgieuzqx.eu-north-1.rds.amazonaws.com |
| Public access | Disabled |

Passwords and other credentials are intentionally excluded from this repository.

---

## 4. Joomla Database Configuration

The Joomla application uses the following database configuration:

```text
Database type : mysqli
Host          : rds-mysql-tutorial.cdg6mgieuzqx.eu-north-1.rds.amazonaws.com
Port          : 3306
Database      : joomla_db
User          : joomla_user
Password      : [REDACTED]
Table prefix  : jwavf_
Charset       : utf8mb4
Collation     : utf8mb4_unicode_ci
```

The actual password is stored only in the EC2 Joomla configuration and is not included in this repository.

---

## 5. Database Migration

The Joomla database from the Lab 4 EC2 application was migrated into the RDS MySQL instance.

### Migration sequence

1. Created the RDS MySQL instance.
2. Verified connectivity from EC2 to the RDS endpoint on TCP port 3306.
3. Created the `joomla_db` database.
4. Created the `joomla_user` database user.
5. Granted the required privileges on `joomla_db`.
6. Imported the Lab 4 Joomla database backup.
7. Verified the `jwavf_` Joomla table prefix.
8. Verified migrated Joomla users and content.
9. Updated Joomla `configuration.php` to use the RDS endpoint.
10. Updated the database password to the RDS `joomla_user` password.
11. Tested the Joomla application after the migration.

### Connectivity test

The EC2 instance successfully connected to RDS using:

```bash
mariadb -h <RDS-ENDPOINT> -P 3306 -u joomla_user -p
```

RDS returned:

```text
Server version: 8.4.9
```

---

## 6. Security Configuration

The RDS database is not publicly exposed.
The RDS Security Group allows MySQL traffic on TCP port 3306 only from the EC2 Security Group.

```text
EC2 Security Group
        |
     TCP 3306
        v
RDS Security Group
        |
        v
    RDS MySQL
```

No `0.0.0.0/0` rule is used for RDS database access.

The EC2 instance exposes HTTP on TCP port 80 for the Joomla application.

SSH access to EC2 is restricted to the administrator's IP address.

---

## 7. CRUD Demonstration

All CRUD operations were performed through the running Joomla application connected to RDS.

### CREATE

A temporary Joomla article named:

```text
RDS Crud Test
```

was created through the Joomla Administrator interface.

**Result:** PASS

### READ

The article was exposed through a temporary public Joomla menu item and successfully viewed through the public EC2 application.

**Result:** PASS

### UPDATE

The article content was modified to:

```text
Lab 5 RDS CRUD UPDATE test
```

The updated content was then verified through the public Joomla application.

**Result:** PASS

### DELETE

The test article was trashed and permanently deleted.

The RDS database was then queried:

```sql
SELECT id,title,state
FROM jwavf_content
WHERE title='RDS Crud Test';
```

The query returned no matching record.

**Result:** PASS

---

## 8. Application Validation

The application was tested locally from the EC2 instance:

```bash
curl -I http://localhost
```

The application returned:

```text
HTTP/1.1 200 OK
```

The Joomla page title was also verified:

```bash
curl -s http://localhost | grep -i "<title" | head
```

Result:

```text
<title>Home</title>
```

The application was also successfully accessed through the EC2 public HTTP endpoint.

---

## 9. Repository Security

Sensitive credentials are intentionally excluded from this repository.

The following files/data are excluded:

```text
configuration.php
configuration.php.local-backup
.env
*.pem
*.key
backup/
cache/
administrator/cache/
logs/
tmp/
```

Database passwords, AWS credentials and private keys must never be committed to the repository.

---

## 10. Public Application

EC2 application URL:

```text
http://16.171.234.164
```

---

## 11. Evidence

The accompanying report contains screenshots demonstrating:

1. EC2 application running.
2. RDS MySQL instance configuration.
3. RDS Security Group configuration.
4. CREATE operation.
5. READ operation.
6. UPDATE operation.
7. DELETE operation.
8. RDS-side validation of the deleted record.

---

## 12. Conclusion

The Lab 4 Joomla application was successfully migrated from its local MariaDB database to Amazon RDS MySQL.

The EC2 application successfully connects to the RDS database through TCP port 3306, while database access is restricted to the EC2 Security Group.

The migrated Joomla data and users were preserved, and all four required CRUD operations were successfully demonstrated through the running EC2 application.

This completes the database deployment and connection requirements for Lab 5 Assignment 1.
