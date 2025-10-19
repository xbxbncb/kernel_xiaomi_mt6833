# 使用教程
## 1. 自行 git 本项目编译
### 1. 安装编译所需工具链
推荐使用 debian 12 或 ubuntu 18.04 进行编译
```
sudo apt update && sudo apt install -y build-essential clang lld \
libssl-dev libelf-dev flex bison bc ccache curl git git-lfs gnupg \
gperf imagemagick liblz4-tool libncurses5 libncurses5-dev libsdl1.2-dev \
libxml2 libxml2-utils lzop pngcrush rsync schedtool \
squashfs-tools xsltproc zip zlib1g-dev gcc-aarch64-linux-gnu
```
### 2. 开始编译
配置 ccache（二次编译时加速）
```
#开启 ccache
export USE_CCACHE=1

#配置 ccache 缓存大小（根据自己实际情况调整）
ccache -M 20G
```
创建 .config 配置文件
```
make -j$(nproc --all) O=out \
          CC="ccache clang" \
          LD=ld.lld \
          CLANG_TRIPLE=aarch64-linux-gnu- \
          CROSS_COMPILE=aarch64-linux-gnu- \
          CROSS_COMPILE_COMPAT=aarch64-linux-gnueabi- \
          CROSS_COMPILE_ARM32=arm-linux-gnueabi- \
          LLVM_IAS=1 \
          KCFLAGS="--target=aarch64-linux-gnu \
                   -Wno-unused-but-set-variable \
                   -Wno-implicit-function-declaration \
                   -Wno-unused-variable -Wno-unused-function -Wno-unused-label" \
          evergo_defconfig
```
开始编译（注意：默认编译KernelSU Next和SUSFS，如不需要请手动修改配置文件）
```
make -j$(nproc --all) O=out \
          CC="ccache clang" \
          LD=ld.lld \
          CLANG_TRIPLE=aarch64-linux-gnu- \
          CROSS_COMPILE=aarch64-linux-gnu- \
          CROSS_COMPILE_COMPAT=aarch64-linux-gnueabi- \
          CROSS_COMPILE_ARM32=arm-linux-gnueabi- \
          LLVM_IAS=1 \
          KCFLAGS="--target=aarch64-linux-gnu \
                   -Wno-unused-but-set-variable \
                   -Wno-implicit-function-declaration \
                   -Wno-unused-variable -Wno-unused-function -Wno-unused-label"
```
若一切顺利，编译产物会生成于out/arch/arm64/boot/Image.gz-dtb
使用AnyKernel3打包后进行刷入即可
## 2.使用 Github Actions 编译
即将到来

Linux kernel
============

This file was moved to Documentation/admin-guide/README.rst

Please notice that there are several guides for kernel developers and users.
These guides can be rendered in a number of formats, like HTML and PDF.

In order to build the documentation, use ``make htmldocs`` or
``make pdfdocs``.

There are various text files in the Documentation/ subdirectory,
several of them using the Restructured Text markup notation.
See Documentation/00-INDEX for a list of what is contained in each file.

Please read the Documentation/process/changes.rst file, as it contains the
requirements for building and running the kernel, and information about
the problems which may result by upgrading your kernel.
