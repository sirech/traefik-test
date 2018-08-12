# Traefik Test

This is intended to test [Traefik](https://traefik.io/) capabilities

## Requirements

- [Docker](https://www.docker.com/) 
- [docker-compose](https://docs.docker.com/compose/) >= 3

## Setup

### Enable domain

In order to test the echo app, you need to add this line in your `/etc/hosts` file:

```
127.0.0.1 echo.testing.com
```

Generate a self signed certificate for the url with:

```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout traefik/certs/cert.key -out traefik/certs/cert.crt
```

### Running the code

_Traefik_ and the two example apps are run in different compose files:

```
docker-compose -f docker-compose.base.yml up --build
docker-compose -f docker-compose.app1.yml up
docker-compose -f docker-compose.app2.yml up
```

## Access

- The ui is available under `localhost:8080`
- The first app is reached through `localhost/findme`
- The second app is reached through `https://echo.testing.com`

## What is being tested

- *Discovery*: Traefik picks new applications automatically: When you start one of the example apps it appears in the ui automatically
- *Routing*: Traefik sends requests based on the configuration, which is passed through labels. No restart in `traefik` itself is needed for this
- *Password Access for the admin interface*: It is generated through `htpasswd`
- *Load Balancing*: Two instances of the same app receive traffic using `wrr`
- *SSL Termination*: Using a self signed certificate, the requests to the echo app are sent through http

## Open Questions

- Authentication: Can you do auth by sending the request to a SAML provider through the `auth.forward` directive?

## Pending

- Dynamic update of the `traefik.toml`
