How to Install Velocity Proxy on a Minecraft Server

Velocity is a high-performance proxy for Minecraft servers that allows you to manage multiple Minecraft servers as part of a single network. It acts as a proxy between the client and the Minecraft servers, providing features like player forwarding, cross-server communication, and more. Setting up Velocity is a great way to scale your Minecraft server network and improve its performance.

In this guide, we'll walk you through the steps to install and configure Velocity on your Minecraft server network.


---

Prerequisites

Before you begin, ensure you have the following:

A Minecraft server (Velocity acts as a proxy to manage multiple servers).

Java (Velocity requires Java 8 or later).

A running server (Ubuntu Server or similar preferred for ease of setup).

Sufficient system resources (RAM and CPU depending on the size of your Minecraft network).

Access to your servers via SSH (or physical access).



---

Step 1: Update Your Server

Before starting, it’s always a good idea to update your server's software to the latest versions.

sudo apt update
sudo apt upgrade -y


---

Step 2: Install Java (if not installed)

Velocity requires Java 8 or later. To install Java 17 (recommended version), run the following commands:

sudo apt install openjdk-17-jdk -y

To verify the installation, you can check the Java version:

java -version

You should see something like:

openjdk version "17.0.x" 202x-xx-xx


---

Step 3: Download Velocity

Now, let's download the latest stable release of Velocity. Visit the Velocity download page for the latest version, or run the following command to download it directly from the official GitHub repository:

wget https://github.com/VelocityPowered/Velocity/releases/download/1.1.0/velocity-1.1.0.jar -O velocity.jar

Make sure to replace the version (1.1.0) with the latest version available if necessary.


---

Step 4: Create a Directory for Velocity

Create a new directory to store Velocity and its configurations:

mkdir /home/minecraft/velocity
cd /home/minecraft/velocity

Move the downloaded velocity.jar into the new directory:

mv /path/to/velocity.jar /home/minecraft/velocity/


---

Step 5: Create a Configuration File for Velocity

Once you've placed the velocity.jar file in your server directory, you’ll need to run the server once to generate configuration files.

Run the following command to start the proxy server:

java -jar velocity.jar

This will create a config folder with the necessary configuration files.

After running the command, you should see the following directory structure:

/home/minecraft/velocity/
│
├── config/
│   ├── config.toml
│   ├── permissions.toml
│   └── velocity.toml
├── velocity.jar
└── logs/


---

Step 6: Configure the Proxy (velocity.toml)

Now, you can edit the configuration files to customize your Velocity server. Open the config.toml file:

nano /home/minecraft/velocity/config/config.toml

Key settings to configure:

bind_address: Defines the IP address the proxy listens to. By default, it binds to all available interfaces (0.0.0.0). You can change this to a specific IP if needed.

bind_address = "0.0.0.0"

host: This specifies the IP address and port that the proxy should connect to. The default is localhost:25577, which you can change based on your network setup.

host = "0.0.0.0:25577"

query_address: If you want to expose your server to the query system for listing in server lists, use the query address option.

query_address = "0.0.0.0:25577"

motd: The Message of the Day (MOTD) for the proxy. You can change this to any custom message you like.

motd = "Welcome to my Minecraft Network"


Save and exit by pressing Ctrl + X, then Y, and finally Enter.


---

Step 7: Set Up Servers to Connect to Velocity

Velocity doesn’t host Minecraft servers itself, but it proxies players to Minecraft backend servers. You will need to set up a few Minecraft servers for Velocity to forward players to.

1. Set up a backend Minecraft server (e.g., Spigot, Paper, etc.) on a separate machine or on the same machine. Ensure that the Minecraft server is set to use online mode and a unique port, e.g., 25565.


2. Modify the config.toml to include your backend servers under the servers section.



For example, if you have a backend server on port 25565:

[[servers]]
name = "survival"
address = "127.0.0.1:25565"
restricted = false
motd = "Survival Minecraft"

3. You can add more servers as needed (e.g., for creative, skyblock, etc.) by adding additional server entries under the servers section.




---

Step 8: Configure the Minecraft Server (Backend)

In each backend Minecraft server (Spigot, Paper, etc.), you’ll need to disable online mode so Velocity can handle authentication.

1. Open the server.properties file of each Minecraft server.


2. Set online-mode=false:



online-mode=false

This will allow Velocity to handle the authentication and proxy connections.


---

Step 9: Start Velocity Proxy

Now, you're ready to start the Velocity proxy. Run the following command in your Velocity directory:

java -jar velocity.jar

Velocity will now start as a proxy and forward players to your Minecraft servers.


---

Step 10: Set Up a Systemd Service (Optional)

To keep Velocity running in the background and start automatically on boot, set it up as a systemd service.

1. Create a service file:



sudo nano /etc/systemd/system/velocity.service

2. Add the following configuration:



[Unit]
Description=Velocity Minecraft Proxy
After=network.target

[Service]
User=minecraft
WorkingDirectory=/home/minecraft/velocity
ExecStart=/usr/bin/java -jar /home/minecraft/velocity/velocity.jar
Restart=on-failure

[Install]
WantedBy=multi-user.target

3. Reload systemd to apply the changes:



sudo systemctl daemon-reload

4. Enable and start the service:



sudo systemctl enable velocity
sudo systemctl start velocity

Now, Velocity will automatically start when the server boots.


---

Step 11: Connect to the Proxy

1. In your Minecraft client, add a new server and enter the IP address of your Velocity proxy. The default port is 25577.


2. Join the network by connecting to your Velocity proxy. From there, you can navigate between different Minecraft servers in your network, like "Survival", "Creative", etc.




---

Conclusion

You've successfully installed and configured Velocity Proxy on your Minecraft network! Velocity provides a high-performance proxy that helps you manage multiple Minecraft servers and offers various features like player forwarding and server switching. With this setup, you can easily scale your Minecraft network and enhance the multiplayer experience for your players.

For more advanced features, such as permissions, plugins, and more, check out the Velocity documentation.
