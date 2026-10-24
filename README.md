# Mesh Town packages for OpenWrt

Signed package feed of [Mesh Town](https://github.com/mesh-town/meshtown) for OpenWrt. CI builds it from the releases of
`github.com/mesh-town/meshtown`; the feed holds only the latest version.

## Install

On the router, as root:

    wget -O- https://mesh-town.github.io/meshtown-openwrt/install.sh | sh

The script reads the router architecture, adds the feed and its key, and installs the package `meshtown`.

Architectures: `aarch64_cortex-a53`, `aarch64_cortex-a72`, `aarch64_cortex-a76`, `aarch64_generic`, `x86_64`, `arm_cortex-a5_vfpv4`, `arm_cortex-a7`, `arm_cortex-a7_neon-vfpv4`, `arm_cortex-a7_vfpv4`, `arm_cortex-a8_vfpv3`, `arm_cortex-a9`, `arm_cortex-a9_neon`, `arm_cortex-a9_vfpv3-d16`, `arm_cortex-a15_neon-vfpv4`.

The commands below differ by OpenWrt version: 24.10 uses `opkg`, 25.12 uses `apk`. Check yours with
`grep DISTRIB_RELEASE /etc/openwrt_release` and follow only the matching section.

## OpenWrt 24.10 (opkg)

Manual setup instead of the script (`ARCH` is the value of `DISTRIB_ARCH` in `/etc/openwrt_release`):

    wget -O /tmp/meshtown-usign.pub https://mesh-town.github.io/meshtown-openwrt/meshtown-usign.pub
    opkg-key add /tmp/meshtown-usign.pub
    echo "src/gz meshtown https://mesh-town.github.io/meshtown-openwrt/opkg/$ARCH" >> /etc/opkg/customfeeds.conf
    opkg update && opkg install meshtown

Update:

    opkg update && opkg upgrade meshtown

Remove:

    opkg remove meshtown

## OpenWrt 25.12 (apk)

Manual setup instead of the script (`ARCH` is the value of `DISTRIB_ARCH` in `/etc/openwrt_release`):

    wget -O /etc/apk/keys/meshtown.pem https://mesh-town.github.io/meshtown-openwrt/meshtown-apk.pem
    echo "https://mesh-town.github.io/meshtown-openwrt/apk/$ARCH/packages.adb" >> /etc/apk/repositories.d/customfeeds.list
    apk update && apk add meshtown

Update:

    apk update && apk upgrade meshtown

Remove:

    apk del meshtown

After removal the node state in `/etc/meshtown/` stays on the router.

## Keys

The public keys are `meshtown-usign.pub` and `meshtown-apk.pem` in this feed; the same files are kept in
[platforms/linux/build/openwrt/keys](https://github.com/mesh-town/meshtown/tree/main/platforms/linux/build/openwrt/keys) of the code repository, compare them before trusting the feed.

## More

Details and the fully manual install: [platforms/linux/build/openwrt/README.md](https://github.com/mesh-town/meshtown/blob/main/platforms/linux/build/openwrt/README.md).
