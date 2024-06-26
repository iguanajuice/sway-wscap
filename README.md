# sway-wscap

Screen share a workspace instead of your entire screen, great for privacy.

## Features:

* Works with multiple monitors
* Works with multiple applications capturing at the same time
* Choose between full output and per workspace capture
* Can run using `dash` instead of `bash` for a bit of extra performance

## Downsides:

First downside is small, but that is "workspace wrapping" when scrolling swaybar.

Second, if switching between workspaces, the capture may sometimes not resume for up to a second or two (I know, devastating).
However, the workspace switching itself is *always* responsive, so normal usage is not impeded.

Third, some actions may cause this script to malfunction, see below:

### Don'ts:
* Do not move the captured workspace to an other output
* Do not click the workspace numbers in swaybar (unless you want to temporarily show that workspace)
* Do not rapidly scroll swaybar, may sometimes cause the wrong workspace to be captured

## Installation:

Clone the repo:
```
git clone https://github.com/iguanajuice/sway-wscap
```

"Install" executables:
```
cd sway-wscap
sudo rsync libexec /usr/local/libexec
sudo chmod +x /usr/local/libexec/sway-wscap*
```

Edit `/etc/xdg/xdg-desktop-portal-wlr/config` to look something like this:
```
[screencast]
max_fps=30
chooser_type=simple
chooser_cmd=/usr/local/libexec/sway-wscap-picker
```

Edit your Sway configururation so that workspace switching is done using `sway-wscap-switcher`:

*Go from this:*
```
# Switch to workspace
bindsym $mod+grave workspace 0
bindsym $mod+1 workspace 1
bindsym $mod+2 workspace 2
bindsym $mod+3 workspace 3
bindsym $mod+4 workspace 4
...
```
*To something like this:*
```
# Switch to workspace
set $workspace exec /usr/local/libexec/sway-wscap-switcher
bindsym $mod+grave $workspace 0
bindsym $mod+1 $workspace 1
bindsym $mod+2 $workspace 2
bindsym $mod+3 $workspace 3
bindsym $mod+4 $workspace 4
...
```
*Mind the `$` in `$workspace`.*

The same trick should be done for your bar config. For swaybar, add these lines to its config:
```
bindsym button4 $workspace prev_on_output
bindsym button5 $workspace next_on_output
```
*User's of other bars, you should be smart enough to figure this out.*
