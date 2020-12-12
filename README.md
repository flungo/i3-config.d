# i3-config.d

A simple script that creates a `config.d` directory which will contain the i3 sections of i3 configuration. This allows configurations to be split into smaller files that can be related to certain applications or sets of configuration. Beyond making the configurations easier to manage, this allows a lot of flexability and control when it comes to adding and sharing configuration snippets. Configuration snippets can be shared as files online and put into a single file within the `config.d` directory. They can also be synchronised between machines and tools such as [shared-config-init](https://github.com/flungo/shared-config-init) can be used to load the relevant configurations for the machine.

## How it works?

Running the `config-init` script will search the `config.d` subdirectory of the i3 configration folder (typically `~/.config/i3/config.d`) for files ending `.conf` and concattenate them together in lexilogical order into one configuration file stored in the standard i3 configuration location (typically `~/.config/i3/config`). The script can be run manually, or integrated as part of your `xinit` process (see [hooks](#hooks) to see how to integrate).

## Installation

To get the files onto your machine you will need to clone this repository onto your machine. You can install this anywhere on your machine and the script will run correctly. Make sure to backup your existing i3 configuration before installing and using as this script will overwirte the file when run.

### Single User Installation

For a single user installation, simply clone the repository into your home directory. If you would like to install this to your i3 config directory (`~/.config/i3/`) then you will need to delete or move your old directory before doing so. Git will only clone into empty or new directories. Typically the config directory only contains the `config` file which will need to be moved inside of the `config.d` folder anyway (otherwise it will be overriden).

```
cd ~/.config
mv i3 i3.bak
git clone https://github.com/flungo/i3-config.d.git i3
mkdir i3/config.d
mv i3.bak/config i3/config.d/00-base.conf
rmdir i3.bak # This will fail if you had any other files in your i3 folder.
```

To make the `config-init` script part of your path, you will need to symbolically link to it from some directory that you have added to your path. It is recomended that you name the link `i3-config-init`. This step is not required, but will allow you to run `i3-config-init` from anywhere. If, for example, you had a directory `~/.local/bin` which was part of your path and you had installed into your i3 config directory, then you could run:

```
ln -s ~/.config/i3/config-init ~/.local/bin/i3-config-init
```

### Global Installation

For a computer with multiple users who will all want access to the script, it can be installed globally. To do this, you will need to do the following as root or using `sudo`:

```
git clone https://github.com/flungo/i3-config.d.git /usr/local/share/i3-config.d
ln -s /usr/local/share/i3-config.d/config-init /usr/local/bin/i3-config-init
```

`i3-config-init` should then be available for all users to use.

## Hooks

In order to have the script run automatically before `i3` starts, it is recomended that you add the command to the `.xinitrc` file. If you have installed gloablly, then you can add a file containing `i3-config-init` to `/etc/X11/xinit/xinitrc.d/i3-config-init` (assuming you want this to be run automatically for all users). For an individual user, you will want to add `i3-config-init` before `exec i3` in your `~/.xinitrc`.

Another place that you can hook this script into is within your i3 configuration for the reload and restart commands. The simplest way to do this is to add the following to your `config.d` directory and comment out or remove the respective `bindsym` lines from your other files (to avoid conflicts):

```
bindsym $mod+Shift+c exec "~/.config/i3/config-init; i3-msg reload"
bindsym $mod+Shift+r exec "~/.config/i3/config-init; i3-msg restart"
```

## Example configurations

Example configurations can be found as part of the [flungo/i3-config.d-configs](https://github.com/flungo/i3-config.d-configs) repository. This contains the configuration used by the developer of this utility and can be used for inspriation for your own configurations.
