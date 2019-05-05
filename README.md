# polybar_i3-marks

Polybar module to display i3 marks.

## Configuration

Requires jq.

You can specify marks to ignore by adding them in the script.

Add the script to your $PATH, or list its full path in the `exec` field as shown below.

## Module

```ini
[module/marks]
type = custom/script
#format-prefix = "marks: "
format = <label>
exec = $HOME/scripts/i3_getmarks.sh
label = "%output%"
interval = 1
```
This will always show marks.

## Module using IPC

```ini
[module/marks]
type = custom/ipc
#format-prefix = "marks: "
format = <output>
hook-0 = echo ""
hook-1 = $HOME/scripts/i3_getmarks.sh
initial = 1
```

This will only show marks when told to using polybar-msg:

```
polybar-msg hook marks 1 # off (default)
polybar-msg hook marks 2 # on
```

I recommend adding these commands as bindings in your i3 config.

# i3 configuration

You could take advantage of the IPC version of the module and add modes specifically for marking/switching to windows, and only display marks when on these specific modes like so:

```ini
# define aliases
set $showmarks exec --no-startup-id polybar-msg hook marks 2
set $hidemarks exec --no-startup-id polybar-msg hook marks 1

# create mode to mark windows
bindsym $mod+m mode "mark", $showmarks
mode "mark" {
        bindsym $mod+1 mark --toggle M1, $hidemarks, mode "default"
        bindsym $mod+2 mark --toggle M2, $hidemarks, mode "default"
        bindsym $mod+3 mark --toggle M3, $hidemarks, mode "default"
        bindsym $mod+4 mark --toggle M4, $hidemarks, mode "default"
        bindsym $mod+5 mark --toggle M5, $hidemarks, mode "default"
        bindsym $mod+6 mark --toggle M6, $hidemarks, mode "default"
        bindsym $mod+7 mark --toggle M7, $hidemarks, mode "default"
        bindsym $mod+8 mark --toggle M8, $hidemarks, mode "default"
        bindsym $mod+9 mark --toggle M9, $hidemarks, mode "default"
        bindsym $mod+0 mark --toggle M0, $hidemarks, mode "default"

        bindsym Return $hidemarks, mode "default"
        bindsym Escape $hidemarks, mode "default"
}

# create mode to go to marked window
bindsym $mod+apostrophe mode "goto", $showmarks
mode "goto" {
        bindsym $mod+1 [con_mark="M1"] focus, $hidemarks, mode "default"
        bindsym $mod+2 [con_mark="M2"] focus, $hidemarks, mode "default"
        bindsym $mod+3 [con_mark="M3"] focus, $hidemarks, mode "default"
        bindsym $mod+4 [con_mark="M4"] focus, $hidemarks, mode "default"
        bindsym $mod+5 [con_mark="M5"] focus, $hidemarks, mode "default"
        bindsym $mod+6 [con_mark="M6"] focus, $hidemarks, mode "default"
        bindsym $mod+7 [con_mark="M7"] focus, $hidemarks, mode "default"
        bindsym $mod+8 [con_mark="M8"] focus, $hidemarks, mode "default"
        bindsym $mod+9 [con_mark="M9"] focus, $hidemarks, mode "default"
        bindsym $mod+0 [con_mark="M0"] focus, $hidemarks, mode "default"

        bindsym Return $hidemarks, mode "default"
        bindsym Escape $hidemarks, mode "default"
}
```
