# Handheld Hardware Monitor


Version **0.0.6** provides compiled Display packages for the supported R36 and TrimUI installations. Python application roles, including the installer, are compiled with Nuitka; first-party JavaScript is minified and mangled. This public repository contains release packages, installation instructions and version/license information.

Karl’s software is free for noncommercial use. Commercial use, including internal company use, requires Karl’s prior written permission. See [LICENSE.md](LICENSE.md) and [third-party notices](THIRD-PARTY.md). Previously granted GPL rights remain effective.

Before installing, confirm the release is marked stable on its page. Pre-release packages are for validation.

[Download both packages](https://github.com/KarlSideProjects/mqtt-handheld-hardware-monitor-releases/releases/tag/v0.0.6). [Version metadata](version.json) identifies the compiled source, archive hashes and installer manifest hashes. Installation uses the complete compiled package from this public release.

## Compatibility and preparation

* **R36:** the tested Debian 13 ARM64 installation with glibc 2.41; run as `ark`. Exit the monitor with **Select+B** before installing. This artifact does not support older glibc firmware.
* **TrimUI Smart Pro S:** the tested ARM64 Buildroot system with glibc 2.33; run as `root`. The existing compatible Chromium/SDL runtime and CJK font under `/mnt/UDISK/hwmonitor/runtime` are prerequisites and remain installed separately.

Use a terminal with curl, gzip, tar and sha256sum. If curl is unavailable on the handheld, download and verify the matching archive on your computer, transfer it to the device, then extract and run the same bundled installer. Existing MQTT and language settings are retained. Installation restarts the display; choose a time when that interruption is acceptable.

## R36 installation

```sh
curl --fail --location https://github.com/KarlSideProjects/mqtt-handheld-hardware-monitor-releases/releases/download/v0.0.6/display-r36-arm64-0.0.6.tar.gz --output display-r36-arm64-0.0.6.tar.gz
printf '%s  display-r36-arm64-0.0.6.tar.gz\n' 'b203adfc8772a6dad4132821816d321c5a4af7ab8811596725a7f90d044a0c50' | sha256sum --check -
bundle=$(mktemp -d)
gzip -dc display-r36-arm64-0.0.6.tar.gz | tar -xf - -C "$bundle"
cd "$bundle"
sh ./install-r36.sh --manifest-sha256 d605942965ed91e9a503a8a8073742cc4cb2eb4c6f4a2835c09b284065c59731
```

## TrimUI installation

```sh
curl --fail --location https://github.com/KarlSideProjects/mqtt-handheld-hardware-monitor-releases/releases/download/v0.0.6/display-trimui-arm64-0.0.6.tar.gz --output display-trimui-arm64-0.0.6.tar.gz
printf '%s  display-trimui-arm64-0.0.6.tar.gz\n' 'e8949200f8b5258c7f93351a6cd323ea0fc1418196511933a674740d936f74b8' | sha256sum --check -
bundle=$(mktemp -d)
gzip -dc display-trimui-arm64-0.0.6.tar.gz | tar -xf - -C "$bundle"
cd "$bundle"
sh ./install-trimui.sh --manifest-sha256 0ef39e1cf5e2451e990675ba3dac8e5172899ae2615ee39f30076c3fd271bd05
```

For a new MQTT configuration, after verifying and extracting the archive, run `./app/mqtt_setup.bin --output /absolute/private/path/new-config.json` in an interactive terminal. The output file must not exist. Add `--config /absolute/private/path/new-config.json` to the installer command. Keep this file private and outside this repository.

The installer verifies its supplied manifest and every registered file before running the compiled installer. It preserves a private backup and reports incomplete recovery rather than silently claiming success. If it reports an unknown display owner or an unsupported running-process stop, exit the application normally before retrying. Dependency and firmware changes are outside application rollback.
