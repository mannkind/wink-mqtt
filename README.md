# Wink Local

[![Software
License](https://img.shields.io/badge/License-MIT-orange.svg?style=flat-square)](https://github.com/mannkind/wink-local/blob/master/LICENSE.md)
[![Travis CI](https://img.shields.io/travis/mannkind/wink-local/master.svg?style=flat-square)](https://travis-ci.org/mannkind/wink-local)
[![Coverage Status](https://img.shields.io/codecov/c/github/mannkind/wink-local/master.svg)](http://codecov.io/github/mannkind/wink-local?branch=master)
[![Go Report Card](https://goreportcard.com/badge/github.com/mannkind/wink-local)](https://goreportcard.com/report/github.com/mannkind/wink-local)

A local-control replacement for the Wink Hub that utilizes MQTT.

## Building

The following set of commands should help you build the application for the Wink Hub.

```
# Building the application
git clone https://github.com/mannkind/wink-local
cd wink-local
GOOS=linux GOARCH=arm GOARM=5 make
```

## Configuring

Configuration happens in the `wfs/opt/wink-local/wink-local.yaml` file, 
which ends up in `/opt/wink-local/` on the Wink Hub.  

An example might look this:

```
http:
    port: 8080
mqtt:
    clientid: 'WinkLocal'
    broker:   'tcp://mosquitto:1883'
    topicbase: 'winkhub'
    retain: false
```

## Installing

The following set of commands should help you copy the necessary files to the Wink Hub. 

```
GOREPO=wink-local
NODEREPO=$GOREPO/web
WFS=$GOREPO/wfs
WINKHUBHOST=winkhub

ssh $WINKHUBHOST "mkdir -p /opt/wink-local" 
scp -r $GOREPO/bin/linux_arm/wink-local \
    $NODEREPO/dist \
    $WFS/opt/wink-local/wink-local.yaml \
    $WINKHUBHOST:/opt/wink-local
scp $WFS/etc/rc.d/init.d/wink-local $WINKHUBHOST:/etc/rc.d/init.d/wink-local
cat $WFS/etc/monitrc | ssh $WINKHUBHOST "cat >> /etc/monitrc"
cat $WFS/etc/rc.d/init.d/wink-local | ssh $WINKHUBHOST "cat >> /etc/rc.d/init.d/wink-local"
```

# Notice - Unmaintained
Unmaintained. I decomissioned my Wink Hub in favor of a HUSBZB-1 ZWave/Zigbee stick. The repository will remain available for reference, but the code is unmaintained.
