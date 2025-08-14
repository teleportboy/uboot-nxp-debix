[English](./README.md) | [简体中文](./README_CN.md)

### System SDK Download

- Ubuntu 22.04 :
  https://github.com/nxp-imx/meta-nxp-desktop/tree/lf-6.1.22-2.0.0-mickledore

- Yocto-Linux 6.1.22_2.2.0

  ```shell
  repo init -u https://github.com/nxp-imx/imx-manifest -b imx-linux-mickledore -m imx-6.1.22-2.0.0.xml
  ```

  

### Modify sources/meta-imx/meta-bsp/recipes-bsp/u-boot/u-boot-imx-common_2023.04.inc

```shell
UBOOT_SRC ?= "git://github.com/debix-tech/uboot-nxp-debix;protocol=https"
SRCBRANCH = "lf_v2023.04-debix_model_ab_4gddr"
SRCREV = " ... commit id ... "
```

`SRCREV` can be obtained through the commit on git hub or through the `git log` command:

```shell
ljm@polyhex:~/workstation/Github/uboot-nxp-debix$ git checkout lf_v2023.04_4GBDDR
Branch 'lf_v2023.04_4GBDDR' set up to track remote branch 'lf_v2023.04_4GBDDR' from 'origin'.
Switched to a new branch 'lf_v2023.04_4GBDDR'
ljm@polyhex:~/workstation/Github/uboot-nxp-debix$ git log
commit 08539a36dfd16e301f1a7cd1abd1d8d567ebf046 (HEAD -> lf_v2023.04_4GBDDR, origin/lf_v2023.04_4GBDDR)
Author: John_gao <9278978@qq.com>
Date:   Mon Aug 12 03:27:14 2024 +0000

```

`08539a36dfd16e301f1a7cd1abd1d8d567ebf046` is the commit id

### Build uboot

```shell
DISTRO=imx-desktop-xwayland MACHINE=imx8mpevk source imx-setup-desktop.sh -b debix-desktop
bitbake -c compile -f -v u-boot-imx
bitbake -c deploy -f -v u-boot-imx
bitbake -c compile -f -v imx-boot
bitbake -c deploy -f -v imx-boot
```



uboot bin file: `debix-desktop/tmp/deploy/images/imx8mpevk/imx-boot-imx8mpevk-sd.bin-flash_evk`

### Use ubuntu `dd `command write to device

```shell
sudo dd if=imx-boot-imx8mpevk-sd.bin-flash_evk of=/dev/sdx bs=1k seek=32 conv=fsync
```