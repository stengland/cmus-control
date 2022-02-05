# MPD Control

Control [mpd](https://musicpd.org/) with Media Keys :rewind: :arrow_forward: :fast_forward: under [macOS](https://en.wikipedia.org/wiki/MacOS).

## Project Outlines

This is a simple fork of [CMUS Control](https://github.com/TheFox/cmus-control) by [Christian Mayer](https://github.com/TheFox) replacing the calls to `cmus-remote` with `mpc`

The original project outlines are described his blog post about [Open Source Software Collaboration](https://blog.fox21.at/2019/02/21/open-source-software-collaboration.html).

- The main purpose of this software is to provide support for mpd under macOS. MPD can be controlled by the Media Keys of your Apple Keyboard.
- The feature-set is restricted because this software already provides the features what it was made of. But still, feel free to request features.

## Requirements

- At least **macOS 10.8**.
- `cmake` to build it.
- Since MPD Control doesn't have the behavior of changing any foreign processes it's highly recommended to [deactivate the *Remote Control Daemon*](https://blog.fox21.at/2015/11/20/control-cmus-with-media-keys.html).
- [mpc](https://musicpd.org/clients/mpc/) installed. ;)

## Install

You can install MPD Control [manually](#manually-installation).

TODO: Allow instalation via via [Homebrew](https://brew.sh/)

### Manual installation

1. You need to install cmake: `brew install cmake`
2. Run `make install` to compile MPD Control Daemon* and install `mpdcontrold` under `/usr/local/bin` path.
	A [launchd.plist](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man5/launchd.plist.5.html) file named `at.fox21.mpdcontrold.plist` will be created under `~/Library/LaunchAgents` to start *MPD Control Daemon* automatically on login.

If you just want to compile *MPD Control Daemon* without installing run `make`. The binary will be created at `build/release/bin/mpdcontrold`.

#### Uninstall

Just run `make uninstall`. Doing so

- `mpdcontrold` will be unloaded via [`launchctl`](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/launchctl.1.html);
- `~/Library/LaunchAgents/at.fox21.mpdcontrold.plist` will be removed;
- `/usr/local/bin/mpdcontrold` will be removed.

#### Load/Unload

After a successful manual installation the `mpdcontrold` is loaded/started automatically with `launchctl`. You can unload the daemon manually:

```bash
$ make unload
```

Or load it manually:

```bash
$ make load
```

#### Re-build

After changing the source code you might want to re-build the binary and re-install it.

```bash
make unload
make -C build/release
make install
```
### Remote MPD Server

If your MPD server is not local you can configure the `MPD_HOST` in `~/Library/LaunchAgents/at.fox21.mpdcontrold.plist`

```xml
		<key>EnvironmentVariables</key>
		<dict>
			<key>PATH</key>
			<string>/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:/sbin</string>
			<key>MPD_HOST</key>
			<string>192.1.2.3</string>
		</dict>
```
