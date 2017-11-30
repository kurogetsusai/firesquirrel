# Firesquirrel

Firesquirrel is a version / profile manager for Firefox (Linux only). You can
easily download a specific version of Firefox and, extract it and create
profiles using it. You can use multiple Firefox profiles using many Firefox
versions at the same time. They are run in a semi-portable mode, the binaries
and their profiles are isolated, but $HOME is not, so it still uses your real
~/.cache, ~/.config, ~/.local etc to allow it to work properly with iBus, GVFS
and other tools.

When you run it, it creates a `data` folder in the same folder, the data
structure looks like this:

- data
	- binaries
		- (extracted Firefox archives, every used version in its own folder)
	- packages
		- (.tar.bz2 files downloaded from Mozilla's servers)
	- profiles
		- (folders with all your Firefox profiles)
	- tmp
		- (temporary files used when downloading / extracting data)

It is NOT safe to run multiple downloads or creating multiple profiles at the
same time, you may encounter conflicts in the `tmp` directory (too lazy to fix
it now). But it is safe to run multiple profiles, even using multiple Firefox
versions at the same time.
