---
title: "sway-setup"
date: 2022-12-16
lastmod: 2022-12-29
draft: false
tags: ["Programming","Linux","Notes"]
categories: ["Linux"]
---

# Sway configuration 

## Configure keyboard language

Get all available input devices with `swaymsg -t get_inputs`.

Find the keyboard and then use this to change the sway configuration to desired language
if needed as shown under:

```
input "type:keyboard" {
	xkb_layout gb 
	xkb_options grp:alt_shift_tonggle
	}
```
See `man 7 xkeyboard-config` for options you can use with `xkb_layout`

## Configure application launcher 

For now I use [wofi](https://hg.sr.ht/~scoopta/wofi).
For installation see link above. In addition you need to install _Mercurial_ with
`sudo apt install mercurial`

Configuration is simple for now just add `bindsym $mod+d exec wofi --show=run`

## Configure WiFi 

Not yet configures, use Ethernet. 

I was misisng wifi card firmware that was identified by:
`sudo dmesg | grep iwlwifi`

This identified that firmware __iwlwifi-7260-17.ucode` was missing

Cloning the missing git repository as indicated in `dmesg` and copying the __ucode__
file in `/lib/firmware/` directory. Reboring showed that wifi card has been identified

Scan for available WiFi neworks with `nmcli dev wifi list`

Scan with `sudo iwlist scan` 


Manually change fine in `/etc/network/interfaces` to add 


# Configure terminal 

_foot_ is the default terminal 


# Battery life info

This can be found in `/sys/class/power_supply/BAT0/capacity`
