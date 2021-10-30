# Container based solutions

In this repository common container solutions will be documented.

The common red thread will to show that you can literally execute
any container based solution on Windows, Linux and Mac platform
that follows the Open Container Initiative specifications. A
container solution usually has a defined set of version based
dependencies and should always work every time you run the container.

The interest in this repository will be on Linux containers that
should be possible to execute on any of the mentioned platforms above. 
As long as graphic rendering is not mandatory. 

Graphic rendering in a Linux container requires special graphic
rendering network solution to be able to render graphics from the
Linux container to the Windows platform, resulting into degraded metal
speed performance, that is, works just fine with Linux/Mac host
machine.

## Compile WINE i386/amd64 in Docker

Dockerfile that compiles WINE version 6.20 from source with necessary development dependencies included for both
i386 and amd64 architecture in a Debian buster distro environment. The wine-staging patches will also be applied to
wine upstream specific version. This container should be possible to execute on Mac/Linux platform without any 
necessary tweaks, that is, wine/wine64 command in the container will be able to execute Windows games/applications
. You need to have the vulkan/mesa graphic drivers installed on your host machine to be able to render graphics from
Wine to your host machine via X11/xorg.

```bash
xhost +

docker build -f wine_from_source/Dockerfile -t wine-in-docker .
docker run -it \
-v ${PWD}/models:/models \
--device=/dev/dri \
--group-add video \
--volume=/tmp/.X11-unix:/tmp/.X11-unix \
--env="DISPLAY=$DISPLAY" \
--name install-wine-from-source \
wine-in-docker

# Start a game/application

WINEPREFIX="${WINE_PATH}" WINEARCH="${ARCHITECTURE}" ${WINE} <path to exe file>
```

