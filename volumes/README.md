kubectl port-forward svc/mysql-service  30002:3306

kubectl.exe exec -it mysql-deployment-6d4764875b-wpdsj bash

bash-5.2#  mysql -u admin -padmin
mysql>
SELECT User, Host FROM mysql.user;
SHOW DATABASES;
USE DATABASENAME;
SHOW TABLES;

CREATE TABLE example_table (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO example_table (name) 
VALUES ('Sample Data 1'), ('Sample Data 2');

SELECT * FROM example_table;


  ls -al /proc/
 kill pid
