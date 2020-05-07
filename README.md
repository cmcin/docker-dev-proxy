# docker-dev-proxy

This project was inspired by and utilizes the great packages from @speto and @jwilder.

When running multiple projects on environments with docker on the same machine, how do we handle all of the conflicting ports? Using unique port numbers is cumbersome and a challenge to remember, so why not use a custom-mapped domain name. You should be able to use sobdomains of _localhost_ without any extra configuration, or setup a wildcard redirect to your machine using a .test domain.

Run the docker-dev-proxy script to launch a proxy containers for MySql and Web traffic, create a docker network for each, then scan running container names for a pattern match and add them to their appropriate network. This needs to be rerun whenever there is a change to the container configuration (i.e. start/stop a proxied container).

## Container Config

### MySQL
Add a label to the docker-compose config for `db.network.tunnel.hostname` with the name you will use to connect through the proxy to your MySQL server.
```
labels:
	db.network.tunnel.hostname: db_host_name
```

### Web
Add environment variables to control the address that the container will be available at (make sure that this domain name points to your machine).
```
environment:
	VIRTUAL_HOST: web-container.localhost
```

## Script Config
The proxy container configurations can be adjusted with an env file. Rename .env.example to .env and change the required settings there.