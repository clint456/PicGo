# EdgeX MQTT Device Service

This repository contains Docker Compose configuration for running the EdgeX MQTT Device Service.

## Overview

The EdgeX MQTT Device Service enables EdgeX to communicate with MQTT-based devices. This service acts as a bridge between MQTT topics and the EdgeX framework.

## Prerequisites

Before running this service, ensure you have:
- Docker installed (version 19.03 or later)
- Docker Compose installed (version 1.27 or later)
- Required environment variables set

## Environment Variables

Create a `.env` file in the same directory as `docker-compose.yml` with the following variables:

```bash
# Device service configuration
DEVICE_SVC_REPOSITORY=edgexfoundry
ARCH=
DEVICE_MQTT_VERSION=3.0.0

# Command flags
CP_FLAGS=

# User configuration
EDGEX_USER=2002
EDGEX_GROUP=2001
```

## Configuration Files

### docker-compose.yml
The main Docker Compose file that defines the device-mqtt service with:
- Service dependencies (core-keeper, core-data, core-metadata, mqtt-broker, core-common-config-bootstrapper)
- Network configuration (edgex-network)
- Security options (no-new-privileges)
- Volume mounts (timezone synchronization)

### common-non-security.env
Environment variables shared across EdgeX services in non-security mode, including:
- Registry configuration
- Database configuration (Redis)
- Message bus configuration
- Client service hosts

## Usage

1. Set up environment variables:
```bash
cp .env.example .env
# Edit .env with your configuration
```

2. Start the service:
```bash
docker-compose up -d
```

3. Check service status:
```bash
docker-compose ps
```

4. View logs:
```bash
docker-compose logs -f device-mqtt
```

5. Stop the service:
```bash
docker-compose down
```

## Service Details

- **Container Name**: edgex-device-mqtt
- **Hostname**: edgex-device-mqtt
- **Port**: 59982 (exposed on localhost only)
- **Network**: edgex-network (bridge driver)
- **Restart Policy**: always
- **Read-Only Filesystem**: Yes (security hardening)

## Dependencies

The device-mqtt service depends on the following services:
- `core-keeper`: Configuration and registry service
- `core-data`: Core data microservice
- `core-metadata`: Core metadata microservice
- `mqtt-broker`: MQTT broker service
- `core-common-config-bootstrapper`: Configuration bootstrapper

## Security

The service is configured with the following security features:
- Read-only root filesystem
- No new privileges security option
- User/group isolation
- Localhost-only port binding

## Troubleshooting

### Service fails to start
- Check that all dependent services are running
- Verify environment variables are correctly set
- Check Docker logs for error messages

### Cannot connect to MQTT broker
- Verify `MQTTBROKERINFO_HOST` is set correctly
- Ensure the mqtt-broker service is running and accessible

### Configuration issues
- Verify `common-non-security.env` file exists and contains valid settings
- Check that all required environment variables in `.env` are set

## Additional Resources

- [EdgeX Foundry Documentation](https://docs.edgexfoundry.org/)
- [EdgeX Device MQTT Service](https://github.com/edgexfoundry/device-mqtt-go)
- [MQTT Protocol](https://mqtt.org/)
