
# Deploying WordPress with Docker on Ubuntu

This project demonstrates deploying **WordPress**, a popular content management system, using **Docker** and **Ubuntu**. 

---

## Table of Contents
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Cloning the Repository](#cloning-the-repository)
4. [Building and Starting Docker Containers](#building-and-starting-docker-containers)
5. [Configuring the Hosts File](#configuring-the-hosts-file)
6. [Setting Up NGINX Proxy (Optional)](#setting-up-nginx-proxy-optional)
7. [Downloading and Installing WordPress](#downloading-and-installing-wordpress)
8. [Accessing WordPress](#accessing-wordpress)
9. [Video](#video)
10. [Acknowledgements](#acknowledgements)


---

## Introduction

### What is WordPress?

**WordPress (WP, or WordPress.org)** is a free and open-source web content management system. Initially designed as a tool for blogging, it has evolved to support various web content types, including:
- Traditional websites
- Mailing lists and forums
- Media galleries
- Membership sites
- Learning management systems
- Online stores

As of December 2023, WordPress powers **43.1% of the top 10 million websites**, making it one of the most popular content management systems globally.

### Features of WordPress
- **Written in PHP** and paired with a **MySQL or MariaDB database**.
- Includes a **plugin architecture** and a **template system** referred to as "Themes."
- Can be installed on a web server or a local computer running the WordPress software package.


### Why Use Docker for WordPress?
Using Docker containers for WordPress deployment offers:
- Isolation of components like web server, PHP processor, and database.
- Easier portability and scalability.
- Simplified setup through Docker Compose.

This project leverages Docker to deploy WordPress locally or on a server, including:
- **Web Server**: NGINX
- **PHP Processor**: PHP-FPM
- **Database**: MariaDB

---

## Prerequisites

Ensure you have the following:
- A system with Docker and Docker Compose installed
- **Optional**: NGINX installed on the host system for reverse proxy

---

## Cloning the Repository

Clone the repository containing the necessary Docker Compose files and configurations:

```bash
git clone https://github.com/iabhinavr/local-php-site-docker.git
```

Rename the folder to reflect your project:

```bash
mv local-php-site-docker wgtreks
cd wgtreks
```

---

## Building and Starting Docker Containers

Build the Docker containers:

```bash
docker compose build
```

Start the containers in detached mode:

```bash
docker compose up -d
```

Verify the containers are running:

```bash
docker ps | grep wgtreks
```

---

## Configuring the Hosts File

Update the hosts file to map the domain name to the loopback IP address:

```bash
sudo nano /etc/hosts
```

Add the following line:

```plaintext
127.0.0.1 wgtreks.local
```

Save and exit the file.

---

## Setting Up NGINX Proxy (Optional)

Install NGINX on the host system:

```bash
sudo apt install nginx -y
```

Create a new site configuration file for the proxy:

```bash
sudo nano /etc/nginx/sites-available/wgtreks.local
```

Add the following content:

```nginx
server {
    listen 127.0.0.1:80;

    server_name wgtreks.local;

    location / {
        proxy_pass http://wgtreks.local:8010;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_read_timeout 600s;
    }
}
```

Enable the site by creating a symbolic link:

```bash
sudo ln -s /etc/nginx/sites-available/wgtreks.local /etc/nginx/sites-enabled/wgtreks.local
```

Reload NGINX to apply the new configuration:

```bash
sudo systemctl reload nginx
```

---

## Downloading and Installing WordPress

Navigate to the public directory:

```bash
cd ./app/public
```

Download the latest version of WordPress:

```bash
wget http://wordpress.org/latest.tar.gz
```

Extract the downloaded archive:

```bash
tar -xzvf latest.tar.gz
```

Move WordPress files to the public directory:

```bash
mv wordpress/* .
```

Delete the downloaded archive:

```bash
rm -f latest.tar.gz
```

---

## Accessing WordPress

1. Open your browser and navigate to `http://wgtreks.local`.
2. Follow the WordPress installation wizard:
   - Set up database credentials (defined in `docker-compose.yml`).
   - Enter site title, admin username, password, and email.
3. Complete the installation and log in to the WordPress dashboard.

---


## Video



https://github.com/user-attachments/assets/a6a8fee7-fb35-45f7-a9cc-84c8ad623a06


---

## Acknowledgements

Special thanks to:
- **Prof. Ashok Harnal**: For providing insights into Docker-based deployments.
- **Abhinav Reddy (CoralNodes)**: For video guidance and sharing the repository and configuration files.

---
