# AiMesh

> Tip: If your router or AP doesn't support Asuswrt-Merlin, checkout [asuswrt-scripts](https://github.com/jacklul/asuswrt-scripts/).

You need to connect over SSH to your AiMesh node.
The IP Address is listed on the Network-tab of the AiMesh router.

The credentials and SSH-key (recommended) is the same as the main router (they are synced):

```bash
ssh 192.168.x.x
```

## Using scripts

It is recommended to have the same values defined in `wlupdate`, even when the main router doesn't offer that feature as they are in sync (e.g. the `wl2` interface may not exists on the AiMesh router, but it's settings are still being pushed to the node(s)).

You may also want to setup `wlupdate` (and `wlboost`) with the same values on the AiMesh node (including service scripts). If your current node doesn't has Merlin support, you may want to setup [asuswrt-scripts](https://github.com/jacklul/asuswrt-scripts/). This also offers [event](https://github.com/jacklul/asuswrt-scripts?tab=readme-ov-file#user-content-service-eventsh) support, which should help to maintain the interface overrules.

## Access Web GUI

> Warning: The following may cause your AiMesh-node from stop working!<br>
> See Troubleshooting if this happens.

### Disable route redirect

This allows you to enter the AiMesh-node interface:

```bash
nvram set re_mode=0
```

This means you can change your WiFi settings (and open other pages):

- <http://192.168.x.x/Advanced_Wireless_Content.asp>
- <http://192.168.x.x/Advanced_WAdvanced_Content.asp>

After a reboot, it may take some time before the AiMesh node is ready again:

```bash
nvram set re_mode=1
reboot
```

## Troubleshooting

It's possible to restore factory nvram settings by using a hard reset:

- <https://www.asus.com/support/faq/1039077/>
- <https://www.asus.com/support/faq/1039078/>

This will clear all overwrites and restores factory defaults.

### Change AiMesh node mode

```bash
nvram get sw_mode # make sure to save current value!
nvram set sw_mode=2 # choose between 1-5
nvram set sw_mode=3 # revert afterwards to previous value or use 3 (repeater-mode)
reboot
```

It may take a while after the reboot for the node to be re-added again to the netwerk.
