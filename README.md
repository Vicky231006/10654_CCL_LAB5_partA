# Cloud Computing Lab 5 – Assignment 1

## Objective
Connect the Lab 4 EC2 Joomla application to Amazon RDS MySQL
and demonstrate all CRUD operations.

## AWS Configuration

- EC2: t3.micro
- Region: eu-north-1
- Database: Amazon RDS MySQL
- RDS Instance: db.t3.micro
- Port: 3306
- Database: joomla_db

## Connection

The Joomla application was configured to connect to the RDS
endpoint through configuration.php.

Database credentials are not included in this repository.

## Security

RDS inbound traffic on port 3306 is allowed only from the
EC2 Security Group.

Public access to RDS is disabled.

## CRUD Operations

- Create – Created "RDS Crud Test"
- Read – Viewed the article through the Joomla application
- Update – Updated the article content
- Delete – Deleted the test article

## Deployment

1. Launch EC2 instance.
2. Install and configure Joomla.
3. Create RDS MySQL instance.
4. Restore the Joomla database into RDS.
5. Update Joomla configuration.php with the RDS endpoint.
6. Test CRUD operations through the EC2 application.

## Application URL

http://16.171.234.164/index.php
