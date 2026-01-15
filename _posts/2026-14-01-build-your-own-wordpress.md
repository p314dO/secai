---
title: Build your own lab for Wordpress
image:
    path: /assets/posts/2026-14-01-build-your-own-wordpress/1.png
date: 2026-01-15 06:48:05 +/-TTTT
categories: [Wordpress]
tags: [Wordpress, Tutorial]
---

WordPress powers more than 40% of websites worldwide, making it a critical target for pentesters and ethical hackers. This lab will allow you to practice enumeration techniques, vulnerability exploitation, privilege escalation, and hardening in a controlled and legal environment. It is the perfect foundation to prepare for certifications such as OSCP, eJPT, or CEH.

## Introduction

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/2.png)

Did you know that WordPress is one of the most attacked CMS platforms in the world? And that’s no coincidence: it’s everywhere. From personal blogs to massive corporate websites.

Today you are going to build something every ethical hacker needs: your own fully vulnerable and legal WordPress lab, where you can practice pentesting techniques without getting into trouble.
The beauty of this is that everything will run on your own machine, isolated from the outside world. We are going to set up a virtual machine with Ubuntu Server and then configure WordPress from scratch.

I won’t lie to you: this requires using the terminal. But I will guide you command by command. By the end, you will not only have your lab up and running, but you will also understand exactly how WordPress works under the hood. And that, trust me, is pure gold when you are looking for vulnerabilities.

## Prepare the virtual machine

First, you need virtualization software. Go to [access.braodcom.com](https://access.broadcom.com/default/ui/v1/signin/), create an account and download VMWare.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/3.png)

The installation is just next, next, next. Nothing out of the ordinary.
Now we need the operating system for our virtual machine. We are going to use [Ubuntu Server](https://ubuntu.com/download/server).

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/4.png)

Ubuntu Server is perfect for this because it’s free, stable, and it’s what you’ll find on most real-world web servers. Download the latest LTS version. LTS stands for “Long Term Support.” At the time of write this, version 22.04.3 is the last one.

Open VMWare and click in "Create a New Virtual Machine"
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/5.png)

Select Typical.  
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/6.png)

Click in browser and select the Ubuntu Server iso.
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/7.png)

Put a pretty name.  
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/8.png)

Choose whatever you want.  
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/9.png)

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/10.png)

Click in "Power on this virtual machine"
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/11.png)

Ubuntu Server will boot in text mode. Don’t panic if you don’t see nice graphical windows—this is normal. Servers don’t need graphical interfaces because they only waste resources.
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/12.png)



The installer is quite user-friendly:  
**Language**: Choose the one you prefer. I go with **English** because most documentation and forums are in English.
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/13.png)

**Keyboard**: Select your keyboard layout correctly, or you are going to suffer when typing passwords.


![Image](./assets/posts/2026-14-01-build-your-own-wordpress/14.png)

**Installation type**: Leave the default **Ubuntu Server** option.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/15.png)

**Network**: If you configured **NAT**, Ubuntu will probably obtain an IP address automatically via DHCP. Write down this IP because you will need it later.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/16.png)

**Proxy address**: We dont need one.
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/17.png)

**Mirror address:** Let it run and done.
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/18.png)

**Disk**: Use the entire disk. You don’t need LVM for this simple lab. Next Done and Continue.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/19.png)
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/20.png)
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/21.png)

**User and password**: Create your administrator user here. Use something easy to remember because you will type it a THOUSAND times. But in a real production environment, you would obviously use something much more secure.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/22.png)

**Enable Ubuntu Pro**: Skip for Now and Continue.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/23.png)

**OpenSSH Server**: When the installer asks which additional software you want, **CHECK** the **OpenSSH server** option. This will allow you to connect to your VM from a more comfortable terminal on your host system. It’s extremely useful for copying and pasting commands.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/24.png)

Click continue, accept the changes, and wait for the installation to finish.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/25.png)

When it finishes, it will ask you to reboot. Do it, and remove the virtual ISO in VMWare so it doesn’t boot from the installer again.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/26.png)

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/27.png)

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/28.png)

## Introduction To SSH

Before starting the full installation, let’s configure something that will make your life a THOUSAND times easier: **SSH**. SSH (Secure Shell) allows you to connect to your Ubuntu server from the terminal of your main computer. Why is this so great?

- **Without SSH**: You work in the uncomfortable VMWare window, copying and pasting is painful, and the resolution is terrible.  
- **With SSH**: You use your favorite terminal, copy and paste commands effortlessly, open multiple tabs, and even transfer files easily.

For an ethical hacker, SSH is essential—not only because it makes you more efficient, but because this is exactly how you would access compromised servers in the real world.

### 1. Verify that openssh is installed

First, you need to be logged into your Ubuntu virtual machine. If you followed the installation we covered earlier, you should already have the OpenSSH Server installed (we checked it during the Ubuntu installation).

Let’s verify that it is running:
```
sudo systemctl status ssh
```
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/29.png)

If you see **active (running)** in green, perfect. SSH is working.   
If for some reason it is **NOT** installed or not running, install it like this:

```
sudo apt update
sudo apt install openssh-server -y
```

Y luego inícialo:

```
sudo systemctl start ssh
sudo systemctl enable ssh
```

- **start ssh**: Starts the service immediately.
- **enable ssh**: Makes it start automatically every time you power on the VM.

### 2. Get the IP address of your server

To connect via SSH, you need to know the IP address of your virtual machine. Run:

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/30.png)

### 3. Connect via SSH

Now comes the magic. Open a terminal on your main computer and verify that you have connectivity with the WordPress server.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/31.png)

Now, connect via SSH.


![Image](./assets/posts/2026-14-01-build-your-own-wordpress/32.png)

## Install the LAMP stack

Perfect. You now have Ubuntu up and running. Now comes the part where we turn this basic server into a machine capable of running WordPress.
LAMP is an acronym: **Linux, Apache, MySQL, PHP**. These are the four fundamental technologies that power WordPress and millions of websites.

First, always—**ALWAYS**—update the system before installing anything new:

```
sudo apt update
sudo apt upgrade -y
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/33.png)

- `apt update` refreshes the list of available packages.
- `apt upgrade` installs the updates.
The `-y` flag automatically answers “yes” to everything.

This may take a few minutes depending on how many updates are available.

## Installing APACHE

Apache is the web server. It is the software that listens for HTTP requests and serves web pages. Let’s install it:
```
sudo apt install apache2 -y
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/34.png)

Once it’s installed, start the service:

```
sudo systemctl start apache2
```
And enable it so it starts automatically every time you power on the VM:

```
sudo systemctl enable apache2
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/35.png)

Now, how do we verify that it works? We need the IP address again.
Open a browser on your main computer, type that IP address into the address bar, and press Enter.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/36.png)

If you see the default Apache page that says **“It works!”** or **“Apache2 Ubuntu Default Page,”** congratulations! Your web server is working.

## Installing MySQL

MySQL is our database. WordPress needs to store posts, users, configurations—everything goes into MySQL.

```
sudo apt install mysql-server -y
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/37.png)

Once it’s installed, we need to secure it. This is important because, by default, MySQL comes with insecure settings intended for rapid development.

```
sudo mysql_secure_installation
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/38.png)

This script will ask you several questions:
- **Validate Password Component**: This enforces strong passwords. For a lab, you can say “No,” but in production you should say “Yes.”
- **Root password**: Set a password for the MySQL root user. **WRITE IT DOWN.**
- **Remove anonymous users**: Yes.
- **Disallow root login remotely**: Yes. We don’t want root connecting from outside.
- **Remove test database**: Yes.
- **Reload privilege tables**: Yes.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/39.png)

## Installing PHP

PHP is the programming language WordPress is written in. But we don’t just need PHP—we also need several modules that WordPress uses:
```
sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip -y
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/40.png)

Let me explain a few:
- **libapache2-mod-php**: Connects Apache with PHP
- **php-mysql**: Allows PHP to communicate with MySQL
- **php-curl**: For making HTTP requests
- **php-gd**: For image manipulation
- **php-mbstring**: Handles multibyte strings

The others are for specific WordPress functionalities.

Verify that PHP is installed:
```
php -v
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/41.png)

You should see something like **“PHP 8.1.x”** or similar.

## Prepare the Database

Before installing WordPress, we need to create a dedicated database for it. Let’s log into MySQL:

```
sudo mysql -u root -p
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/42.png)

It will ask for the root password you set earlier.
Now you are inside MySQL. The prompt changes to **mysql>**. Here we will run SQL commands. First, let’s create the database:
```
CREATE DATABASE wordpress;
```

Now let’s create a specific user for WordPress with its password:
```
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'Your_Secure_Password';
```

Important: Replace **'your_secure_password'** with a real password. Write it down because you will need it.
Now, let’s grant this user full permissions on the WordPress database:

```
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
```

Reload the privileges so the changes take effect:

```
FLUSH PRIVILEGES;
```

And exit MySQL:
```
EXIT;
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/43.png)

Perfect. You now have:
- **Database**: wordpress
- **User**: wpuser
- **Password**: the one you chose
- **Permissions**: full access to the WordPress database

## Download and install wordpress

Now, let’s get WordPress. We’ll download it directly from wordpress.org.
First, navigate to the temporary directory:

```
cd /tmp
```

```
wget https://wordpress.org/latest.tar.gz
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/44.png)

`wget` is like a downloader from the terminal. **latest.tar.gz** always points to the newest version.
Once downloaded, extract the compressed file:

```
tar -xvzf latest.tar.gz
```

- **x**: extract
- **v**: verbose (shows what it’s doing)
- **z**: decompress gzip
- **f**: file

This creates a folder called **wordpress** in `/tmp`.
Now we need to move everything to the directory where Apache serves websites. That directory is `/var/www/html/`:

```
sudo mv wordpress /var/www/html/
```

But there’s a problem: these files belong to your user. Apache runs under a special user called **www-data**, and it needs to own these files to be able to read and write them.

Let’s change the ownership:
```
sudo chown -R www-data:www-data /var/www/html/wordpress
```

`chown` changes the owner. **-R** means recursive (all files and subfolders). **www-data:www-data** is user:group.
We also set the correct permissions:
```
sudo chmod -R 755 /var/www/html/wordpress
```

**755** means:
- **Owner (www-data)**: read, write, execute (**7**)
- **Group**: read, execute (**5**)
- **Others**: read, execute (**5**)

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/45.png)

This is important for security. We don’t want just anyone to be able to write to our files.

## Configure Wordpress

```
cd /var/www/html/wordpress
```

WordPress comes with a sample configuration file that we need to copy and edit:
```
sudo cp wp-config-sample.php wp-config.php
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/46.png)

```
sudo nano wp-config.php
```

Find these lines and replace them with your database information:

```
define('DB_NAME', 'wordpress');
define('DB_USER', 'wpuser');
define('DB_PASSWORD', 'tu_contraseña_segura');
define('DB_HOST', 'localhost');
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/47.png)

- **DB_NAME**: the name of your database
- **DB_USER**: the user we created
- **DB_PASSWORD**: the password you set
- **DB_HOST**: localhost, because MySQL is on the same machine

To save in **nano**: Ctrl + O, then Enter to confirm.
To exit: Ctrl + X.

## Configure Apache for Wordpress

CONFIGURE APACHE FOR WORDPRESS

Apache can serve multiple websites at the same time. Each one has its own configuration. Let’s create one for WordPress.

```
sudo nano /etc/apache2/sites-available/wordpress.conf
```

Paste this inside (I’ll explain each part):
```
<VirtualHost *:80>
    ServerAdmin admin@example.com
    DocumentRoot /var/www/html/wordpress
    ServerName tu_ip_o_dominio

    <Directory /var/www/html/wordpress>
        Options FollowSymlinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/48.png)

- **<VirtualHost *:80>**: Listens on port 80 (HTTP) on all interfaces
- **DocumentRoot**: Where the site files are located
- **ServerName**: You can put your VM’s IP or a domain if you have one
- **AllowOverride All**: Allows WordPress to use **.htaccess** files to configure rules
- The logs will help you when you’re testing or hacking the site

Replace **[admin@example.com](mailto:admin@example.com)** and **your_ip_or_domain** with real values.
Save and exit.
Now, let’s enable this site:

```
sudo a2ensite wordpress.conf
```

**a2ensite** is an Apache command that enables site configurations.
WordPress uses friendly URLs, which require Apache’s **rewrite** module:

```
sudo a2enmod rewrite
```

Finally, restart Apache so the changes take effect:
```
sudo systemctl restart apache2
```

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/49.png)

Moment of truth. Open your browser and go to:
```
http://YOUR_IP/wordpress
```

If everything went well, you should see the WordPress web installer. We’re almost there!
Select your language and continue.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/50.png)

WordPress will ask you for the following:
- **Site Title**: Something like “Ethical Hacking Lab” or whatever you prefer.
- **Username**: This will be your WordPress administrator user. Do NOT use “admin” in production, but for a vulnerable lab it’s fine.
- **Password**: WordPress will suggest a strong one, but you can use a simple password for the lab.
- **Email**: Any email will do; it can be fake in this case.
- **Search engine visibility**: Check the box to discourage search engines. We don’t want Google indexing our hacking lab.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/51.png)

Click **“Install WordPress.”**
BAM! Success! WordPress is installed.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/52.png)

Now you can log in with the username and password you just created.
Welcome to your WordPress administration panel. This is the command center of the entire site.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/53.png)

And if you visit http://YOUR_VM_IP/wordpress again, you’ll see your WordPress site running with the default theme.

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/54.png)

## Additional configurations for Ethical Hacking

OK, you have WordPress running, but let’s make a few additional configurations that will make your lab more useful and secure.
Configure the Firewall
Ubuntu comes with UFW (Uncomplicated Firewall), which is—well—uncomplicated.

```
sudo ufw enable
```

It will warn you that you might lose SSH connections. That’s fine—we are going to allow SSH:

![Image](./assets/posts/2026-14-01-build-your-own-wordpress/55.png)

```
sudo ufw allow 22/tcp
```

Now allow HTTP (port 80):
```
sudo ufw allow 80/tcp
```
And if in the future you want to set up HTTPS with SSL certificates:

```
sudo ufw allow 443/tcp
```

Check the status:
```
sudo ufw status
```
![Image](./assets/posts/2026-14-01-build-your-own-wordpress/56.png)

Perfect. Now your VM only accepts connections on the ports you explicitly authorized.

---

## Make Wordpress Intentionally vulnerable
Now comes something counterintuitive but essential for an ethical hacking lab: we are going to make WordPress vulnerable on purpose.

Why? Because you want to practice finding and exploiting vulnerabilities. On a fully updated and well-hardened WordPress installation, that’s very difficult.

Some ideas:
- Install old and vulnerable plugins: Look on wordpress.org for plugins with older versions that have known vulnerabilities. You can search databases such as the WPScan Vulnerability Database.
- Install vulnerable themes: The same applies to themes.
- Do not update: Leave WordPress on an older version if you want to practice specific exploits.

Insecure configurations:

* Enable XML-RPC (Settings → Writing)
* Allow user registration
* Leave comments unmoderated
* Use predictable permalinks

---

## Install Testing Tools
From your main machine (not the VM), you need tools to attack your lab:

WPScan: WordPress scanner
```
gem install wpscan
```
Or, on Kali Linux, it comes preinstalled.

```
wpscan --url http://YOUR_VM_IP/wordpress --enumerate u,p,t
```

This enumerates users, plugins, and themes.
- Burp Suite: Intercepting proxy to analyze web traffic.
- SQLMap: For SQL injection testing.
- Metasploit: Exploitation framework with WordPress-specific modules.

---

## Conclusion
Congratulations! You have just built a complete WordPress ethical hacking lab from scratch.

You now have:  
✓ An isolated virtual machine  
✓ Ubuntu Server configured  
✓ A working LAMP stack  
✓ WordPress installed and accessible  
✓ Firewall configured  
✓ A legal and safe environment to practice  

What can you practice here?

- **Enumeration**: WPScan, Nmap, plugin/theme discovery
- **Exploitation**: Vulnerable plugin exploits, SQL injection, XSS
- **Post-exploitation**: Privilege escalation, persistence, data extraction
- **Hardening**: Learn how to secure WordPress by applying countermeasures
- **Forensics**: Simulate attacks and then analyze logs

All of this without breaking any laws, without harming real websites, and from the comfort of your own computer.

**Pro tip**: Before you start hacking, take a snapshot of your VM in VirtualBox. That way you can experiment freely, and if something breaks, you can revert to the snapshot in seconds.

If you made it this far, you’re awesome. I know it was long, but now you understand exactly how WordPress works—from the operating system all the way up to the web application. That knowledge is power when you’re doing penetration testing.
