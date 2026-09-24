# World

A small desktop IPv4 blocklist firewall for Linux Mint, inspired by PeerGuardian.

**Public preview:** the application has been used on Linux Mint and tested with
isolated firewall tests. Broad distribution compatibility and a clean-machine
installation/upgrade/reboot test matrix are not yet established.

## Features

- A globe tray indicator with manual ON/OFF control.
- A rate-limited blocked-activity viewer with blocklist descriptions.
- Temporary exceptions for an IPv4 address, lasting 1 minute to 24 hours.
- Controls to select, add, edit, and remove blocklist feeds.
- Daily list downloads, independent of whether blocking is on.

## Install or upgrade

Requires an amd64 Linux Mint environment with systemd, nftables, GTK3 and XApp.
Download the Debian installer from this repository's Releases page, open a
terminal in its folder, and run:

```sh
sudo apt-get -o DPkg::Lock::Timeout=300 install ./world_1.0.9~preview1_amd64.deb
```

Log out and back in to activate desktop permissions and start the tray.
The first list download needs Internet access. Verify the setup with:

```sh
sudo /usr/local/sbin/world-verify
```

If the installer cannot identify the desktop user, run
`sudo /usr/local/sbin/world-setup "$USER"`, then log out and in.
Existing packaged installations can upgrade without a purge.

## How blocking works

World starts **OFF after reboot**. Turn it on from the globe menu.
Closing the tray does not turn blocking off. World edits only its own nftables
table; other firewall rules still apply.

World filters **IPv4 input and output only**. It does not filter IPv6 or
forwarded traffic, and is not a VPN or anonymity service. Lists can block
legitimate services; their labels are not proof that an address is malicious.

After saving and rebuilding lists, turn World OFF then ON to load them.
This clears temporary exceptions, as does rebooting. Exceptions otherwise
expire automatically, even with the settings window closed.

The activity viewer is rate limited and is not a complete packet log.
Descriptions come from downloaded lists, not verified ownership lookups.

## Build and test

Download and extract the source archive from Releases, then open a terminal
in the extracted `world` directory. The archive includes all source files,
tests, packaging scripts, and documentation.

```sh
python3 -m unittest discover -s tests -v
sh build.sh
```

The build writes the installer and SHA256SUMS into the parent directory.
Two kernel tests skip unless WORLD_RUN_NAMESPACE_TESTS=1 is set and isolated
network namespaces are available. They never operate on the host firewall.

To recreate the original icons:

```sh
python3 tools/generate-icons.py
```

## Preview packaging limitations

This initial preview retains the existing /usr/local installation layout for
compatibility. It is not a Debian/Ubuntu archive submission. Downloaded list
generations are retained as backups and consume disk space over time.
See the detailed documentation and settings guide under
`root/usr/share/doc/world/` in the source archive.

## License and credits

GPL-3.0-or-later; see [LICENSE](LICENSE) and [NOTICE](NOTICE).
Developed with AI assistance for nenelson1981-ctrl. Inspired by
[PeerGuardian Linux](https://sourceforge.net/projects/peerguardian/).
Third-party blocklists and system dependencies retain their own terms.

## Feedback

Report reproducible problems through this repository's Issues tab. Include
your Linux Mint version, World version, and steps to reproduce. Remove personal
addresses and private feed URLs from logs before posting them.
