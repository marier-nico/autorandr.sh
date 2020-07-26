# autorandr.sh
A really really simple script to reliably manage monitors with profiles

## Introduction

This is a super simple script to manage monitor configurations easily and
reliably. Its current features were taken from
[phillipberndt/autorandr](https://github.com/phillipberndt/autorandr),
but this script is different in that it can more reliably save monitor
configurations as profiles. It does so by not actually trying to save
a configuration on its own. Instead, `arandr` is used to save configurations.

Another difference is that it was written in bash for simplicity. Also,
the script makes no attempt to respond to monitor hot-plug events at all,
so it needs to be manually called when monitors are added or removed.
The rest is fairly similar to the original project's way of doing things.

## Installation

Installation is super easy, just copy this script in your path or wherever
you want to execute it.

A fun use-case is to use this with highly customizable window-managers like
i3, bspwm, etc. You can add this repository as a git submodule to your dotfiles
under a `bin/` directory or something similar. Then just add the submodule's
directory to your path and use `autorandr.sh` in your window-manager's config
to automatically configure monitors.

### Dependencies

The only dependencies are `xrandr` and `arandr`, which are very commonly packaged
in distributions.

**Arch Linux**
```
# pacman -S xorg-xrandr arandr
```

**Ubuntu**
```
# apt install x11-xserver-utils arandr
```

## Usage

There are very few commands for this script because it only implements a
small amount of features, but they are the most important.

As is mentioned in the introduction, this script should be added to your
path if you need it to be executed as is shown below.

**Options**
```
$ autorandr.sh [--save, --load, --change, --detected]
```

- `--save profile_name`

This option saves the current monitor configuration to a profile called
`profile_name` by first invoking `arandr` to save the configuration and
then getting all the `EDID`s to fingerprint the chosen configuration.

- `--load profile_name`

This option looks for a profile with the provided name and runs the script
that was created by `arandr` after the `--save` command if it exists. If
a profile that does not exist is passed to `--load`, the script exits with
an error.

- `--change`

This option gets a fingerprint for the current display configuration and
tries to match it to a fingerprint in any saved profile. If there's a match,
the script created by `arandr` with the `--save` is run. With no match,
the command `xrandr --auto` is called, which uses all connected monitors.

- `--detected`

This option will do the same thing as `--change`, but it won't change the
monitor configuration. Instead, it just outputs the name of the profile that
was found.

### Use-Cases

This can be used as a stand-alone script to manage displays, but it's well
suited for use with a bare-bones window-manager like bspwm. To use it with
a window-manager, a good option is to call the script in the configuration
itself, or a pre-hook, if one is available. First, call `autorandr.sh --change`
to update the monitor configuration and then use `autorandr.sh --detected`
to get the name of the chosen profile. That way, it's possible to make
changes to the window-manager's configuration depending on the selected
profile. For example, it's possible to configure bspwm desktops depending
on the selected profile.

An example of this use-case is my own dotfiles [marier-nico/dotfiles](https://github.com/marier-nico/dotfiles).