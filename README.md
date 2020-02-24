# nodejs_db_hackernoon

This study is a product of of this [tutorial](https://hackernoon.com/setting-up-node-js-with-a-database-part-1-3f2461bdd77f) by *hackernoon* for using `MySQL` from inside a `node.js` app, with small changes:
  - `MariaDB` implementation for `MySQL` is used; and
  - `LibreOffice` is used to view the database.

## MariaDB setup (Arch Linux)

Installing and configuring `mariadb` is easy. I've followed [Arch Wiki's article on MariaDB](https://wiki.archlinux.org/index.php/MariaDB) in this part.


### Instalation and Starting
```sh
# mariadb is the default Arch Linux implementation
sudo pacman -S mysql
sudo mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql

# for starting mariadb service for this session only
sudo systemctl start mariadb.service

# for enabling mariadb service to start at every session (optional)
sudo systemctl enable mariadb.service
```

### Configuration

Enter `MariaDB` prompt:
```sh
sudo mysql -u root -p
```

For creating a database, I've followed [this DigitalOcean tutorial](https://www.digitalocean.com/community/tutorials/how-to-create-and-manage-databases-in-mysql-and-mariadb-on-a-cloud-server):

```mysql
# create a new database
# CREATE DATABASE IF NOT EXISTS new_database;
CREATE DATABASE IF NOT EXISTS nodehackernoon;

# see databases with
SHOW DATABASES;

# find which database is currently selected
SELECT database();

# select database
#USE my_database;
USE nodehackernoon;

#####################################
# If needed, delete a database with #
# DROP DATABASE a_database;         #
#####################################
```

Then I came back to Arch Wiki.

Create user "marcosrdac" with password "" for accessing MySQL; then give it pemissions to access and modify "nodehackernoon" database.
```mysql
# CREATE USER 'my_user'@'localhost' IDENTIFIED BY 'my_password';
CREATE USER 'marcosrdac'@'localhost' IDENTIFIED BY '';
# GRANT ALL PRIVILEGES ON my_database.* TO 'my_user'@'localhost';
GRANT ALL PRIVILEGES ON nodehackernoon.* TO 'marcosrdac'@'localhost';
FLUSH PRIVILEGES;
quit
```

this configuration must be in concordance with `/knexfile.js`.


## LibreOffice Base

Set this options when creating a new database at LibreOffice Base (you are actually just going to connect to MySQL and view its data):

  - Connect to an existing database:
    - MySQL;
  - Connect directly:
    - Database name: "nodehackernoon";
    - Server: "localhost";
    - Port: "3306" (its the default;
  - User Setup:
    - User: "marcosrdac";
    - Password: "";

For convenience, I've saved it at the project folder. Now you can already see your database.


## Node.js and its modules (Arch Linux)

Install node with
```sh
sudo pacman -S nodejs
```

Enter the project folder and:
```sh
#npm init
npm install knex mysql express body-parser --save
# also install knex globally
sudo npm install knex -g
```

Knex database migration (changing database structure):
```sh
knex migrate:latest
```


## Running app

```sh
node .
```

Then enter [](http://localhost:7555) from your browser. Create some users. See them at LibreOffice, at the database's users table.

That's all.
