---
title: Wordpress - 2. Build your own lab with Docker
image:
    path: /assets/posts/2026-14-01-build-your-own-wordpress/1.png
date: 2026-01-17 06:00:00 +/-TTTT
categories: [Wordpress]
tags: [Wordpress, Tutorial]
---

Docker revolutionizes how penetration testers create practice environments. Instead of spending hours configuring a traditional LAMP stack on a virtual machine, you can deploy a fully functional, intentionally vulnerable WordPress instance in minutes. This lab will be accessible from your Kali Linux VM, allowing you to practice real-world WordPress pentesting techniques including enumeration with WPScan, exploitation of vulnerable plugins, SQL injection, and post-exploitation in a safe, controlled, and reproducible environment.

## Introduction

If you've ever set up a [WordPress lab](https://p314do.github.io/secai/posts/build-your-own-wordpress/) the traditional way—installing Ubuntu Server, configuring Apache, MySQL, PHP, downloading WordPress, setting permissions—you know it can take 30-60 minutes and consume significant resources.

Docker changes everything!.

![Image](https://substackcdn.com/image/fetch/$s_!SfCa!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F55ac4874-04fc-4ea7-a083-bde8f6f99cf5_2360x2960.png)
> Image from [https://blog.bytebytego.com/](https://blog.bytebytego.com/)

With Docker, you define your entire environment in simple text files:  
**Need to reset your lab after breaking something?** One command.   
**Want to create 5 different WordPress versions to practice different exploits?** Five minutes.  
**Need to share your exact lab configuration with your team?** Send them a file.

For ethical hackers and penetration testers, Docker offers:  
✓ `Speed`: From zero to fully functional lab in under 5 minutes  
✓ `Reproducibility`: Identical environment every single time  
✓ `Portability`: Run the same lab on any machine with Docker  
✓ `Isolation`: Experiment freely without breaking your host system  
✓ `Resource efficiency`: Multiple labs running simultaneously on modest hardware  
✓ `Version control`: Keep different vulnerable configurations for different practice scenarios  

Today, we're building a WordPress penetration testing lab on Ubuntu Desktop using Docker, and we'll configure it to be accessible from a separate Kali Linux VM so you can practice real-world pentesting workflows.
Let's dive in.

## PART 1 : INSTALLING DOCKER ON UBUNTU DESKTOP

### **STEP 1: Update the System.**   
Open a terminal by pressing Ctrl + Alt + T and run:
```
sudo apt update && sudo apt upgrade -y
```
This ensures your system packages are up to date before we install Docker.

### **STEP 2: Install Docker Prerequisites.** 
```
sudo apt install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common \
    gnupg \
    lsb-release
```

These packages allow apt to fetch packages over HTTPS and verify their authenticity.

### **STEP 3: Add Docker's Official GPG Key.**
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
This cryptographic key verifies that the Docker packages you download are authentic and haven't been tampered with.

### **STEP 4: Add Docker Repository**  
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
This adds Docker's official repository to your system so you can install and receive updates directly from Docker.  

### **STEP 5: Install Docker Engine**

```
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
This installs:  
- `docker-ce`: Docker Community Edition engine
- `docker-ce-cli`: Command-line interface
- `containerd.io`: Container runtime
- `docker-buildx-plugin`: Advanced build features
- `docker-compose-plugin`: Multi-container orchestration (critical for our lab)

### **STEP 6: Configure Docker Permissions**  
By default, Docker requires sudo for every command. Let's fix that:
```
sudo usermod -aG docker $USER
```
This adds your user to the docker group, granting permission to run Docker commands.
Apply the change immediately:
```
newgrp docker
```
Or log out and log back in for the change to take effect.

### **STEP 7: Verify Docker Installation**

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/57.png)

Test Docker with a hello-world container:
```
docker run hello-world
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/58.png)

If you see "Hello from Docker!" with additional information, Docker is installed correctly and working.

### **STEP 8: Enable Docker to Start on Boot**

```
sudo systemctl enable docker
sudo systemctl start docker
```

This ensures Docker starts automatically whenever you boot Ubuntu.
Verify Docker service status:
```
sudo systemctl status docker
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/59.png)

---

## PART 2: UNDERSTANDING THE LAB ARCHITECTURE

Before we start building, let's understand what we're creating:  

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/diagram.svg)

Components:
- WordPress Container: Our target application (vulnerable WordPress 5.8.0)
- MySQL Container: Database backend (MySQL 5.7 with known vulnerabilities)
- PHPMyAdmin Container: Web interface for database management (useful for SQL injection practice)
- Docker Network: Private network connecting all containers
- Bridge Network: Allows Kali VM to access the lab

---

## PART 3: BUILDING THE WORDPRESS LAB

**STEP 1: Create Project Structure**

Create a dedicated directory for your lab:  
```
mkdir -p ~/labs/wordpress-pentest-lab
cd ~/labs/wordpress-pentest-lab
```

**STEP 2: Create the Dockerfile**

The Dockerfile defines our custom WordPress image with intentional security weaknesses.
```
nano Dockerfile
```
Or use your preferred editor:
```
gedit Dockerfile &
```
Or if you have VS Code installed:
```
code Dockerfile
```

Copy this content into the Dockerfile:
```Dockerfile
# Use WordPress 5.8.0 with PHP 7.4 (both have known vulnerabilities)
FROM wordpress:5.8.0-php7.4-apache

# Metadata
LABEL maintainer="pentester@lab.local"
LABEL description="Intentionally vulnerable WordPress for ethical hacking practice"
LABEL version="1.0"

# Install useful debugging and exploitation tools
RUN apt-get update && apt-get install -y \
    vim \
    nano \
    wget \
    curl \
    unzip \
    net-tools \
    iputils-ping \
    dnsutils \
    default-mysql-client \
    && rm -rf /var/lib/apt/lists/*

# Configure PHP for maximum verbosity (reveals information useful for attackers)
RUN echo "display_errors = On" >> /usr/local/etc/php/conf.d/custom.ini && \
    echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/custom.ini && \
    echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/custom.ini && \
    echo "log_errors = On" >> /usr/local/etc/php/conf.d/custom.ini && \
    echo "error_log = /var/log/php_errors.log" >> /usr/local/etc/php/conf.d/custom.ini

# Disable PHP security functions (DANGEROUS - Lab only!)
RUN echo "disable_functions = " >> /usr/local/etc/php/conf.d/custom.ini

# Allow dangerous PHP settings
RUN echo "allow_url_fopen = On" >> /usr/local/etc/php/conf.d/custom.ini && \
    echo "allow_url_include = On" >> /usr/local/etc/php/conf.d/custom.ini

# Set permissive file permissions (vulnerable by design)
RUN chown -R www-data:www-data /var/www/html && \
    chmod -R 755 /var/www/html

# Install WP-CLI for automation and easier plugin installation
RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && \
    chmod +x wp-cli.phar && \
    mv wp-cli.phar /usr/local/bin/wp && \
    chown www-data:www-data /usr/local/bin/wp

# Create uploads directory with permissive permissions
RUN mkdir -p /var/www/html/wp-content/uploads && \
    chmod -R 777 /var/www/html/wp-content/uploads

# Expose HTTP port
EXPOSE 80

# Default command (inherited from base image)
CMD ["apache2-foreground"]
```

Security Weaknesses Intentionally Introduced:
- display_errors = On - Reveals sensitive error information
- disable_functions = "" - Allows execution of dangerous PHP functions
- allow_url_include = On - Enables remote file inclusion attacks
- chmod 777 - Overly permissive file permissions
- Old WordPress version (5.8.0) - Contains known CVEs
- Old PHP version (7.4) - Contains vulnerabilities

Save the file `(Ctrl + O, Enter, Ctrl + X in nano)`.

**STEP 3: Create docker-compose.yml**
Docker Compose orchestrates multiple containers working together.
```
nano docker-compose.yml
```
Copy this content:  
```Dockerfile
services:
  # MySQL Database
  db:
    image: mysql:5.7
    container_name: wordpress-db
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: ${DB_ROOT_PASSWORD}
      MYSQL_DATABASE: ${DB_NAME}
      MYSQL_USER: ${DB_USER}
      MYSQL_PASSWORD: ${DB_PASSWORD}
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - wordpress-network
    ports:
      - "3306:3306"
    command: 
      - '--default-authentication-plugin=mysql_native_password'
      - '--sql_mode='
      - '--general-log=1'
      - '--general-log-file=/var/lib/mysql/general.log'

  # WordPress Application
  wordpress:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: wordpress-app
    restart: unless-stopped
    depends_on:
      - db
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_NAME: ${DB_NAME}
      WORDPRESS_DB_USER: ${DB_USER}
      WORDPRESS_DB_PASSWORD: ${DB_PASSWORD}
      WORDPRESS_DEBUG: 'true'
      WORDPRESS_CONFIG_EXTRA: |
        define('WP_DEBUG', true);
        define('WP_DEBUG_LOG', true);
        define('WP_DEBUG_DISPLAY', true);
        define('SCRIPT_DEBUG', true);
        define('SAVEQUERIES', true);
        define('CONCATENATE_SCRIPTS', false);
        /* Disable automatic updates */
        define('AUTOMATIC_UPDATER_DISABLED', true);
        define('WP_AUTO_UPDATE_CORE', false);
        define('DISALLOW_FILE_MODS', false);
        /* Allow direct filesystem access */
        define('FS_METHOD', 'direct');
        /* Enable XML-RPC (often exploited) */
        ## add_filter('xmlrpc_enabled', '__return_true');
    volumes:
      - wordpress_data:/var/www/html
      - ./uploads.ini:/usr/local/etc/php/conf.d/uploads.ini
      - ./logs:/var/log/apache2
    ports:
      - "0.0.0.0:8080:80"
    networks:
      - wordpress-network

  # PHPMyAdmin for database management
  phpmyadmin:
    image: phpmyadmin/phpmyadmin:latest
    container_name: wordpress-phpmyadmin
    restart: unless-stopped
    depends_on:
      - db
    environment:
      PMA_HOST: db
      PMA_PORT: 3306
      MYSQL_ROOT_PASSWORD: ${DB_ROOT_PASSWORD}
      PMA_ARBITRARY: 1
    ports:
      - "8081:80"
    networks:
      - wordpress-network

# Persistent volumes
volumes:
  db_data:
    driver: local
  wordpress_data:
    driver: local

# Network configuration
networks:
  wordpress-network:
    driver: bridge
```

Key Configuration Points:
Port Bindings:
- "0.0.0.0:8080:80" - WordPress accessible from ANY network interface (including from Kali VM)
- "3306:3306" - MySQL exposed (for SQLMap and other tools)
- "8081:80" - PHPMyAdmin interface

Vulnerable MySQL Settings:
- ---sql_mode='' - Disables strict SQL mode (allows unsafe queries)  
- ---general-log=1 - Logs all queries (useful for learning but dangerous in production)

WordPress Debug Mode:
- All debug flags enabled - exposes detailed error information
- DISALLOW_FILE_MODS: false - Allows plugin/theme installation
- XML-RPC enabled - common attack vector

Save the file.

**STEP 4: Create Environment Variables File**

```
nano .env
```

Content:
```bash
# MySQL Root Password
DB_ROOT_PASSWORD=toor123

# WordPress Database Configuration
DB_NAME=wordpress_db
DB_USER=wp_user
DB_PASSWORD=wp_pass123

# WordPress Admin (will be created during installation)
# These are just for your reference
WP_ADMIN_USER=admin
WP_ADMIN_PASSWORD=admin123
WP_ADMIN_EMAIL=admin@vulnerable-lab.local

# Additional Configuration
WORDPRESS_TABLE_PREFIX=wp_
```

> ⚠️ SECURITY NOTE:
These are intentionally weak credentials for a lab environment. NEVER use such simple passwords in production. For real-world scenarios, use strong, randomly generated passwords and store them in a secure password manager.

Save the file.

**STEP 5: Create PHP Upload Configuration**

This allows uploading larger files (useful when uploading shells and exploits):
```
nano uploads.ini
```

Content:
```
file_uploads = On
memory_limit = 512M
upload_max_filesize = 128M
post_max_size = 128M
max_execution_time = 600
max_input_time = 600
max_input_vars = 3000
```

Save the file.

**STEP 6: Create Logs Directory**

```
mkdir logs
```
This will store Apache access and error logs, useful for analyzing attacks.

**STEP 7: Verify Project Structure**

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/60.png)

---

## PART 4: DEPLOYING THE LAB

**STEP 1: Build the Docker Images**
```
docker compose build
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/61.png)

**STEP 2: Launch the Containers**

```
docker compose up -d
```

The `-d` flag runs containers in detached mode (background).

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/62.png)

**STEP 3: Verify Containers Are Running**

```
docker compose ps
```
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/63.png)

All three should show Up status.
Check logs if something went wrong:
```
docker compose logs
```

Or for specific container:
```
docker compose logs wordpress
```

**STEP 4: Get Your Ubuntu VM's IP Address**

We need this IP so Kali can access the lab:
```
ip addr show
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/64.png)

## PART 5: COMPLETING WORDPRESS INSTALLATION

**STEP 1: Access WordPress from Ubuntu Desktop**

Open Firefox or Chrome in Ubuntu and navigate to: [http://localhost:8080](http://localhost:8080).  
You should see the WordPress installation wizard.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/65.png)

**STEP 2: Select Language**

Choose your preferred language (English recommended for consistency with pentesting tools) and click Continue.

**STEP 3: Complete Installation Form**

Fill in the following:  
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/66.png)

> 🎯 Pentesting Note: We're using common, weak credentials (admin:admin123) intentionally. This makes the lab realistic for practicing:

- Brute force attacks
- Credential stuffing
- Default credential exploitation

Click "Install WordPress"

**STEP 4: Login to WordPress**

You should see "Success!" Click Log In.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/67.png)

Enter:  
- **Username**: admin    
- **Password**: admin123

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/68.png)

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/69.png)

Welcome to your WordPress admin dashboard!

---

## PART 6: ACCESSING THE LAB FROM KALI LINUX

Now let's configure access from your Kali Linux pentesting machine.
**STEP 1: Verify Network Connectivity**

From your Kali Linux VM, open a terminal and ping the Ubuntu VM:
```
ping -c 4 192.168.1.195
```
You should see responses:
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/70.png)

Press Ctrl + C to stop.  
If ping fails:
1. Check VM network settings:
- Both VMs should be on Bridged Adapter (same physical network)
- Or both on Internal Network with same network name
- Or NAT Network (not simple NAT)

2. Check Ubuntu firewall:
```
# On Ubuntu
   sudo ufw status
```

If active, allow the necessary ports:
```
sudo ufw allow 8080/tcp
sudo ufw allow 3306/tcp
sudo ufw allow 8081/tcp
```

3. Verify containers are listening on all interfaces:

```
# On Ubuntu
sudo netstat -tulpn | grep 8080
```

Should show `0.0.0.0:8080` not `127.0.0.1:8080`

**STEP 2: Access WordPress from Kali**

From Kali Linux, open a browser and navigate to:
```
http://192.168.1.195:8080/
```
(Use your Ubuntu VM's actual IP)

You should see the WordPress site!

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/71.png)

Try accessing PHPMyAdmin.

```
http://192.168.1.195:8081
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/72.png)

Login with:
- Server: db (or leave empty)
- Username: root
- Password: toor123 (from your .env file)

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/73.png)

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/74.png)

**STEP 3: Verify Direct MySQL Access**
From Kali, test MySQL connection:

Enter password: `toor123`

```
mysql -h 192.168.1.195 -P 3306 -u root -p --ssl-verify-cert=OFF
```

If connected, you'll see:
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/75.png)


Type `exit;` to disconnect.
This confirms SQLMap and other tools can access the database directly.

---


## PART 7: MAKING WORDPRESS INTENTIONALLY VULNERABLE

Now let's add specific vulnerabilities for pentesting practice.

**1: Install Vulnerable Plugins**  

**A. File Manager 6.0 (Vulnerable - CVE-2020-25213)**

```
docker exec -it wordpress-app bash
cd /var/www/html/wp-content/plugins
wget https://downloads.wordpress.org/plugins/wp-file-manager.6.0.zip
unzip wp-file-manager.6.0.zip
unzip wp-file-manager/wp-file-manager.6.0.zip
chown -R www-data:www-data file-manager
exit
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/78.png)

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/79.png)

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/81.png)

Now activate the plugin from WordPress:
- Go to Plugins → Installed Plugins
- Search for ‘File Manager’
- Click on Activate

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/80.png)

**B. Mail Masta (LFI Vulnerability)**

```
docker exec -it wordpress-app bash
cd /var/www/html/wp-content/plugins
apt install git
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/82.png)

```
apt update
apt install git -y
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/83.png)

```
git clone https://github.com/p314dO/mail-masta.git
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/84.png)

Vulnerability: [Local File Inclusion allowing reading sensitive files](https://www.exploit-db.com/exploits/50226)
Activate from admin panel.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/85.png)

**C. Social Warfare (RCE)**

```
docker exec -it wordpress-app bash
cd /var/www/html/wp-content/plugins
wget https://downloads.wordpress.org/plugin/social-warfare.3.5.2.zip
unzip social-warfare.3.5.2.zip
rm -rf social-warfare.3.5.2.zip
chown -R www-data:www-data social-warfare
exit
```
Vulnerability: [CVE-2019-9978 - Remote Code Execution](https://unit42.paloaltonetworks.com/exploits-in-the-wild-for-wordpress-social-warfare-plugin-cve-2019-9978/)   
Activate from admin panel.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/86.png)

**D. XMLRPC Available**

List available methods.
```
curl -X POST -d '<?xml version="1.0"?><methodCall><methodName>system.listMethods</methodName></methodCall>' http://localhost:8080/xmlrpc.php
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/87.png)

List available capabilities.

```
curl -X POST -d '<?xml version="1.0"?><methodCall><methodName>system.getCapabilities</methodName></methodCall>' http://localhost:8080/xmlrpc.php
```
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/88.png)


**E. Allow User Registration**
1. Go to Settings → General
2. Check "Anyone can register"
3. Set New User Default Role to Subscriber (or Editor for more access)
4. Click Save Changes

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/89.png)

This allows attackers to create accounts, useful for practicing privilege escalation.  

**F. Disable Comment Moderation**
1. Go to Settings → Discussion
2. Uncheck "Comment must be manually approved"
3. Uncheck "Comment author must have a previously approved comment"
4. Click Save Changes

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/90.png)

This enables comment-based XSS injection practice.

**F. Use Predictable Permalinks**
1. Go to Settings → Permalinks
2. Select "Day and name" or "Numeric"
3. Click Save Changes

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/91.png)

Makes enumeration easier for pentesting practice.

**G. Create Vulnerable Users**

From Users → Add New, create several users:  

| Username     | Email              | Role       | Password   |
|--------------|--------------------|------------|------------|
| editor1      | editor@lab.local   | Editor     | editor123  |
| author1      | author@lab.local   | Author     | author123  |
| subscriber1  | sub@lab.local      | Subscriber | sub123     |

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/92.png)

These provide targets for:  
- User enumeration
- Brute force attacks
- Privilege escalation practice

**H. Add Vulnerable Themes**

Twentyfifteen.2.0
```
sudo docker exec -it wordpress-app bash
cd /var/www/html/wp-content/themes
wget https://downloads.wordpress.org/theme/twentyfifteen.2.0.zip
unzip twentyfifteen.2.0.zip
rm -rf twentyfifteen.2.0.zip
chown -R www-data:www-data twentyfifteen
exit
```

Twentyseventeen
```
docker exec -it wordpress-app bash
cd /var/www/html/wp-content/themes
wget https://downloads.wordpress.org/theme/twentyseventeen.1.0.zip
unzip twentyfifteen.2.0.zip
rm -rf twentyfifteen.2.0.zip
chown -R www-data:www-data twentyfifteen
exit
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/93.png)


---

## PART 8: INITIAL RECONNAISSANCE FROM KALI