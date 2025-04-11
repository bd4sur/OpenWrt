# 适用于IPQ系列设备的OpenWrt

- 本仓库基于[LiBwrt/openwrt-6.x/tree/kernel-6.12](https://github.com/LiBwrt/openwrt-6.x/tree/kernel-6.12)。
- 本仓库仅供个人嵌入式技术学习之用，**请勿复刻本仓库**。
- 本仓库将不定期与上游同步，但是绝不会提交PR。

## 说明
> 以前IPQ系列想要使用OpenWrt系统，只能放弃部分NSS功能，或者使用相对老旧的内核。开源社区努力完善了这部分的支持。本库融合了[JiaY-shi](https://github.com/JiaY-shi/openwrt)、[qosmio](https://github.com/qosmio/openwrt-ipq)两位大佬支持NSS的代码，并使用了[immortalwrt](https://github.com/immortalwrt)的luci、packages插件支持，在此感谢大佬们的付出。

目前已实现的功能：

| Target  | NSS NAT | 2.4G WiFi <br />NSS Offload | 5G WiFi <br />NSS Offload |
| :-:     | :-:     | :-:       | :-:     |
| IPQ807X | ✅      | ✅       | ✅      |
| IPQ60XX | ✅      | ✅       | ✅      |
| IPQ50XX | ❌      | ❌       | ❌      |


## 本地编译

- **不要用 `root` 用户编译**。
- 默认静态IP`192.168.1.1`，密码`password`或空。

1. 建议使用 Ubuntu 20.04 LTS。

2. 安装编译依赖。

```bash
sudo apt update -y
sudo apt full-upgrade -y
sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libssl-dev libtool lrzsz \
mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python3 python3-pip libpython3-dev qemu-utils \
rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
```

3. 下载源代码，更新feeds并选择配置。

```bash
git clone https://github.com/bd4sur/OpenWrt.git
cd OpenWrt
./scripts/feeds update -a && ./scripts/feeds install -a
make menuconfig
```

4. 下载dl库并开始编译，建议首次编译用1个线程，防止因网络问题或依赖问题导致编译失败。

```bash
make download -j$(nproc)
make -j1 V=s
```

5. 二次编译，拉取最新代码和feeds，重新选择配置并开始编译。

```bash
cd OpenWrt
git fetch && git reset --hard origin/kernel-6.12
./scripts/feeds update -a && ./scripts/feeds install -a
make menuconfig
make V=s -j$(nproc)
```

6. 如果需要重新配置。

```bash
rm -rf .config
make menuconfig
make V=s -j$(nproc)
```

7. 编译完成后输出路径：bin/targets
