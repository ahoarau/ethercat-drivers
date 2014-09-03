EtherCAT  drivers
================

This package contains EtherCAT drivers to integrate with [IgH EtherCAT Master for Linux](http://etherlab.org/en/ethercat/).

For now this only contains **r8169** driver, for kernel **3.8.x** and **3.10.x**. They are used to control the Meka robot at Ensta ParisTech at 1Khz with [**rtai 4.0**](https://www.rtai.org/).

## Installation


```bash
wget http://etherlab.org/download/ethercat/ethercat-1.5.2.tar.bz2
tar xfvj ethercat-1.5.2
cd ethercat-1.5.2
git clone https://github.com/ahoarau/ethercat-drivers.git
#TODO: Apply patch
./configure --enable-cycles --enable-r8169  --with-rtai-dir=/usr/realtime/ --disable-8139too --with-r8169-kernel=3.10
make -j$[$(nproc)+1] all modules
sudo make modules_install install
sudo depmod
```
Then when you have configured your environnement (see the install note on ethercat-1.5.2), you can start the driver:
```bash
sudo service ethercat restart
```


> Contact : Antoine Hoarau <hoarau.robotics@gmail.com>


