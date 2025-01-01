# AiMesh

> Tip: If your router or AP doesn't support Asuswrt-Merlin, please checkout [asuswrt-scripts](https://github.com/jacklul/asuswrt-scripts/).

You need to connect over SSH to your AiMesh node.
The IP Address is listed on the Network-tab of the AiMesh node.

The credentials and SSH-key (recommended) is the same as the main router, as they are synced:

```bash
ssh 192.168.x.x
```

## Using scripts

It is recommended to have the same values defined in `wlboost`, even when the main router doesn't offer that feature as they are in sync (e.g. the `wl2` interface may not exists on the AiMesh router, but it's settings are still being pushed to the node(s)).

You may also want to setup `wlboost` on the AiMesh node (including service scripts). Comment out the first entries, and adjust the values for the AiMesh node.

## Experimental

> Warning: The following may cause your AiMesh-node from stop working!<br>
> See Troubleshooting if this happens.

The following are experimental, please use at your own risk.

### Disable route redirect

This allows you to enter the AiMesh-node interface:

```bash
nvram set re_mode=0
nvram set re_mode=1 # revert afterwards!
```

### Change AiMesh node mode

```bash
nvram get sw_mode # make sure to save current value!
nvram set sw_mode=2 # choose between 1-5
nvram set sw_mode=3 # revert afterwards to previous value or use 3 (repeater-mode)
reboot
```

It may take a while after the reboot for the node to be re-added again to the netwerk.

## Troubleshooting

It's possible to restore factory nvram settings by using a hard reset:

- <https://www.asus.com/support/faq/1039077/>
- <https://www.asus.com/support/faq/1039078/>

This will clear all overwrites and restores factory defaults.
