# DHT Sensor Logger - Docker Image

## Disclaimer

This project is complete overengineering – although it is fully functional, it was more about learning Docker and GitHub than delivering optimal value.

## Overview

This Docker image logs temperature and humidity data from a DHT22 like sensor and stores it in a CSV file and InfluxDB.

### Features:

- Reads data from DHT22 sensor
- Logs temperature and humidity in CSV files
- Stores data in InfluxDB for further analysis
- Uses Loguru for enhanced logging

## How to Use

### Pull the Image

To pull the latest version of the Docker image:

```sh
docker pull ghcr.io/andrzejsiemion/logger:latest
```

Or pull a specific version:

```sh
docker pull ghcr.io/andrzejsiemion/logger:0.0.3
```

### Run the Container

```sh
docker run -d --name logger \
  -e LOG_INTERVAL=60 \
  -e DHT_PIN=D4 \
  -e INFLUXDB_URL=http://influxdb:8086 \
  -e INFLUXDB_TOKEN=my_secret_token \
  -e INFLUXDB_ORG=my_org \
  -e INFLUXDB_BUCKET=sensor_data \
  ghcr.io/andrzejsiemion/logger:latest
```

## Environment Variables

| Variable                     | Default                | Description                                |
| ---------------------------- | ---------------------- | ------------------------------------------ |
| `LOG_INTERVAL`               | `60`                   | Interval (in seconds) between sensor reads |
| `DHT_PIN`                    | `D4`                   | GPIO pin connected to the sensor           |
| `DHT_SENSOR_TEMP_CORRECTION` | `0`                    | Correction factor for temperature readings |
| `DHT_SENSOR_HUM_CORRECTION`  | `0`                    | Correction factor for humidity readings    |
| `INFLUXDB_URL`               | `http://influxdb:8086` | InfluxDB server URL                        |
| `INFLUXDB_TOKEN`             | `my_secret_token`      | InfluxDB authentication token              |
| `INFLUXDB_ORG`               | `my_org`               | InfluxDB organization                      |
| `INFLUXDB_BUCKET`            | `sensor_data`          | InfluxDB bucket name                       |

## Available Tags

| Tag      | Description                            |
| -------- | -------------------------------------- |
| `latest` | Latest stable release                  |
| `0.0.7`  | Added temperature fine adjustments |

Check all available versions [here](https://github.com/andrzejsiemion/logger/pkgs/container/logger).

## Updating the Image

To pull the latest version and restart your container:

```sh
docker pull ghcr.io/andrzejsiemion/logger:latest
docker stop logger
docker rm logger
docker run -d --name logger ghcr.io/andrzejsiemion/logger:latest
```

## Dockerfile Overview

The Dockerfile follows a two-stage build:

1. **Stage 1: Builder**
   - Installs necessary dependencies for DHT22 sensor and InfluxDB client.
   - Uses `python:3.9-slim` as the base image.
2. **Stage 2: Runtime**
   - Uses `python:3.9-alpine` for a smaller image size.
   - Includes only essential dependencies.
   - Copies the script and sets up logging and data directories.

## License

This project is licensed under the MIT License – see the [LICENSE](LICENSE) file for details.

## Support & Contributions

- If you find issues, open an [issue](https://github.com/andrzejsiemion/logger/issues).
- Contributions are welcome! Submit a [pull request](https://github.com/andrzejsiemion/logger/pulls).

