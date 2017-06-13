# Ionic v3 Docker Image

This docker image provides the following installed:

* nodejs
* npm
* Ionic CLI (latest)
* Cordova CLI (latest)
* Android SDK (latest)

> For an Android SDK only docker image check out: [thyrlian/AndroidSDK](https://github.com/thyrlian/AndroidSDK)

## Building the image

From inside the repos directory run:

```
docker build -t ionic3 .
```
### ARG Variables

You can pass the following in using the `--build-args` option.
E.g. `docker build -t ionic3 --build-args NODE_VERSION=7.0.0 .`

> See `Dockerfile` for example and format for each option.

#### NODE\_VERSION
[Nodejs Releases](https://nodejs.org/en/download/releases/)

#### NPM\_VERSION
[NPM Releases](https://github.com/npm/npm/tags) or use `latest`

#### IONIC\_VERSION
[Ionic CLI Releases](https://github.com/driftyco/ionic-cli/tags) or `latest`

#### CORDOVA\_VERSION
[Cordova CLI Releases](https://github.com/apache/cordova-cli/tags) or `latest`

#### ANDROID\_BUILD\_TOOLS\_VERSION

[Build Tools Releases](https://developer.android.com/studio/releases/build-tools.html)

#### ANDROID\_PLATFORMS

SDK Platforms installed by default.


## Running

Running the following will start `ionic serve` for the current directory:

```
docker run -ti --privileged -v $PWD:/project:rw -p 8100:8100 -p 35729:35729 -p 53703:53703 ionic3

# Explanation
docker run
  -ti                           # interactive tty
  --rm                          # remove the container on exit
  --privileged                  # allow the container to access all devices on the host
  -v $PWD:/project:rw           # map the current directory to the project directory in the container
  -v /dev/bus/usb:/dev/bus/usb  # allows adb to connect to devices that were not present when the container started
  -p 8100:8100                  # default port for ionic serve
  -p 35729:35729 -p 53703:53703 # default ports used for live realod
  ionic3                        # docker image
  <command>                     # optional command to run in the container
```

For running other commands you can just append the command you want after
the command above.

e.g. for `npm install` run:
```
docker run -ti --rm -v $PWD:/project:rw ionic3 npm install
```

## Alias

You can alias the docker command to make it easier to run for multiple
projects:

```
alias ionic3="docker run -ti --rm --privileged -v $PWD:/project:rw -v /dev/bus/usb:/dev/bus/usb -p 8100:8100 -p 35729:35729 -p 53703:53703 ionic3"
```

## Running on and Android Device

To run your app on a plugged in device it is better to start a container
with bash, this will let you connect to the device using `adb` before
running any `cordova` commands.

```
docker run -ti --rm --privileged -v $PWD:/project:rw -v /dev/bus/usb:/dev/bus/usb ionic3 /bin/bash
adb start-server
ionic cordova run android --device
```


## Android SDK Commands

* sdkmanager - Update and install sdk packages. See [sdkmanager docs](https://developer.android.com/studio/command-line/sdkmanager.html)
* adb - connect to a device/emulator. See [Android Debug Bridge docs](https://developer.android.com/studio/command-line/adb.html)

> Full list can be found here: [Command Line Tools Guide](https://developer.android.com/studio/command-line/index.html)
