# Handheld Hardware Monitor


Version **0.0.4** provides compiled Display packages for the supported R36 and TrimUI installations. Python application roles, including the installer, are compiled with Nuitka; first-party JavaScript is minified and mangled. This public repository contains release packages, installation instructions and version/license information.

Karl’s software is free for noncommercial use. Commercial use, including internal company use, requires Karl’s prior written permission. See [LICENSE.md](LICENSE.md) and [third-party notices](THIRD-PARTY.md). Previously granted GPL rights remain effective.

Before installing, confirm the release is marked stable on its page. Pre-release packages are for validation.

[Download both packages](https://github.com/KarlSideProjects/mqtt-handheld-hardware-monitor-releases/releases/tag/v0.0.4). [Version metadata](version.json) identifies the compiled source, archive hashes and installer manifest hashes. Installation uses the complete compiled package from this public release.

## Compatibility and preparation

* **R36:** the tested Debian 13 ARM64 installation with glibc 2.41; run as `ark`. Exit the monitor with **Select+B** before installing. This artifact does not support older glibc firmware.
* **TrimUI Smart Pro S:** the tested ARM64 Buildroot system with glibc 2.33; run as `root`. The existing compatible Chromium/SDL runtime and CJK font under `/mnt/UDISK/hwmonitor/runtime` are prerequisites and remain installed separately.

Use a terminal with curl, gzip, tar and sha256sum. If curl is unavailable on the handheld, download and verify the matching archive on your computer, transfer it to the device, then extract and run the same bundled installer. Existing MQTT and language settings are retained. Installation restarts the display; choose a time when that interruption is acceptable.

## R36 installation

```sh
curl --fail --location https://github.com/KarlSideProjects/mqtt-handheld-hardware-monitor-releases/releases/download/v0.0.4/display-r36-arm64-0.0.4.tar.gz --output display-r36-arm64-0.0.4.tar.gz
printf '%s  display-r36-arm64-0.0.4.tar.gz\n' '64cf726e7d1de111c2a7965f4fc2c51f362f574c56d46f1822c7e08861e68692' | sha256sum --check -
bundle=$(mktemp -d)
gzip -dc display-r36-arm64-0.0.4.tar.gz | tar -xf - -C "$bundle"
cd "$bundle"
sh ./install-r36.sh --manifest-sha256 ccfa9e3990cf338f8dcf96e147ad730e647156f7e7666a8fc49d734b77664350
```

## TrimUI installation

```sh
curl --fail --location https://github.com/KarlSideProjects/mqtt-handheld-hardware-monitor-releases/releases/download/v0.0.4/display-trimui-arm64-0.0.4.tar.gz --output display-trimui-arm64-0.0.4.tar.gz
printf '%s  display-trimui-arm64-0.0.4.tar.gz\n' '200ad99324fb97d40eb1c683fe81a286efaea3fcfa586d117f884a09e02ecd57' | sha256sum --check -
bundle=$(mktemp -d)
gzip -dc display-trimui-arm64-0.0.4.tar.gz | tar -xf - -C "$bundle"
cd "$bundle"
sh ./install-trimui.sh --manifest-sha256 ee9e820434d3c4636d8f2c833a11d12fd38ac7e6783d35f3682c164149523557
```

For a new MQTT configuration, after verifying and extracting the archive, run `./app/mqtt_setup.bin --output /absolute/private/path/new-config.json` in an interactive terminal. The output file must not exist. Add `--config /absolute/private/path/new-config.json` to the installer command. Keep this file private and outside this repository.

The installer verifies its supplied manifest and every registered file before running the compiled installer. It preserves a private backup and reports incomplete recovery rather than silently claiming success. If it reports an unknown display owner or an unsupported running-process stop, exit the application normally before retrying. Dependency and firmware changes are outside application rollback.
