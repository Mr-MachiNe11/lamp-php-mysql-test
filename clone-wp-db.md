# 📦 Clone WordPress Database & Connect to New One

This guide walks through how to clone an existing WordPress MySQL database (`wp_db`) into a new one (`wp_clone`), update WordPress to use the new database, and connect both databases in DBeaver.

---

## 🔁 Step-by-Step

### 1. Export the Existing Database

mysqldump -u root -p wp_db > wp_db_dump.sql

This creates a dump of the wp_db database in a file named wp_db_dump.sql.

2. Create a New Database and User
Login to MySQL:

mysql -u root -p
Then inside the MySQL prompt:

CREATE DATABASE wp_clone;
CREATE USER 'wp_user'@'localhost' IDENTIFIED BY '1qaz';
GRANT ALL PRIVILEGES ON wp_clone.* TO 'wp_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;


3. Import the Dump into the New Database

mysql -u root -p wp_clone < wp_db_dump.sql


4. Update WordPress to Use the New Database
Edit the wp-config.php file in /var/www/html/wordpress:

/** The name of the database for WordPress */
define( 'DB_NAME', 'wp_clone' );

/** MySQL database username */
define( 'DB_USER', 'wp_user' );

/** MySQL database password */
define( 'DB_PASSWORD', '1qaz' );


5. Restart Apache (Optional but Recommended)

$ sudo systemctl restart apache2


6. Test in Browser
Visit:

http://wordpress.rinku.test

✅ If the site loads without error, your cloned database is working!


🐬 Optional: View Databases in DBeaver
🛠️ Setup MySQL Connection

Open DBeaver.

Click Database → New Database Connection.

Select MySQL, then click Next.

Use the following credentials:

Host: 127.0.0.1
Port: 3306
Database: wp_clone
Username: wp_user
Password: ****

Click Test Connection, then Finish.

🔧 Ensure MySQL Is Listening on 127.0.0.1

Edit /etc/mysql/mysql.conf.d/mysqld.cnf:

bind-address = 127.0.0.1
Then restart MySQL:

$ sudo systemctl restart mysql


✅ Result
✅ wp_clone contains a full copy of wp_db

✅ wordpress.rinku.test is now using the cloned DB

✅ Both databases are visible in DBeaver

