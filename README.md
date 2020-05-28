# Docker build for Coturn

Based on [https://github.com/coturn/coturn/](official) and [https://github.com/instrumentisto/coturn-docker-image/](instrumentisto) dockerfile to build from sources, permit command line arguments, or execute with default parameters.

## Added

* Default command expects a config file in /etc/turnserver.conf
* Entrypoint with exec form for arguments usage on docker run and docker-compose
* Documentation about build process and docker run or docker-compose usage (this document)

**Notes:**

* This clones the coturn git repository, obtaining the latest version.
* To provide a custom configuration file, you need to use docker-compose and mount a turnserver.conf configuration file on /etc/turnserver.conf
* If no configuration file is provided, it will use default command-line settings.
* By default service logging is redirected to /var/log/turn_*.log. To see logs on console, set parameter --log-file=stdout

## Usage

### Image build

```bash
docker build -t NAMESPACE/IMAGE_NAME:TAG ./
```

### Running a container

```bash
# Docker run with default configuration file (with --rm container autoremoves after stopping)
docker run --rm -p 3478-3479:3478-3479 -p 3478-3479:3478-3479/udp hzamorano/coturn:4.5.1.2

# Docker run with custom 'realm' parameter
docker run --rm -p 3478-3479:3478-3479 -p 3478-3479:3478-3479/udp hzamorano/coturn:4.5.1.2 --realm=NEW_REALM

# Docker run with detached console
docker run --rm -d -p 3478-3479:3478-3479 -p 3478-3479:3478-3479/udp hzamorano/coturn:4.5.1.2

# Run with docker-compose
docker-compose up

# Docker-compose with detached console
docker-compose up -d
```

## Service testing

You can use Tricle ICE website to test service functionality [https://webrtc.github.io/samples/src/content/peerconnection/trickle-ice/]((https://webrtc.github.io/samples/src/content/peerconnection/trickle-ice/)):

1. Add your serviceIP:port on "STUN or TURN URI"
2. Press "Add server"
3. Press "Gather candidates"
4. You should see in the results a response of type **srflx**:

  ```log
  Time  Component Type  Foundation  Protocol  Address Port  Priority
  0.004 rtp host  1636205621  udp SOMEIP.ADDRESS  SOME.PORT 126 | 30 | 255
  0.005 rtp srflx 842163049 udp SOMEIP.ADDRESS SOME.PORT 100 | 30 | 255
  0.104 Done
  ```
