# alpine-pkg-openjfx

[![CircleCI](https://img.shields.io/circleci/project/sgerrand/alpine-pkg-java-openjfx/master.svg)](https://circleci.com/gh/sgerrand/alpine-pkg-java-openjfx)

This is [OpenJFX][openjfx] as an Alpine Linux package.

## Releases

See the [releases page](https://github.com/sgerrand/alpine-pkg-java-openjfx/releases) for the latest
download links.

## Installing

The current installation method for these packages is to pull them in using
`wget` or `curl` and install the local file with `apk`:

    apk --no-cache add ca-certificates wget
    wget --quiet --output-document=/etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub
    wget https://github.com/sgerrand/alpine-pkg-java-openjfx/releases/download/8.151.12-r0/java-openjfx-8.151.12-r0.apk
    apk add --no-cache java-openjfx-8.151.12-r0.apk

[openjfx]: https://wiki.openjdk.java.net/display/OpenJFX/Main
