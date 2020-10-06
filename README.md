# Create New Docker Network

Before running the docker compose create a new docker network named *proxy* or use any other name you want to use :
```
$ docker network create proxy
```

# Creating directories for traefik 

To use the reverse proxy called traefik we are going to use the same directory of `/opt`
```
$ sudo mkdir -p /opt/docker/{traefik,tmp}
```

* Copying the traefik configuration

So that traefik work we need the configuration files so that he interprets our website url

```
$ sudo cp -R traefik/ /opt/docker/
 
```

# Configuring docker compose our Traefik Dashboard

Within the docker file compose this domain of our site must be changed so that bringfik can identify the request with which we are working

```
-traefik.frontend.rule=Host:monitor.${DOMAIN}
```

# Setup file of traefik

Inside the folder of traefik there is a file called .env which has all the configuration parameters of traefik to be able to execute it, which are the following

```
TRAEFIK_VERSION=v2.0
DOMAIN=example.com
HTTP_PORT=80
HTTPS_PORT=443
TRAEFIK_TOML_FILE_ROUTE=/opt/docker/traefik/traefik.toml
TRAEFIK_ACME_FILE_ROUTE=/opt/docker/traefik/acme.json
TRAEFIK_TMP_ROUTE=/opt/docker/traefik/tmp
```
We need to create our user and password to enter the traefik dashboard. For this we modify the file traefik.toml that we just copied in line 22 [entryPoints.foo.auth.basic], putting our username and password, the password is in the format `htpasswd `which can be generated in the following link https://www.web2generators.com/apache-tools/htpasswd-generator

```
[entryPoints.foo.auth.basic]
users = ["user:$apr1$6orwmnzb$YRm1DtL0zVYz9KSdiO8Cy0"]
Example for User=admin and Password=admin is admin:$apr1$14fkxcbe$R3XDQXwrmrHp1XNuPJesa/
```

# Launching the container

## TREFIK CONTAINER

The first container that we are going to execute is the one of traefik which will allow us to reach our container by means of url

execute the following commands

```
$ docker-compose up -d
```
- Wait for the contain to run
- Validate the container by entering in the browser to the following address http://monitor.example.com, the localhost.com is configured in the .env file of traefik,will ask us for a username and password that are the ones we configured earlier


