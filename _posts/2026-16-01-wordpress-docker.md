---
title: Wordpress - 2. Build your own lab with Docker
image:
    path: /assets/posts/2026-14-01-build-your-own-wordpress/1.png
date: 2026-01-16 06:48:05 +/-TTTT
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

Si estas viendo esto...aun no lo termine. Tengo ganas de dormir...lo terminare pronto. Bye!!