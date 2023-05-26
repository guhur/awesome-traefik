# List of projects compatible with Traefik 


`Traefik` is a modern edge router. It considerably helps the installation of services. 

It is ideal to easily manage your own self-hosted services.

My own servers are running over Ubuntu 18, 20 and 22. I am using a recent version of docker:

```bash
$ docker --version
Docker version 20.10.18, build b40c2f6
$ docker compose version
Docker Compose version v2.10.2
```

Here is a list of services that can directly be launched:

- [appsmiimth](#generic) is a low-code platforme for building internal CRUD interfaces.
- [collabora](#collabora) is an online version of LibreOffice
- [freescout](#freescout) is a help-desk
- [matomo](#matomo) is a web-analytics tool
- [metabase](#metabase) is a business intelligence tool
- [mautic](#generic) is a mailing management system
- [n8n](#generic) is an automatization platform
- [nginx](#generic) is used for static file delivery
- [nextcloud](#nextcloud) is a file-hosting service
- [nocodb](#nocodb) is a no-code database alternative to Airtable
- [php](#generic) is an Nginx with PHP 7.1 container
- [portainer](#generic) is a tool for Docker
- [weblate](#generic) is a translation management system 
- [wordpress](#generic) is the most famous content management system


If you need another service, you can open an issue or contact me directly on Linkedin.


# :beaver: Global installation: configuration and Traefik

Copy the `.env` in `.env.local` and edit the latter with your own configuration.

Copy the `traefik/.env` file anywhere else (it could be your `/etc/` or `traefik/.env.local`). 

Launch it with:

```bash
bin/cmd -c boot -s traefik -e /path/to/your/env 
```

# :bug: Debugging 

Because `traefik` is nicely integrated with Docker, installation and maintenance have been much easier than with `nginx`.
However, I can sometimes spend half-a-day installing a new service, and I found it useful to gather a check-list for troubleshooting:

1. Does the service appear in `docker stats`? If not, you haven't been able to start it. Check errors during the building of your container.

1. Does the service quickly die and restart in `docker stats`? It means that the service has an issue. Go the service folder, and look at the docker logs with: 

```bash
bin/cmd -s log -s [service] -e /path/to/your/env
```

1. Is the service unresponding? It might be a wrong configuration. Add a port forwarding in the `docker-compose.yaml` and see if you can use the service locally. For example, in `collobora/docker-compose.yml`, add:

```
    ports:
      - "9980:9980"
```

Restart the container and visit: `http://localhost:9980`. You should obtain a "OK" message.

1. Is it still not working? Start an interactive session with `docker exec -it [name-of-the-container] sh` 


# Installing services

## Generic

The generic solution is as below. It works for `collabora`, `mautic`, `n8n` and `portainer`.
Let's call the service you want `service`. 
Copy the `service/.env` file anywhere else (it could into your `/etc/` or `service/.env.local`). 

Then, launch the container:

```bash
bin/cmd -c boot --env-file /path/to/your/env
```

## Nextcloud

For Nextcloud, you can install the service `collabora` in addition to the [generic](#generic) installation.

Then, on your Nextcloud apps, disable "Built-in CODE" and enable "Nextcloud Office". Then, in your settings, on the "Nextcloud Office" pan, put the collabora FQDN.


## Plex

https://howto.wared.fr/plex-docker-traefik-ubuntu/

```bash
sudo adduser plex
sudo adduser plex docker
sudo su plex
cd

```


## Pontoon

Make sure you loaded submodules:

```bash
git submodule update --init --recursive
```

Copy the `pontoon/pontoon/docker/config/server.env.template` file as `pontoon/ pontoon/.env`. Edit its values. 
Similarly, copy `pontoon/.env` as `pontoon/.env.local` and edit it. Add the same postgress password.

Launch the container:

```bash
bin/cmd -c boot -s pontoon -e pontoon/.env
```

## Crater

```bash
cd crater
cp .env.example .env
docker-compose up -d
./docker-compose/setup.sh
```

# Stopping a service

If you still have access to your `.env` file:

```bash
bin/cmd -c down -s [service] -e /path/to/service's/.env
```

Otherwise, you might not be able to use docker compose, and you need hence to stop containers one by one: (i) find the container name(s) (e.g. with `docker stats`), and (ii) launch `docker stop [containername]`.

# :pray: Similar repository

- [lfache](https://github.com/lfache/awesome-traefik).
- [tomMoulard](https://github.com/tomMoulard/make-my-server)

