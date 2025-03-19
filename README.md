# AWS-RDS-and-MySQL-on-EC2-A-Step-by-Step-Guide
# AWS RDS and MySQL on EC2: A Step-by-Step Guide

## **RDS Networking Concepts**
1. **Background Network of RDS:**
   - RDS requires a minimum of **two subnets**.
   - Each subnet must be in a different **Availability Zone (AZ)** to ensure **high availability and disaster recovery**.
   - Subnets must always be **private**, ensuring the database remains private.
   - In a **Multi-AZ setup**, the database is created in one subnet, while the other subnet serves as a standby.

2. **Understanding Public Access in RDS:**
   - **Public Access Enabled:** The database receives a **public IP address**.
   - **Scenarios:**
     - **Condition 1:** DB in a **private subnet** with **public access enabled** → **Cannot be accessed externally** (traffic must go through the internet).
     - **Condition 2:** DB in a **public subnet** with **public access enabled** → **Can be accessed externally**.
     - **Condition 3:** DB in a **public subnet** with **public access disabled** → **Cannot be accessed externally**.

3. **Reasons for Inaccessibility:**
   - DB is in a **private subnet**.
   - DB is in a **public subnet** but **public access is disabled**.
   - **Security Group (SG) rules do not allow traffic on port 3306**.

4. **Subnet Groups in RDS:**
   - Used to **define subnets** for RDS creation.
   - Requires at least **two subnets in two different AZs**.
   - Private subnets are recommended for security.

5. **Connecting to RDS from a Server:**
   ```sh
   mysql -h <endpoint> -u <username> -p
   ```
   - Ensure **MySQL client** is installed before connecting.
   - If connecting from another **VPC**, use **VPC Peering** for internal communication.

## **Installing MySQL on EC2 (Amazon Linux 2023)**

1. **Update the System:**
   ```sh
   sudo dnf update -y
   ```
2. **Download and Install MySQL Repository:**
   ```sh
   sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-4.noarch.rpm
   sudo dnf install mysql80-community-release-el9-4.noarch.rpm
   ```
3. **Install MySQL Server:**
   ```sh
   sudo dnf install mysql-community-server -y
   ```
4. **Start MySQL Service:**
   ```sh
   sudo systemctl start mysqld
   ```
5. **Retrieve Temporary Root Password:**
   ```sh
   sudo grep 'temporary password' /var/log/mysqld.log
   ```
6. **Login to MySQL:**
   ```sh
   mysql -u root -p
   ```
7. **Change Root Password:**
   ```sh
   ALTER USER 'root'@'localhost' IDENTIFIED BY 'NewPassword!123';
   FLUSH PRIVILEGES;
   ```
8. **Create and Use a Database:**
   ```sh
   CREATE DATABASE test;
   USE test;
   ```
9. **Create a Table:**
   ```sh
   CREATE TABLE users (
       id INT AUTO_INCREMENT PRIMARY KEY,
       name VARCHAR(100),
       email VARCHAR(100)
   );
   ```

## **Key Differences: MySQL Client vs. MySQL Server**
| Feature          | MySQL Client                        | MySQL Server                         |
|-----------------|-----------------------------------|------------------------------------|
| Purpose        | Connects to a database            | Stores and manages databases       |
| Installation   | Lightweight                        | Includes full database functionality |
| Usage         | Query execution and data retrieval | Data storage, query processing     |

## **Challenges of Running MySQL on EC2 (Interview Question)**
1. **Scalability:** Manual scaling required.
2. **Security:** Requires manual configuration of firewalls and security groups.
3. **Maintenance:** Backups and failovers must be handled manually.
4. **Availability:** No built-in high availability like RDS Multi-AZ.
5. **Networking:** Needs careful setup of VPC, subnets, and security groups.

This guide provides an easy-to-follow process for working with RDS and MySQL on EC2, covering installation, configuration, and networking aspects.

