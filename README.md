# DockM compose setup

A simple setup to deploy DockM with custom templates.

This setup also comes with [watchtower](https://hub.docker.com/r/v2tec/watchtower/) to automatically upgrade DockM version :)

# Requirements

1. Install [Docker](http://docker.io).
2. Install [Docker-compose](http://docs.docker.com/compose/install/).
3. Clone this repository

# Usage

The default configuration will connect DockM against the local Docker host, using an nginx container (port 80).

Run it:

```
$ docker-compose up -d
```

And then access DockM by hitting [http://localhost/dockm](http://localhost/dockm) with a web browser.

# Configuration

## How can I specify which Docker host I want to manage?

You'll need to pass the IP/hostname of your Docker host to the dockm binary.

Update the `command` field of the **dockm** service in the `docker-compose.yml` file:

```yml
dockm:
  image: click2cloud/dockm
  container_name: "dockm-app"
  command: --templates http://templates/templates.json -d /data -H tcp://<DOCKER_HOST>:<DOCKER_PORT>
  networks:
    - local
```

## How can I specify my own templates?

Create the file `templates/templates.json` and insert your template definitions in it.

For more information about the template definition format, see: https://github.com/click2cloud/templates

Then, bind mount the file for the **templates** service in the `docker-compose.yml` file:

```yml
templates:
  image: click2cloud/templates
  container_name: "dockm-templates"
  networks:
    - local
  volumes:
    - ./templates:/usr/share/nginx/html
```
