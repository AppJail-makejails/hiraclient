# HiraClient

HiraClient is an IRC client for the hirana.net network written in Electron with a beautiful web and desktop interface.

hirana.net

![hiranaclient logo](https://raw.githubusercontent.com/hirananet/HiraClient/main/src/assets/256x256.png)

## How to use this Makejail

```
INCLUDE options/network.makejail
INCLUDE gh+AppJail-makejails/hiraclient

ARG ruleset=0

OPTION template=files/linux.conf
OPTION devfs_ruleset=${ruleset}
# Recommended
OPTION tmpdir
# But this works fine without sharing /tmp
#OPTION x11
```

Where `options/network.makejail` are the options that suit your environment, for example:

```
ARG network
ARG interface=appjail0

OPTION alias=${interface}
OPTION virtualnet=${network} default
OPTION nat
```

In the above example `appjail0` is a loopback interface, so it must first exist before creating the jail.

The `files/linux.conf` template is as follows:

```
exec.start: /bin/true
exec.stop: /bin/true
persist
```

Open a shell and run `appjail makejail` and `appjail start`:

```sh
appjail makejail -j hiraclient -- --network development --ruleset 11
appjail start hiraclient
```

Your ruleset must unhide `shm` and `shm/*`.

After Makejail builds the jail, you can run HiraClient using the `hiraclient_open` custom stage:

```sh
appjail run -s hiraclient_open hiraclient
```

## Notes

1. Note that this Makejail will install `pulseaudio` on the host.
2. This Makejail uses [Ubuntu](https://github.com/AppJail-makejails/Ubuntu).
