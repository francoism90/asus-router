# Disclaimer

> Following instructions are provided without any warranty, and may even get you in trouble legally.<br>
> The instructions are provided for testing, learning, preventing e-waste, and should be use with care.<br>
> We (including contributers + commentators) are not responsible for any damage to your device(s) or any legal issues.

## Introduction

This repo provides information on how one may possibly enable additional or change WiFi-channels, and TX-power on ASUS Merlin provided routers. Please use the instructions with care, and read the disclaimer before applying any changes.

The purpose is to adjust a router of AP to the legal state of the country (e.g. you bought an ASUS router in JAP, and want to re-use again it in GER). The current method is focussing on exposed nvram variables, and overrule them again when a service or event has been restarted or dispatched. Since the overruling happens in memory, the nvram writes should be minimal.

The adjusted nvram settings have been tested on the ASUS RT-AX58U v1 (Asuswrt-Merlin) & ASUS ZenWiFi AX XT8 v2 (AiMesh node - stock firmware).

> Note: Please do not ask for any support on the SNBForums, ASUS or any (official) Merlin related projects about these tweaks. See the Troubleshooting section to reset any changes, or remove any adjustments first if you need to ask for support/help.

> Note: Most modern wireless devices use something called Location Aware Regulatory (LAR), and other hidden signals to check the origin of the country.
> This means that even changing/forcing your router's WiFi-settings, your clients may choose to ignore it (use a fallback-mode) or do not connect at all. See troubleshooting if this happens in your case.

A massive shoutout goes out to the contributes on my previous [gist](https://gist.github.com/francoism90/3dede7973354d067c41bff5e54203fe9/), and members of the [SNBForums](https://www.snbforums.com/) for findings and information about certain parameters.

## Getting started

> See <https://www.htpcguides.com/enable-ssh-asus-routers-without-ssh-keys/> for instructions. The guide explains both the usage of a password and SSH-keys.

Login into router using SSH (password or key):

1. Run `nvram dump > dump.txt`
2. Use `scp` to copy it to your working machine or copy the contents of `nvram dump` directly to somewhere safe
3. Make sure `Enable JFFS custom scripts and configs` is enabled and working in System settings
4. Reboot router

### User-scripts

> See <https://github.com/RMerl/asuswrt-merlin.ng/wiki/User-scripts> for details.

1. Make sure JFFS has been enabled:

```bash
nvram set jffs2_on=1
nvram set jffs2_scripts=1
nvram commit
```

2. Create/update the required `/jffs/scripts` files, see [given example](https://github.com/francoism90/asus-router/tree/main/jffs/scripts) for details, and paste your `nvram` overwrites into the `/jffs/scripts/wlboost` file.

3. Make sure scripts are executable:

```bash
chmod a+rx /jffs/scripts/*
```

4. Test the changes:

```bash
/jffs/scripts/wlboost
```

5. If everything keeps working, reboot the router:

```bash
reboot
```

6. You may want to permanently write the changes into nvram to let it survive on a reboot:

```bash
/jffs/scripts/wlboost
nvram commit
```

## Testing

To make changes persistent, adjust the `/jffs/scripts/wlboost` file.

Restart the wireless service using `service restart_wireless` or by using the scMerlin addon. These methods are preferred, since the router itself may overrule values.

## Troubleshooting

It's possible to restore factory nvram settings by using a hard reset:

- <https://www.asus.com/support/faq/1039077/>
- <https://www.asus.com/support/faq/1039078/>

This will clear all overwrites and restores factory defaults.
