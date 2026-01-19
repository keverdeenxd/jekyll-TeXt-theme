How to Install Pterodactyl Panel on a Minecraft Server

Pterodactyl is an open-source game server management panel that allows you to easily manage game servers like Minecraft. It provides a web-based interface where you can control your Minecraft servers, monitor their performance, and manage users—all in a simple and efficient manner.

In this guide, we'll walk you through how to install Pterodactyl Panel on your server and configure it to manage a Minecraft server.


---

Prerequisites

Before you begin, make sure you have the following:

A Ubuntu 20.04 or 22.04 server (Pterodactyl runs on Linux).

A Minecraft server running on a different machine or on the same machine (this guide will focus on setting up Pterodactyl for Minecraft).

At least 2GB of RAM (depending on your usage and the number of servers you intend to run).

Root access or sudo privileges on the server.



---

Step 1: Prepare Your Server

1. Update your server to ensure all packages are up to date:



sudo apt update && sudo apt upgrade -y

2. Install required packages such as curl, git, nginx, mysql, php, and other dependencies:



sudo apt install -y curl git nginx zip unzip sudo lsb-release ca-certificates apt-transport-https

3. Set up a swap file if your server has less than 2GB of RAM. This will help Pterodactyl run more smoothly.



sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

4. Verify that the swap file is active:



sudo swapon --show


---

Step 2: Install Docker

Pterodactyl uses Docker to run game servers in isolated environments. You’ll need to install Docker first.

1. Install Docker:



sudo apt install -y docker.io

2. Start and enable Docker to run on boot:



sudo systemctl start docker
sudo systemctl enable docker

3. Add your user to the docker group to run Docker commands without sudo:



sudo usermod -aG docker $USER

4. Verify the Docker installation:



docker --version


---

Step 3: Install Pterodactyl Panel

1. Install Node.js (required for the Pterodactyl Panel):



curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt install -y nodejs

2. Install PHP and required PHP extensions:



sudo apt install -y php-fpm php-cli php-mbstring php-xml php-mysql php-curl php-zip php-bcmath php-json

3. Install Composer (a PHP dependency manager):



curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer

4. Install and configure the database (MySQL):



sudo apt install -y mysql-server
sudo systemctl start mysql
sudo systemctl enable mysql

5. Secure MySQL by running:



sudo mysql_secure_installation

6. Create the Pterodactyl database and user:



sudo mysql -u root -p

CREATE DATABASE pterodactyl;
CREATE USER 'pterodactyl'@'localhost' IDENTIFIED BY 'strongpassword';
GRANT ALL PRIVILEGES ON pterodactyl.* TO 'pterodactyl'@'localhost';
FLUSH PRIVILEGES;
EXIT;

7. Clone the Pterodactyl Panel repository:



cd /var/www
sudo git clone https://github.com/pterodactyl/panel.git pterodactyl

8. Set the correct permissions for the Pterodactyl directory:



sudo chown -R www-data:www-data /var/www/pterodactyl
sudo chmod -R 755 /var/www/pterodactyl

9. Install the required PHP dependencies using Composer:



cd /var/www/pterodactyl
sudo composer install --no-dev --optimize-autoloader

10. Copy the example .env file:



sudo cp .env.example .env

11. Generate the application key:



sudo php artisan key:generate --force

12. Configure the environment file. Open .env for editing:



sudo nano .env

Set your database connection to MySQL, and ensure the DB_* variables match your MySQL configuration:


DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=pterodactyl
DB_USERNAME=pterodactyl
DB_PASSWORD=strongpassword

13. Run the database migrations to set up the Pterodactyl schema:



sudo php artisan migrate --seed --force


---

Step 4: Set Up Nginx

1. Configure Nginx to serve the Pterodactyl Panel. Create a new Nginx configuration file:



sudo nano /etc/nginx/sites-available/pterodactyl

2. Add the following configuration:



server {
    listen 80;
    server_name your-domain.com;
    root /var/www/pterodactyl/public;

    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ ^/index.php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }
}

3. Enable the new site by creating a symbolic link to the sites-enabled directory:



sudo ln -s /etc/nginx/sites-available/pterodactyl /etc/nginx/sites-enabled/

4. Check Nginx configuration for errors:



sudo nginx -t

5. Restart Nginx to apply the changes:



sudo systemctl restart nginx


---

Step 5: Install the Pterodactyl Daemon

Pterodactyl uses a Daemon to manage the game servers. Let’s install it:

1. Download the Daemon installer:



curl -Lo /tmp/daemon.tar.gz https://github.com/pterodactyl/daemon/releases/download/v1.0.0/pterodactyl-daemon.tar.gz

2. Extract and install:



tar -zxvf /tmp/daemon.tar.gz -C /var/lib
cd /var/lib/pterodactyl
sudo chmod +x pterodactyl

3. Configure the Daemon by editing the .env file:



sudo nano /var/lib/pterodactyl/.env

Change the necessary settings such as PANEL_URL, PANEL_API_KEY, and database configuration.

4. Start the Daemon:



sudo systemctl start pterodactyl-daemon


---

Step 6: Access the Panel

1. Open your browser and go to the Pterodactyl Panel URL (http://your-domain.com).


2. Log in using the admin credentials created during installation. The default login information is typically created in the installation process.




---

Step 7: Create a Minecraft Server on Pterodactyl

1. In the Pterodactyl Panel, navigate to Admin → Nodes and set up a node. The node represents the physical server that will host your Minecraft server.


2. Then, under Admin → Locations, create a location for your servers.


3. Under Admin → Servers, create a new Minecraft server. Select the server type (e.g., Minecraft, Spigot, Paper) and configure the details.


4. Start the server, and you're good to go!




---

Conclusion

Congratulations! You've successfully installed Pterodactyl Panel on your server and configured it to manage your Minecraft server. Now you can easily manage your game server through a web interface, monitor server performance, and even scale your setup with more game servers as needed.

For more detailed information and support, visit the Pterodactyl documentation.
