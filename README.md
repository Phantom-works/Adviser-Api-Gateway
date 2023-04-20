# Adviser-Api-Gateway
***

## Installation guide
***

This guide provides steps to configure Kong Gateway on Docker. for other installations feel free to use the installation guides listed on [Kongs](https://docs.konghq.com/gateway/latest/install/) website.

### 1. Create a Docker network

Run the following command:

```
docker network create kong-net
```
You can name this network anything you want. I'll use kong-net as an example throughout this guide.

### 2. Downlaod the [Kong.yaml](https://github.com/Phantom-works/Adviser-Api-Gateway/blob/main/kong.yml) file from the Gateways repository



### 3. Start Kong Gateway in DB-less mode

1. From the same directory where you just saved the kong.yml file, run the following command to start a container with Kong Gateway:

```
docker run -d --name kong-dbless \
  --network=kong-net \
  -v "$(pwd):/kong/declarative/" \
  -e "KONG_DATABASE=off" \
  -e "KONG_DECLARATIVE_CONFIG=/kong/declarative/kong.yml" \
  -e "KONG_PROXY_ACCESS_LOG=/dev/stdout" \
  -e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" \
  -e "KONG_PROXY_ERROR_LOG=/dev/stderr" \
  -e "KONG_ADMIN_ERROR_LOG=/dev/stderr" \
  -e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" \
  -p 8000:8000 \
  -p 8443:8443 \
  -p 8001:8001 \
  -p 8444:8444 \
  kong:3.2.2
```

Where:
+ --name and --network: The name of the container to create, and the Docker network it communicates on.

+ -v $(pwd):/path/to/target/: Mount the current directory on your local filesystem to a directory in the Docker container. This makes the kong.yml file visible from the Docker container.

+ KONG_DATABASE: Sets the database to off to tell Kong not to use any backing database for configuration storage.

+ KONG_DECLARATIVE_CONFIG: The path to a declarative configuration file inside the container. This path should match the target path that you’re mapping with -v.

+ All _LOG parameters: set filepaths for the logs to output to, or use the values in the example to print messages and errors to stdout and stderr.

+ KONG_ADMIN_LISTEN: The port that the Kong Admin API listens on for requests.

+ KONG_ADMIN_GUI_URL: (Enterprise only) The URL for accessing Kong Manager, preceded by a protocol (for example, http://).

+ KONG_LICENSE_DATA: (Enterprise only) If you have a license file and have saved it as an environment variable, this parameter pulls the license from your environment.
