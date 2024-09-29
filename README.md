# Disclaimer

> Following instructions are provided without any warranty, and may even get you in trouble legally.<br>
> The instructions are provided for testing, learning, preventing e-waste, and should be use with care.<br>
> We (including contributers + commentators) are not responsible for any damage to your device(s) or any legal issues.

# Introduction

This repo provides nvram adjusts that may enable additional channels and TX-power (in most cases don't) on ASUS Merlin provided routers.

The purpose is to adjust them to the legal state of the country (e.g. you bought an ASUS router in JAP, and want to re-use it in GER).

It seems most nvram settings are the same for ASUS routers, and are being synced when using AiMesh.
The adjusted nvram settings have been tested on the ASUS RT-AX58U v1 + ASUS ZenWiFi AX XT8 (AiMesh node).

To be sure the nvram adjustments doesn't (soft-)brick your router/APs, please `nvram dump` first, and save the current nvram dump somewhere safe!

A massive shoutout goes out to the contributes on my previous [gist](https://gist.github.com/francoism90/3dede7973354d067c41bff5e54203fe9/), and members of the [SNBForums](https://www.snbforums.com/)!

> Please note: do not ask for any support on the SNBForums, ASUS or any (official) Merlin related projects. See the Troubleshooting section to reset any changes, or remove any adjustments first if you need to ask for support/help.

## Getting started

> See https://www.htpcguides.com/enable-ssh-asus-routers-without-ssh-keys/ for instructions.

Login into router using SSH (password or key):

1. Run `nvram dump > dump.txt`
2. Use `scp` to copy it locally or copy the `nvram dump` directly
3. Make sure `Enable JFFS custom scripts and configs` is enabled in System settings
4. Reboot router

### User-scripts

> See https://github.com/RMerl/asuswrt-merlin.ng/wiki/User-scripts for details.

1. Create/update the required `/jffs/scripts` files, see [given example](https://github.com/francoism90/asus-router/tree/main/jffs/scripts) for details, and paste your `nvram` overwrites into the `/jffs/scripts/wlboost` file.

2. Make sure scripts are executable:

```bash
chmod a+rx /jffs/scripts/*
```

3. You need to reboot the router to apply changes:

```sh
/jffs/scripts/wlboost
reboot
```

## Testing

To make changes persistent, adjust the `/jffs/scripts/wlboost` file.

Restart the wireless service using `service restart_wireless` or by using the ScMerlin interface.

## Troubleshooting

It's possible to restore factory nvram settings by using a hard reset:
- https://www.asus.com/support/faq/1039077/
- https://www.asus.com/support/faq/1039078/

This will clear all overwrites and restores factory defaults.
