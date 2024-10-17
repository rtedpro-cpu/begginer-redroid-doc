English

# Table of contents
- [Overview](#overview)
- [Getting Started](#getting-started)
- [Configuration](#configuration)
- [Native Bridge Support](#native-bridge-support)
- [GMS Support](#gms-support)
- [WebRTC Streaming](#webrtc-streaming)
- [Troubleshooting](#troubleshooting)

## Overview
*redroid* (*Re*mote an*Droid*) is a GPU accelerated AIC (Android In Cloud) solution. You can boot many
instances in Linux host (`Docker`, `podman`, `k8s` etc.). *redroid* supports both `arm64` and `amd64` architectures. 
*redroid* is suitable for Cloud Gaming, Virtualise Phones, Automation Test and more.

![Screenshot of redroid 11](./assets/redroid11.png)

Currently supported:
- Android 15 (`redroid/redroid:15.0.0-latest`)
- Android 15 64bit only (`redroid/redroid:15.0.0_64only-latest`)
- Android 14 (`redroid/redroid:14.0.0-latest`)
- Android 14 64bit only (`redroid/redroid:14.0.0_64only-latest`)
- Android 13 (`redroid/redroid:13.0.0-latest`)
- Android 13 64bit only (`redroid/redroid:13.0.0_64only-latest`)
- Android 12 (`redroid/redroid:12.0.0-latest`)
- Android 12 64bit only (`redroid/redroid:12.0.0_64only-latest`)
- Android 11 (`redroid/redroid:11.0.0-latest`)
- Android 10 (`redroid/redroid:10.0.0-latest`)
- Android 9 (`redroid/redroid:9.0.0-latest`)
- Android 8.1 (`redroid/redroid:8.1.0-latest`)


## Getting Started
*redroid* should be able to run on any linux distribution (with some kernel features enabled).

Quick start on *Ubuntu 20.04* deploy of **Redroid**

```bash
apt install linux-modules-extra-`uname -r`
modprobe binder_linux devices="binder,hwbinder,vndbinder"
modprobe ashmem_linux
## running redroid
docker run -itd --rm --privileged \
    --pull always \
    -v ~/data:/data \
    -p 5555:5555 \
    redroid/redroid:13.0.0-latest
## can limit storage if needed
```

## Configuration

```
## running redroid with custom settings (custom display for example)
docker run -itd --rm --privileged \
    --pull always \
    -v ~/data:/data \
    -p 5555:5555 \
    redroid/redroid:12.0.0_64only-latest \
    androidboot.redroid_width=1080 \
    androidboot.redroid_height=1920 \
    androidboot.redroid_dpi=480 \
```

| Param | Description | Default |
| --- | --- | --- |
| `androidboot.redroid_width` | display width | 720 |
| `androidboot.redroid_height` | display height | 1280 |
| `androidboot.redroid_fps` | display FPS | 30(GPU enabled)<br> 15 (GPU not enabled)|
| `androidboot.redroid_dpi` | display DPI | 320 |
| `androidboot.use_memfd` | use `memfd` to replace deprecated `ashmem`<br>plan to enable by default | false |
| `androidboot.use_redroid_overlayfs` | use `overlayfs` to share `data` partition<br>`/data-base`: shared `data` partition<br>`/data-diff`: private data | 0 |
| `androidboot.redroid_net_ndns` | number of DNS server, `8.8.8.8` will be used if no DNS server specified | 0 |
| `androidboot.redroid_net_dns<1..N>` | DNS | |
| `androidboot.redroid_net_proxy_type` | Proxy type; choose from: `static`, `pac`, `none`, `unassigned` | |
| `androidboot.redroid_net_proxy_host` | | |
| `androidboot.redroid_net_proxy_port` | | 3128 |
| `androidboot.redroid_net_proxy_exclude_list` | comma seperated list | |
| `androidboot.redroid_net_proxy_pac` | | |
| `androidboot.redroid_gpu_mode` | choose from: `auto`, `host`, `guest`;<br>`guest`: use software rendering;<br>`host`: use GPU accelerated rendering;<br>`auto`: auto detect | `guest` |
| `androidboot.redroid_gpu_node` | | auto-detect |
| `ro.xxx`| **DEBUG** purpose, allow override `ro.xxx` prop; For example, set `ro.secure=0`, then root adb shell provided by default | |


## Native Bridge Support
It's possible to run `arm` Apps in `x86` *redroid* instance via `libhoudini`, `libndk_translation` or `QEMU translator`.

Check [@zhouziyang/libndk_translation](https://github.com/zhouziyang/libndk_translation) for prebuilt `libndk_translation`.
Published `redroid` images already got `libndk_translation` included.

## Google Services Support

It's possible to add Google Services support in *redroid* via [Redroid Script](https://github.com/abing7k/redroid-script).


## Web Streaming
```bash
docker run --rm -itd --privileged --name web -p 8000:8000/tcp emptysuns/scrcpy-web:v0.1
docker exec -it web adb connect 172.17.0.1:5555
# Change 5555 to the port of container
```

## Troubleshooting
- Checkout original redroid repo
