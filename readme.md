# Installing headers

Since the kernel interfaces may vary between versions, we need to have part of the kernel source, so the `dummylkm` can assert compatibility during build time.

## Checking installed

You can check the sources that are installed by listing directories at `/usr/src` by entering:

```
ls /usr/src

```

Multiple folder per version as well as multiple kernel versions may appear.

Look up on the presented list whether the current kernel version matches the current version starting with 'linux-headers-'.


## Installing

For learning purposes, lets assume that this dynamic kernel module will execute on the same version building it.

The below command ensures that those headers are going to be installed. On the current version `$(uname -r)` will do the trick.

```
sudo apt install git build-essential linux-headers-$(uname -r)
```


# Build

Run

```
make
```

Check the presence of the final file named: `dummylkm.ko`

```
ls
```


# Operation

Considering the binary file is built and ready, there are a few steps to cover the entire usage of it.


## Module description

The command below shows information about a linux kernel module.

```
modinfo dummylkm.ko
```


It should return somethinf like this:

```
filename:       /tmp/lkm/dummylkm.ko
description:    Introduction to LKM
version:        1.0
author:         EKUSSA
license:        GPL
srcversion:     228339E5397D77CBE372026
depends:        
retpoline:      Y
name:           dummylkm
vermagic:       5.4.0-72-generic SMP mod_unload modversions
```

Further information about this command:

```
man modinfo
```


## Loaded modules

The command below shows the status of loaded modules in the Linux Kernel

```
lsmod
```

A long list of loaded modules and its dependencies will show up categorized in four collumns: name, memory size, usage count and dependency modules, when available. A sample of output is:

```
Module                  Size  Used by
cmac                   16384  1
rfcomm                 81920  21
iptable_nat            16384  1
nf_nat                 40960  2 iptable_nat,xt_MASQUERADE
nf_conntrack          139264  4 xt_conntrack,nf_nat,nf_conntrack_netlink,xt_MASQUERADE
...
```


To seek the specific module, split the lines using `grep` command.

```
lsmod | grep dummylkm
```

If no module is loaded, there should be no output, otherwise the selected name will show.


Further information about this command:

```
man lsmod
```


## Insert

The command below inserts a module into the Linux Kernel

```
sudo insmod dummylkm.ko
```


After adding the module, it may be visible on when listing loaded modules `lsmod `.


Further information about this command:

```
man insmod
```

## Tracing

This module sample uses [printk](https://en.wikipedia.org/wiki/Printk) to output messages.
Those messages are manages by the kernel and can be gathered by `dmesg` command. It prints and controls kernel ring messages.

When running the command below, you may see the init method:

```
dmesg
```

Further information about this command:

```
man dmesg
```


### Tooling to digest content

It will output every message since the OS was started or was available in memory before truncated.


#### Head and tail

To fix the message quantity, you can either get its head or tail, so output only the first or last entries:

```
dmesg | tail
dmesg | head
```

#### Less

Another way it to use `less`, so you can navigate using arrows, page up and down, mouse scroll and so on.

```
dmesg | less
```

#### Content

Being able to navigate all the messages, lessen the effort to find the dummylkm outputs.

Once we started it, the line below should be there:

```
... Hello, world 2
```


## Remove
sudo rmmod dummylkm.ko

Further information about this command:

```
man rmmod
```

