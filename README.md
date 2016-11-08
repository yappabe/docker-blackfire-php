![Docker pulls](https://img.shields.io/docker/pulls/yappabe/blackfire-php.svg?style=flat)
# Yappa PHP FPM Docker Image with Blackfire

## Usage

Add the following to your docker-compose.yml file:

```YAML
php:
    image: yappabe/blackfire-php
    volumes_from:
        - app
    links:
        - mysql
```

## PHP version

To use a specific PHP version, append the version number to the image name.

Eg: `image: yappabe/blackfire-php:7.0`

The following PHP versions are available:

* PHP 7.0 (jessie stable)

## Configurations

Following environment vars are required to correctly run Blackfire:

```
ENV BLACKFIRE_SERVER_ID
ENV BLACKFIRE_SERVER_TOKEN
ENV BLACKFIRE_CLIENT_ID
ENV BLACKFIRE_CLIENT_TOKEN
```

You can find the values for these vars at [your blackfire account page](https://blackfire.io/account).

The following environment vars are not required, these are the defaults.

```
ENV BLACKFIRE_LOG_LEVEL 1
ENV BLACKFIRE_LOG_FILE /tmpfs/logs/blackfire.log
ENV BLACKFIRE_SOCKET unix:///var/run/blackfire/agent.sock
```

## Blackfire

Both the agent and the probe are installed. This way it's possible to profile requests and cli commands.
Using the Blackfire official image did not suffice as we need the agent to be present on the php container to be able to profile cli commands.

### Profiling requests

Follow the [instructions](https://blackfire.io/docs/integrations/chrome) for the Chrome Companion, if the environment vars are correctly set, this should be all that needs to be done.

### Profiling cli commands

Since the agent is present on this container, profiling cli commands is nothing more than running the command through the agent.
This is done like this: `blackfire run <command>`. If all goes well, a url will be provided where the generated profile can be found.