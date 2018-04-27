# linux-gpio-nct5104d
NCT5104D GPIO Linux Driver

## Preconditions (Compile) ##
you should have the following packages installed:
* linux-headers-$(uname -r)
* build-essential
* make
* git (you can also download the repo as zip)

`apt-get install -y linux-headers-$(uname -r) build-essential make git`

`git clone <this repo url>`

## Compilation ##
run `make` in the driver folder on the target machine
(otherwise your driver gets compiled for a different kernel version)

You can also provide the custom kernel directory (to compile for different
kernel than installed on host. E.g.:

```sh
make KDIR=../linux-4.9.0
```

> Kernel has to be prepared first to build the external modules:
> `make modules_prepare`

## Debian dkms package ##

### Build the package ###
Install dkms, debhelper and git package on the build machine:

`apt-get install -y dkms debhelper`

Clone this repository:

`git clone <this repo url>`

Run `dpkg-buildpackage` in the driver directory.

### Install the deb package ###
Install dkms on the target machine:

`apt-get install -y dkms`

Copy the built deb file (e.g. `gpio-nct5104d-dkms_0.20171115-0_all.deb`)
to the target machine and install it:

```sh
dpkg -i gpio-nct5104d-dkms_0.20171115-0_all.deb
```

Now at every kernel package update gpio-nct5104d module will be automatically
rebuilt and installed.

## Usage ##

### Load the driver: ###
`insmod gpio-nct5104d.ko`

### Unload the driver: ###
unexport all the used GPIO Pins first, then run
`rmmod gpio-nct5104d.ko`

### Export a GPIO Pin: ###
`echo 0 > /sys/class/gpio/export` will export/reserve/setup the gpio pin 0

### Unexport a GPIO Pin: ###
`echo 0 > /sys/class/gpio/unexport` will unexport/free the gpio pin 0

### Set GPIO Pin as output: ###
`echo out > /sys/class/gpio/gpio0/direction`

### Set GPIO Pin output value: ###
`echo 1 > /sys/class/gpio/gpio0/value`

### Read GPIO Pin value: ###
`cat /sys/class/gpio/gpio0/value`

### Invert GPIO Pin logic: ###
`echo 1 > /sys/class/gpio/gpio0/active_low`

### Using the Kernel interrupt system for GPIO Pins: ###
`echo <TRIGGER_TYPE> >/sys/class/gpio/gpio0/edge`
`TRIGGER_TYPE` can be one of the following values:
* none
* falling
* rising
* both

### Show GPIO states using kernel debug information: ###
`cat /sys/kernel/debug/gpio`
this has to be enabled in your kernel

## Additional information ##
see those pages for some c-code examples:

* http://wiki.gnublin.org/index.php/GPIO
* http://falsinsoft.blogspot.de/2012/11/access-gpio-from-linux-user-space.html

see the Kernel documentation for more details:

* https://www.kernel.org/doc/Documentation/gpio/sysfs.txt
* https://www.kernel.org/doc/Documentation/gpio/gpio-legacy.txt

and so on...
