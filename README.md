# Traefik and Portainer with SSL

This repository contains setup instructions to secure your internal and external services using SSL. We will use Traefik as a reverse proxy and Portainer for container management, with wildcard certificates from Let's Encrypt.

## Prerequisites

- Ubuntu 22.04
- Docker
- Docker Compose

## Traefik Setup

1. Prepare files: Don't forget to change the configuration inside those files

    ```bash
    cd traefik/data
    chmod 600 acme.json
    ```

2. Create a Docker network:

    ```bash
    docker network create proxy

3. Start Traefik work-dir : /treafik (change the configuration inside the `docker-compose.yml`)

    ```bash
    docker-compose up -d
    ```

## Portainer Setup

1. Generate a basic auth password:

    ```bash
    sudo apt update
    sudo apt install apache2-utils
    echo $(htpasswd -nb "<USER>" "<PASSWORD>") | sed -e s/\\$/\\$\\$/g
    ```

    Replace `<USER>` and `<PASSWORD>` with your desired username and password.

2. Add the hashed password to your `docker-compose.yml` in the Portainer service definition.

3. Start Portainer work-dir : /portainer (change the configuration inside the `docker-compose.yml`)

    ```bash
    docker-compose up -d
    ```

## EXTRA (optional) :Traefik Routes Configuration

1. Create and edit `config.yml`:

    ```bash
    cd traefik/data
    nano config.yml
    ```

    - Refer to [Traefik GitHub](https://github.com/technotim/traefik-docker) for the content of `config.yml`.

2. Recreate Traefik containers:

    ```bash
    docker-compose up -d --force-recreate
    ```

Your folder structure should look like this:

```plaintext
./traefik
├── data
│   ├── acme.json
│   ├── config.yml
│   └── traefik.yml
└── docker-compose.yml
