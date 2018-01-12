# Google Cloud Pub/Sub Emulator

A [Google Cloud Pub/Sub Emulator](https://cloud.google.com/pubsub/docs/emulator) container image. The image is meant to be used for creating an standalone emulator for testing.

## Environment

The following environment variables must be set:

- `PUBSUB_LISTEN_ADDRESS`: The address should refer to a listen address, meaning that `0.0.0.0` can be used. The address must use the syntax `HOST:PORT`, for example `0.0.0.0:8432`. The container must expose the port used by the Pub/Sub emulator.
- `PUBSUB_PROJECT_ID`: The ID of the Google Cloud project for the emulator.

## Connect application with the emulator

The following environment variables need to be set so your application connects to the emulator instead of the production Cloud Pub/Sub environment:

- `PUBSUB_EMULATOR_HOST`: The listen address used by the emulator.
- `PUBSUB_PROJECT_ID`: The ID of the Google Cloud project used by the emulator.

## Custom commands

This image contains a script named `start-pubsub` (included in the PATH). This script is used to initialize the Pub/Sub emulator.

### Starting an emulator

By default, the following command is called:

```sh
start-pubsub
```

## Creating a Pub/Sub emulator with Docker Compose

The easiest way to create an emulator with this image is by using [Docker Compose](https://docs.docker.com/compose). The following snippet can be used as a `docker-compose.yml` for a Pub/Sub emulator:

```YAML
version: "2"

services:
  pubsub:
    image: singularities/pubsub-emulator
    environment:
      - PUBSUB_PROJECT_ID=project-test
      - PUBSUB_LISTEN_ADDRESS=0.0.0.0:8432
    ports:
      - "8432:8432"
```

### Persistence

The image has a volume mounted at `/opt/data`. To maintain states between restarts, mount a volume at this location.
