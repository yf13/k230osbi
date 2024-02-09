
# Introduction

This is an extract from K230 SDK used for check kernel mode NuttX image, the
original README.md is in README.original.md.

The extract can be used together with DTB created from the anmv230.dts file to
build runnables for either little or big core.

The goal of this extract is to help running NuttX kernel build with unchanged 
K230 U-Boot so that to reduce fragmentations. This extact places the cores in 
RiscV standard compliant mode, while Linux/RTT in K230 SDK mainly operates in
T-Head XuanTie mode.

# Usage

Clone this repo to local file system and then run the following commands to wrap
the kernel build nuttx.bin and create final binary to run on K230 device:

```shell
$ git clone https://github.com/yf13/k230osbi
$ mkdir -p build
$ make CROSS_COMPILE=riscv64-unknown-elf- PLATFORM=generic FW_PIC=n FW_PAYLOAD_PATH=$_elf O=$(pwd)/build K230_LITTLE_CORE=1 -C k230osbi -j
$ ls -l build/platform/generic/firmware/fw_payload.bin
```
We can ignore the warnings with canmv230.dts for now. The `fw_payload.bin` can
be loaded by K230 U-Boot to little core like below:

```shell
k230# usb start
k230# ping $serverip
k230# tftp 8000000 fw_payload.bin
k230# go 8000000
```
After seeing the `nsh> ` prompt, type help to see available commands. A few
apps are also available on the ROMFS of NuttX kernel, use `ls -l /system/bin` 
to see them.

I hope you enjoy NuttX, the RTOS that scales.
