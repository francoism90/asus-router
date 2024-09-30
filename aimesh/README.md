# AiMesh

You need to connect over SSH to your AiMesh node.
The IP Address is listed on the Network-tab of the AiMesh node.

The credentials and SSH-key (recommended) is the same as the main router, as they are synced:

```bash
ssh 192.168.x.x
```

> Note: Changing an AiMesh node directly, may cause it stop working.
> It is recommended to work with nvram variables, and not using the web interface itself.
> See Troubleshooting when the node doesn't connect anymore.

## Disable route redirect

This allows you to enter the AiMesh-node interface:

```bash
nvram set re_mode=0
nvram set re_mode=1 # revert afterwards!
```

## Change AiMesh node mode

```bash
nvram set sw_mode=1 # choose between 1-5
nvram set sw_mode=3 # revert afterwards (repeater-mode)
```

## Using scripts

It is recommended to have the same values defined in `wlboost`, even when the main router doesn't offer that feature as they are in sync (e.g. the `wl2` interface may not exists on the AiMesh router, but it's settings are still being pushed to the node).

You can also run `wlboost` on the AiMesh node.
Even when it should sync settings, it seems not to touch everything.

## Disable syncing channels between nodes

This seems to control channel syncthing, however it may cause your node being unable to connect to the AiMesh router.

Please use this option with care!

```bash
nvram set wl0_chsync=0
nvram set wl1_chsync=0
nvram set wl2_chsync=0
```

Only run `nvram commit` on the AiMesh node, if restarting the node doesn't cause any issues!

## Troubleshooting

It's possible to restore factory nvram settings by using a hard reset:

- <https://www.asus.com/support/faq/1039077/>
- <https://www.asus.com/support/faq/1039078/>

This will clear all overwrites and restores factory defaults.
