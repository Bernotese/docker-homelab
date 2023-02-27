
# Homelab Setup with Docker

My personal homelab configuration with docker. Includes traefik, pihole, portainer, dashy, mysql, phpmyadmin, grafana, nextcloud and uptime-kuma.

---

## Requirements

You need to have docker and docker compose installed. It was tested and run on Ubuntu 20.04 LTS and Docker Version 23.0.1.

---

##  Docker Installation

### Docker & Docker-Compose installation

I installed docker using the bash script:

    curl -fsSL https://get.docker.com -o get-docker.sh
    sh get-docker.sh

And insalled docker-compose with:

    apt install docker-compose

---

## Directorys for app-data

The data from the cointainers is stored on the host filesystem. I created for every service  who needs to store dataa single directory. You can adapt it to your needs (keep in mind to change it in the composefiles too). Change the permission of the directories so that the apps/docker can access them.

### 0. Traefik

    mkdir /data/app_data/traefik
    mkdir /data/app_data/traefik/ssl-certs
    mkdir /data/app_data/traefik/etc-traefik

### 1. Pihole

    mkdir /data/app_data/pihole/
    mkdir /data/app_data/pihole/etc-pihole
    mkdir /data/app_data/pihole/etc-dnsmasq.d

### 2. Portainer

    mkdir /data/app_data/portainer
    mkdir /data/app_data/portainer/data

### 3. Dashy

    mkdir /data/app_data/dashy

### 4. MySQL

    mkdir /data/app_data/mysql

### 5. Nextcloud

    mkdir /data/app_data/nextcloud

### 6. Uptime-Kuma

    mkdir /data/app_data/uptime-kuma

---

## Copy Configs

Copy the configs for traefik and dashy.

### 1. Traefik Config

    cp /00-traefik/traefik.yml /data/app_data/traefik/etc-traefik/traefik.yml

#### 2. Dashy

    cp /03-dashy/dashy_conf.yml /data/app_data/dashy/dashy_conf.yml

--- 

## Create Network for Traefik

Traefik needs a "backend" network where the services are deployed in. You can choose any name but have to change it in the compose files too.

    docker network create traefik_web

---

## Run the containers

Choose the App you want to run and go into the directory and run the following command:

    docker-compose up -d 

Required Containers for DNS are traefik and pihole. To access your app over dns you need to create a "local dns record" in pihole. To access pihole visit:

    http://<dockerhost-ip>:8080/admin

There you create a local record for each app wich points to the dockerhost ip address.
If you want to change the dns names of the app, you need to change it in the docker-compose file of the service in the labels section.

---