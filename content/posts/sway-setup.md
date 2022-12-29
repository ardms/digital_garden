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

## Configure application lancher 

For now I use [wofi](https://hg.sr.ht/~scoopta/wofi).
For installation see link above. In addtion you need to install _Mercurial_ with
`sudo apt install mercurial`

Configuration is simple for now just add `bindsym $mod+d exec wofi --show=run`

## Configure wifi 

Not yet configures, use ethernet. 

scan with `sudo iwlist scan` 

manualy change fine in `/etc/network/interfaces` to add 


# Configure terminal 

_foot_ is the default terminal 


# Battery life info

This can be found in /sys/class/power_supply/BAT0/capacity
