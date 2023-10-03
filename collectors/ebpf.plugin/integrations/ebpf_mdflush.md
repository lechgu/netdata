<!--startmeta
custom_edit_url: "https://github.com/netdata/netdata/edit/master/collectors/ebpf.plugin/integrations/ebpf_mdflush.md"
meta_yaml: "https://github.com/netdata/netdata/edit/master/collectors/ebpf.plugin/metadata.yaml"
sidebar_label: "eBPF MDflush"
learn_status: "Published"
learn_rel_path: "Data Collection/eBPF"
message: "DO NOT EDIT THIS FILE DIRECTLY, IT IS GENERATED BY THE COLLECTOR'S metadata.yaml FILE"
endmeta-->

# eBPF MDflush

Plugin: ebpf.plugin
Module: mdflush

<img src="https://img.shields.io/badge/maintained%20by-Netdata-%2300ab44" />

## Overview

Monitor when flush events happen between disks.

Attach tracing (kprobe, trampoline) to internal kernel functions according options used to compile kernel.

This collector is only supported on the following platforms:

- Linux

This collector supports collecting metrics from multiple instances of this integration, including remote instances.

The plugin needs setuid because it loads data inside kernel. Netada sets necessary permission during installation time.

### Default Behavior

#### Auto-Detection

The plugin checks kernel compilation flags (CONFIG_KPROBES, CONFIG_BPF, CONFIG_BPF_SYSCALL, CONFIG_BPF_JIT) and presence of BTF files to decide which eBPF program will be attached.

#### Limits

The default configuration for this integration does not impose any limits on data collection.

#### Performance Impact

This thread will add overhead every time that `md_flush_request` is called. The estimated additional period of time is between 90-200ms per call on kernels that do not have BTF technology.


## Metrics

Metrics grouped by *scope*.

The scope defines the instance that the metric belongs to. An instance is uniquely identified by a set of labels.



### Per eBPF MDflush instance

Number of times md_flush_request was called since last time.

This scope has no labels.

Metrics:

| Metric | Dimensions | Unit |
|:------|:----------|:----|
| mdstat.mdstat_flush | disk | flushes |



## Alerts

There are no alerts configured by default for this integration.


## Setup

### Prerequisites

#### Compile kernel

Check if your kernel was compiled with necessary options (CONFIG_KPROBES, CONFIG_BPF, CONFIG_BPF_SYSCALL, CONFIG_BPF_JIT) in `/proc/config.gz` or inside /boot/config file. Some cited names can be different accoring preferences of Linux distributions.
When you do not have options set, it is necessary to get the kernel source code from https://kernel.org or a kernel package from your distribution, this last is preferred. The kernel compilation has a well definedd pattern, but distributions can deliver their configuration files
with different names.

Now follow steps:
1. Copy the configuration file to /usr/src/linux/.config.
2. Select the necessary options: make oldconfig
3. Compile your kernel image: make bzImage
4. Compile your modules: make modules
5. Copy your new kernel image for boot loader directory
6. Install the new modules: make modules_install
7. Generate an initial ramdisk image (`initrd`) if it is necessary.
8. Update your boot loader



### Configuration

#### File

The configuration file name for this integration is `ebpf.d/mdflush.conf`.


You can edit the configuration file using the `edit-config` script from the
Netdata [config directory](https://github.com/netdata/netdata/blob/master/docs/configure/nodes.md#the-netdata-config-directory).

```bash
cd /etc/netdata 2>/dev/null || cd /opt/netdata/etc/netdata
sudo ./edit-config ebpf.d/mdflush.conf
```
#### Options

All options are defined inside section `[global]`.


<details><summary>Config options</summary>

| Name | Description | Default | Required |
|:----|:-----------|:-------|:--------:|
| update every | Data collection frequency. |  | False |
| ebpf load mode | Define whether plugin will monitor the call (`entry`) for the functions or it will also monitor the return (`return`). |  | False |
| lifetime | Set default lifetime for thread when enabled by cloud. |  | False |

</details>

#### Examples
There are no configuration examples.

