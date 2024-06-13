# dereck-redis

This repository contains the configuration and deployment files for setting up a Redis instance using Docker Compose. The Redis instance is intended to be used by applications running on the local server and is not exposed publicly to the world.

## Usage

To start the Redis container, run the following command:

```bash
yarn docker-run:dev
```

This command will start the Redis container using the configuration specified in the docker-compose.yml file.

To stop the Redis container, run:

```bash
yarn docker-stop:dev
```

### Testing Redis

To test the Redis instance from inside the local machine, you can use the redis-cli command-line interface. Here are a couple of useful commands:

```bash
# Connect to the Redis instance:
docker exec -it linode_dereck-redis redis-cli
# Set a key-value pair:
> SET mykey "Hello, Redis!"
# Retrieve the value of a key:
> GET mykey
# Check the number of keys in the database:
> DBSIZE
# Disconnect from the Redis CLI:
> QUIT
```

### Persistent Data

The Redis data is persisted across container deletions and restarts using a named volume called dereck-redis-data. You can inspect the volume using the following commands:

```bash
docker volume ls
docker volume inspect dereck-redis-data
```

To delete the volume and remove all persisted data, run:

```bash
yarn docker-data-delete:dev
```

## CI/CD

This repository includes a GitHub Actions workflow defined in release_prod.yml that automatically deploys the Redis container to a Linode server whenever a new release is published on GitHub.

The workflow copies the docker-compose.yml file to the Linode server, pulls the latest Redis image, stops any existing containers, prunes unused containers, images, and volumes, and starts the new Redis container using the updated configuration.

Make sure to set the necessary secrets in your GitHub repository settings for the Linode server connection details.
Security

The Redis instance is configured to be accessible only within the local server environment and does not require a password for simplicity. It is not exposed publicly to the world.

If you need to add a password for additional security, you can modify the docker-compose.yml file to include the command: redis-server --requirepass <your-password> option and update the corresponding environment variables in the package.json and release_prod.yml files.

## references

```bash
ssh root@104.200.17.204
```
