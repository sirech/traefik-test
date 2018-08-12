# Traefik Test

This is intended to test [Traefik](https://traefik.io/) capabilities

## Requirements

- [Docker](https://www.docker.com/) 
- [docker-compose](https://docs.docker.com/compose/) >= 3

## Running the code

_Traefik_ and the two example apps are run in different compose files:

```
docker-compose -f docker-compose.base.yml up --build
docker-compose -f docker-compose.app1.yml up
docker-compose -f docker-compose.app2.yml up
```

## Access

- The ui is available under `localhost:8080`
- The first app is reached through `localhost/findme`
- The second app is reached through `localhost/echo`

## What is being tested

- *Discovery*: Traefik picks new applications automatically: When you start one of the example apps it appears in the ui automatically
- *Routing*: Traefik sends requests based on the configuration, which is passed through labels. No restart in `traefik` itself is needed for this
- *Password Access for the admin interface*: It is generated through `htpasswd`
- *Load Balancing*: Two instances of the same app receive traffic using `wrr`

## Open Questions

- Authentication: Can you do auth by sending the request to a SAML provider through the `auth.forward` directive?

## Pending

- Dynamic update of the `traefik.toml`
