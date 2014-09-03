EtherCAT  drivers
================

This package contains EtherCAT drivers to integrate with [IgH EtherCAT Master for Linux](http://etherlab.org/en/ethercat/).

For now this only contains **r8169** driver, for kernel **3.8.x** and **3.10.x**. They are used to control the Meka robot at Ensta ParisTech at 1Khz with [**rtai 4.0**](https://www.rtai.org/).

## Installation

#### Download EtherCAT 1.5.2
```bash
wget http://etherlab.org/download/ethercat/ethercat-1.5.2.tar.bz2
tar xfvj ethercat-1.5.2.tar.bz2
```

#### Get the drivers
```bash
cd ethercat-1.5.2
git clone https://github.com/ahoarau/ethercat-drivers.git
## Apply patch
patch -p1 < ethercat-drivers/r8169-3.8-3.10.patch
```

#### Build the EtherCAT master modules
```bash
./configure --enable-cycles --enable-r8169  --with-rtai-dir=/usr/realtime/ --disable-8139too --with-r8169-kernel=3.10
make -j$[$(nproc)+1] all modules
sudo make modules_install install
sudo depmod
```
#### Configure your environnement (see the install note on ethercat-1.5.2)
```bash
sudo -s
echo KERNEL==\"EtherCAT[0-9]*\", MODE=\"0664\" > /etc/udev/rules.d/99-EtherCAT.rules
cp etc/init.d/ethercat /etc/init.d/ethercat # to start the master
cp etc/sysconfig/ethercat /etc/sysconfig/ethercat # the config file
exit
```
Then copy the mac address of the ethernet card you'd like to use (use ifconfig) to MASTER0_DEVICE and set the driver your card should use to DEVICE_MODULES ( example: DEVICE_MODULES="r8169"):

```bash
sudo nano /etc/sysconfig/ethercat
```
#### Start EtherCAT master
Then when you have configured your environnement (see the install note on ethercat-1.5.2), you can start the driver:
```bash
sudo service ethercat restart
```
See output messages with dmesg:
```bash
dmesg
```

> Contact : Antoine Hoarau <hoarau.robotics@gmail.com>


