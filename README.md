# Disclaimer

> Following instructions are provided without any warranty, and may even get you in trouble legally.<br>
> The instructions are provided for testing, learning, preventing e-waste, and should be use with care.<br>
> We (including contributers + commentators) are not responsible for any damage to your device(s) or any legal issues.

# Introduction

This repo provides nvram adjusts that may enable additional channels and TX-power (in most cases don't) on ASUS Merlin provided routers.
The purpose is to expose all possible features first, and adjust them to the legal state of the country (e.g. you bought an ASUS router in JAP, and want to re-use it in GER).

It seems most nvram settings are the same for ASUS routers, but to be sure please dump + save your current nvram first!
The adjusted nvram settings have been tested on the ASUS RT-AX58U v1 + ASUS ZenWiFi AX XT8.

A massive shoutout to the contributes on my previous [gist](https://gist.github.com/francoism90/3dede7973354d067c41bff5e54203fe9/), and members of the [SNBForums](https://www.snbforums.com/)!

## Getting started

See https://www.htpcguides.com/enable-ssh-asus-routers-without-ssh-keys/ for instructions:

1. Login into router using SSH (password or key)
2. Run `nvram dump > dump.txt`
3. Use `scp` to copy it locally or copy the `nvram dump` directly
4. Make sure `Enable JFFS custom scripts and configs` is enabled in System settings (https://github.com/RMerl/asuswrt-merlin.ng/wiki/User-scripts).
5. Reboot router

### User-scripts

1. Create a `/jffs/scripts/wlboost` file (see [example](blob/main/jffs/scripts/wlboost), and paste your `nvram` overwrites into this file.

2. Create/adjust `/jffs/scripts/init-start`:

```sh
#!/bin/sh

[ -x /jffs/scripts/wlboost ] && /jffs/scripts/wlboost & # should be before any addons!
# [ -x /jffs/addons/AdGuardHome.d/AdGuardHome.sh ] && /jffs/addons/AdGuardHome.d/AdGuardHome.sh init-start &
```

3. Create/adjust `/jffs/scripts/services-start`:

```sh
#!/bin/sh

/jffs/scripts/wlboost >/dev/null 2>&1 & # wlboost
# /jffs/scripts/scmerlin startup & # scMerlin
```

4. Create/adjust `/jffs/scripts/service-event`:

```sh
#!/bin/sh

if echo "$2" | /bin/grep -q "wireless"; then { /jffs/scripts/wlboost service_event "$@" & }; fi # wlboost
```

5. Create/adjust `/jffs/scripts/service-event-end`:

```sh
#!/bin/sh

if echo "$2" | /bin/grep -q "wireless"; then { /jffs/scripts/wlboost service_event "$@" & }; fi # wlboost
```

6. Make sure scripts are executable:

```bash
chmod a+rx /jffs/scripts/*
```

## Applying changes

You need to reboot the router to apply changes:

```sh
/jffs/scripts/wlboost
reboot
``` 

To make the changes persistent, make sure to adjust the `jffs/scripts/wlboost` script.
