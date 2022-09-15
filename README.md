# List of projects compatible with Traefik 


`Traefik` is the next generation of reverse-proxy web servers. Think of it as `nginx`, but with `docker` (+ `k8s`, and so on...).

It is ideal to easily manage your own self-hosted services.

My own servers are running over Ubuntu 18, 20 and 22. I am using a recent version of docker:

```bash
$ docker --version
Docker version 20.10.18, build b40c2f6
$ docker compose version
Docker Compose version v2.10.2
```

Here is a list of services I frequently use:

- [authelia](#authelia) (soon)
- [collabora](#collabora) (soon)
- [mailu](#mailu) (soon)
- [mattermost](#mattermost) (soon)
- [mautic](#generic)
- [n8n](#generic)
- [nextcloud](#nextcloud)
- [plex](#plex) (soon)
- [portainer](#generic)

If you need another service, you can open an issue or contact me directly.


# :beaver: Installation of Traefik

Copy the `traefik/.env` file anywhere else (it could into your `/etc/` or `traefik/.env.local`). 

Install Docker, and launch the container:

```bash
cd traefik
docker compose --env-file /path/to/your/env up -d 
```

# :bug: Debugging 

I personally found that installing self-hosting services is much less harassing than with `nginx`.
However, I can sometimes spend half-a-day installing a new service. Sometimes, an update might cause an issue with my install. 

Here is my troubleshooting quick-list:

1. Does the service appear in `docker stats`? If not, you haven't been able to start it. Check errors during the building of your container.

1. Does the service quickly die and restart in `docker stats`? It means that the service has an issue. Go the service folder, and look at the docker logs with: 

```bash
docker compose --env-file /path/to/your/env logs -f
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
cd service
docker compose --env-file /path/to/your/env up -d 
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
cd pontoon
docker compose --env-file pontoon/.env up -d 
```


# Stopping a service

If you still have access to your `.env` file:

```bash
cd service
docker compose --env-file /path/to/service's/env up -d 
```

If things are... more complicated... find the container name(s) (e.g. with `docker stats`) and launch `docker stop [containername]`.

# :pray: Credits

Inspiration of this repo comes from [lfache](https://github.com/lfache/awesome-traefik).
