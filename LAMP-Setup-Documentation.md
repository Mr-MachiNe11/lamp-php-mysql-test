Title: LAMP Stack Setup and PHP-MySQL Integration on Ubuntu 20.04

✅ Objective:
To set up a LAMP stack (Linux, Apache, MySQL, PHP) on Ubuntu 20.04, verify PHP processing, and test MySQL connectivity using a PHP script.
🛠️ Step-by-Step Documentation Plan (Markdown Format)
1. System Details
OS: Ubuntu 20.04
MySQL Version: 8.0.42



2. LAMP Stack Installation Summary


sudo apt update
sudo apt install apache2
sudo apt install mysql-server
sudo apt install php libapache2-mod-php php-mysql

3. Configure Apache Virtual Host (Optional Custom Domain)

sudo mkdir -p /var/www/your_domain
sudo chown -R $USER:$USER /var/www/your_domain
sudo nano /etc/apache2/sites-available/your_domain.conf
Sample config:
<VirtualHost *:80>
    ServerName rinku-test.local
    ServerAlias www.rinku-test.local
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/rinku-test

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

Then:
sudo a2ensite rinku.test.conf
sudo a2dissite 000-default.conf
sudo systemctl reload apache2


4. PHP Info Test

nano /var/www/your_domain/info.php
Contents:
<?php
phpinfo(); 
?>
Access:
http://your_domain/info.php
Remove it after testing:
sudo rm /var/www/your_domain/info.php

5. MySQL Database and User Creation

CREATE DATABASE test_db;
CREATE USER 'rinku'@'%' IDENTIFIED BY '1qaz';
GRANT ALL ON test_db.* TO 'rinku'@'%';
Test login:
mysql -u rinku -p



6. Create and Populate Table


CREATE TABLE test_db.todo_list (
  item_id INT AUTO_INCREMENT,
  content VARCHAR(255),
  PRIMARY KEY(item_id)
);

Insert sample data:

INSERT INTO test_db.todo_list (content) VALUES ("Buy groceries");
INSERT INTO test_db.todo_list (content) VALUES ("Practice more");
INSERT INTO test_db.todo_list (content) VALUES ("Fix bugs");
 


7. PHP Script to Read MySQL Data

nano /var/www/your_domain/todo_list.php
Contents:
<?php
$user = "rinku";
$password = "1qaz";
$database = "test_db";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
?>

Access:
http://rinku.test/todo_list.php
