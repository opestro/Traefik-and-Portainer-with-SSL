# Traefik and Portainer with SSL

This repository contains setup instructions to secure your internal and external services using SSL. We will use Traefik as a reverse proxy and Portainer for container management, with wildcard certificates from Let's Encrypt.

## Prerequisites

- Docker
- Docker Compose

## Traefik Setup

1. Create necessary directories and files:

    ```bash
    mkdir -p traefik/data
    cd traefik/data
    touch acme.json
    chmod 600 acme.json
    touch traefik.yml
    ```

2. Create a Docker network:

    ```bash
    docker network create proxy
    ```

3. Create `docker-compose.yml`:

    ```bash
    cd ..
    touch docker-compose.yml
    ```

    - Refer to [Traefik GitHub](https://github.com/technotim/traefik-docker) for the content of `traefik.yml` and `docker-compose.yml`.

4. Start Traefik:

    ```bash
    docker-compose up -d
    ```

## Portainer Setup

1. Create necessary directories and files:

    ```bash
    mkdir -p portainer/data
    cd portainer
    touch docker-compose.yml
    ```

    - Refer to [Portainer GitHub](https://github.com/technotim/traefik-docker) for the content of `docker-compose.yml`.

2. Generate a basic auth password:

    ```bash
    sudo apt update
    sudo apt install apache2-utils
    echo $(htpasswd -nb "<USER>" "<PASSWORD>") | sed -e s/\\$/\\$\\$/g
    ```

    Replace `<USER>` and `<PASSWORD>` with your desired username and password.

3. Add the hashed password to your `docker-compose.yml` in the Portainer service definition.

4. Start Portainer:

    ```bash
    docker-compose up -d
    ```

## Traefik Routes Configuration

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
