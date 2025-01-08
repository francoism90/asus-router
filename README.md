# Disclaimer

> Following instructions are provided without any warranty, and may even get you in trouble legally.<br>
> The instructions are provided for testing, learning, preventing e-waste, and should be use with care.<br>
> We (including contributers + commentators) are not responsible for any damage to your device(s) or any legal issues.

## Introduction

This repo provides information on how one may possibly enable additional or change WiFi-channels, and TX-power on ASUS Merlin provided routers. It also offers some tweaks for AiMesh nodes.

Please use the instructions with care, and read the disclaimer before applying any changes.

The purpose is to adjust a router of AP to the legal state of the country (e.g. you bought an ASUS router in JAP, and want to re-use again it in GER). The current method is focussing on exposed nvram variables, and overrule them again when a service or event has been restarted or dispatched.

The adjusted nvram settings have been tested on the ASUS RT-AX58U v1 (Asuswrt-Merlin) & ASUS ZenWiFi AX XT8 v2 (AiMesh node - stock firmware).

> Note: Please do not ask for any support on the SNBForums, ASUS or any (official) Merlin related projects about these tweaks. See the Troubleshooting section to reset any changes, or remove any adjustments first if you need to ask for support/help.

> Note: Most modern wireless devices use something called Location Aware Regulatory (LAR), and other hidden signals to check the origin of the country.
> This means that even changing/forcing your router's WiFi-settings, your clients may choose to ignore it (use a fallback-mode) or do not connect at all. See troubleshooting if this happens in your case.

A massive shoutout goes out to the contributes on my previous [gist](https://gist.github.com/francoism90/3dede7973354d067c41bff5e54203fe9/), and members of the [SNBForums](https://www.snbforums.com/) for findings and information about certain parameters.

## Getting started

> See <https://www.htpcguides.com/enable-ssh-asus-routers-without-ssh-keys/> for instructions. The guide explains both the usage of a password and SSH-keys.

Login into router using SSH (password or key):

1. Run `nvram dump > dump.txt`
2. Use `scp` to copy it to your working machine or copy the contents of `nvram dump` to somewhere safe.
3. Make sure `Enable JFFS custom scripts and configs` is enabled and working in System settings.
4. Reboot router

### User-scripts

> See <https://github.com/RMerl/asuswrt-merlin.ng/wiki/User-scripts> for details.

1. Make sure JFFS has been enabled:

```bash
nvram set jffs2_on=1
nvram set jffs2_scripts=1
nvram commit
reboot
```

2. Create/update the required `/jffs/scripts` files, see [given example](https://github.com/francoism90/asus-router/tree/main/jffs/scripts) for details.

3. Update your device `nvram` overwrites into the `/jffs/scripts/wlupdate` file.

4. Make sure scripts are executable:

```bash
chmod a+rx /jffs/scripts/*
```

5. Apply nvram overwrites:

```bash
/jffs/scripts/wlupdate
```

If the overrules work, you can make them persistent on reboots:

```bash
nvram commit
reboot
```

## Testing

To validate the wireless settings, you may want to use the Android app `com.vrem.wifianalyzer`.

To restart the wireless service, use `service restart_wireless` or by using the scMerlin addon.

The script `/jffs/scripts/wlboost` may be useful to force testing if a channel can be selected.

You may also want to use _System Log_ for diagnostics:

![image](https://github.com/user-attachments/assets/3775cac7-58e0-4333-b0a9-4dcec775c9fc)

## Troubleshooting

It's possible to restore factory nvram settings by using a hard reset:

- <https://www.asus.com/support/faq/1039077/>
- <https://www.asus.com/support/faq/1039078/>

This will clear all overwrites and restores factory defaults.
