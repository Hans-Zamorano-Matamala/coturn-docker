# Docker build for Coturn

Based on [https://github.com/coturn/coturn/](official) and [https://github.com/instrumentisto/coturn-docker-image/](instrumentisto), dockerfile.

## Added

* Default config file
* Entrypoint with exec form for arguments usage on docker run and docker-compose
* Documentation about build process and docker run or docker-compose usage (this document)

## Usage

### Image build

```bash
docker build -t NAMESPACE/IMAGE_NAME:TAG ./
```

Note: This clones the coturn git repository, obtaining the latest version.

### Running a container

```bash
# Docker run
docker run -p 3478-3479:3478-3479 -p 3478-3479:3478-3479/udp hzamorano/coturn:4.5.1.2 --log-file=stdout

# Docker-compose
docker-compose up

# Docker run with detached console
docker run -d -p 3478-3479:3478-3479 -p 3478-3479:3478-3479/udp hzamorano/coturn:4.5.1.2 --log-file=stdout

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
