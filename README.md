# Handheld Hardware Monitor


Version **0.0.3** provides compiled Display packages for the supported R36 and TrimUI installations. Python application roles, including the installer, are compiled with Nuitka; first-party JavaScript is minified and mangled. This public repository contains release packages, installation instructions and version/license information.

Karl’s software is free for noncommercial use. Commercial use, including internal company use, requires Karl’s prior written permission. See [LICENSE.md](LICENSE.md) and [third-party notices](THIRD-PARTY.md). Previously granted GPL rights remain effective.

Before installing, confirm the release is marked stable on its page. Pre-release packages are for validation.

[Download both packages](https://github.com/KarlSideProjects/mqtt-handheld-hardware-monitor-releases/releases/tag/v0.0.3). [Version metadata](version.json) identifies the compiled source, archive hashes and installer manifest hashes. Installation uses the complete compiled package from this public release.

## Compatibility and preparation

* **R36:** the tested Debian 13 ARM64 installation with glibc 2.41; run as `ark`. Exit the monitor with **Select+B** before installing. This artifact does not support older glibc firmware.
* **TrimUI Smart Pro S:** the tested ARM64 Buildroot system with glibc 2.33; run as `root`. The existing compatible Chromium/SDL runtime and CJK font under `/mnt/UDISK/hwmonitor/runtime` are prerequisites and remain installed separately.

Use a terminal with curl, gzip, tar and sha256sum. If curl is unavailable on the handheld, download and verify the matching archive on your computer, transfer it to the device, then extract and run the same bundled installer. Existing MQTT and language settings are retained. Installation restarts the display; choose a time when that interruption is acceptable.

## R36 installation

```sh
curl --fail --location https://github.com/KarlSideProjects/mqtt-handheld-hardware-monitor-releases/releases/download/v0.0.3/display-r36-arm64-0.0.3.tar.gz --output display-r36-arm64-0.0.3.tar.gz
printf '%s  display-r36-arm64-0.0.3.tar.gz\n' '16e311e81bf0e844e41b880da26592fb52ecf44653160f3b39fefd84953f6deb' | sha256sum --check -
bundle=$(mktemp -d)
gzip -dc display-r36-arm64-0.0.3.tar.gz | tar -xf - -C "$bundle"
cd "$bundle"
sh ./install-r36.sh --manifest-sha256 39906281b2af9aeceb960b5136bb30ebe66ce20b1d059d9d4f0a6cf2f1604e91
```

## TrimUI installation

```sh
curl --fail --location https://github.com/KarlSideProjects/mqtt-handheld-hardware-monitor-releases/releases/download/v0.0.3/display-trimui-arm64-0.0.3.tar.gz --output display-trimui-arm64-0.0.3.tar.gz
printf '%s  display-trimui-arm64-0.0.3.tar.gz\n' 'a2002fe24cee407e394841b03f4c58d94ca10193e8ae2c6fe4106601c6699ca0' | sha256sum --check -
bundle=$(mktemp -d)
gzip -dc display-trimui-arm64-0.0.3.tar.gz | tar -xf - -C "$bundle"
cd "$bundle"
sh ./install-trimui.sh --manifest-sha256 682d014f5ee4fb7e19ff2e046fc8db977133caa525696501b01655defdc68583
```

For a new MQTT configuration, after verifying and extracting the archive, run `./app/mqtt_setup.bin --output /absolute/private/path/new-config.json` in an interactive terminal. The output file must not exist. Add `--config /absolute/private/path/new-config.json` to the installer command. Keep this file private and outside this repository.

The installer verifies its supplied manifest and every registered file before running the compiled installer. It preserves a private backup and reports incomplete recovery rather than silently claiming success. If it reports an unknown display owner or an unsupported running-process stop, exit the application normally before retrying. Dependency and firmware changes are outside application rollback.
