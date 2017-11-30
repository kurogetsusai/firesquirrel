# Firesquirrel

## Description

Firesquirrel is a version / profile manager for Firefox (Linux only). It can
easily download a specific version of Firefox, extract it and create profiles
using it. You can use multiple Firefox profiles using many Firefox versions at
the same time. They run in a semi-portable mode, the binaries and their profiles
are isolated, but $HOME is not, so it still uses your real `~/.cache`,
`~/.config`, `~/.local` etc to allow it to work properly with iBus, GVFS and
other tools.

## Data structure

When you run it, it creates a `data` folder in the current directory, the data
structure looks like this:

- data
	- binaries
		- (folders with Firefox binaries extracted from the tarballs)
	- mods
		- (mod scripts)
	- packages
		- (tarballs downloaded from Mozilla's servers)
	- profiles
		- (folders with all your Firefox profiles)
	- tmp
		- (temporary files used when downloading / extracting data)

It is NOT safe to run multiple downloads or creating multiple profiles at the
same time, you may encounter conflicts in the `tmp` directory (too lazy to fix
it now). But it is safe to run multiple profiles, even using multiple Firefox
versions at the same time.

## Mods

It is possible to make Firesquirrel run your own scripts everytime a specific
action takes place, so you can customize your profiles, eg. inject configuration
via the `user.js` file or the `chrome` directory. Mod scripts have access to
Firesquirrel's DIR_* variables.

### Supported mod actions

- postExtractBinary - runs after extracting a Firefox tarball, parameters:
	- version of the extracted Firefox binary
	- file nam of the extracted package
- postNewProfile - runs after creating a new profile, parameters:
	- version of the used Firefox binary
	- profile name

### Examples

#### Remove the `followonsearch@mozilla.com.xpi` built-in add-on

`data/mods/postExtractBinary.sh`:

```sh
#!/bin/bash
VERSION=$1
PACKAGE_NAME=$2

cd "$DIR_BINARIES/$VERSION"

echo "Removing followonsearch@mozilla.com.xpi..."
rm "browser/features/followonsearch@mozilla.com.xpi"
```

#### Inject a `user.js` file and a `chrome` directory

`data/mods/postNewProfile.sh`:

```sh
#!/bin/bash
VERSION=$1
PROFILE_NAME=$2

cd "$DIR_PROFILES/$PROFILE_NAME"

echo "Linking user.js and the chrome directory..."
ln -s "$HOME/.mozilla/custom/chrome" chrome
ln -s "$HOME/.mozilla/custom/user.js" user.js
```
