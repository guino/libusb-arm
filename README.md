# libusb-arm cross compile for linaro toolchain
These are the steps required to compile libusb with/for arm-linux-eabihf linaro &amp; arm toolchain
* Download the linaro-gcc toolchain for your processor from https://releases.linaro.org/components/toolchain/binaries/ such as gcc-linaro-7.3.1-2018.05-x86_64_arm-linux-gnueabihf.tar.xz for a x86_64 linux host with Cortex A9 processor as target
* Download libusb 1.0 and libusb-compat from http://libusb.info and https://github.com/libusb/libusb-compat-0.1

I like to install the arm-toolchain under /usr/local/arm-toolchain like this:
```sh
# cd /usr/local
# tar xf /tmp/gcc-linaro-7.3.1-2018.05-x86_64_arm-linux-gnueabihf.tar.xz
# ln -s gcc-linaro-7.3.1-2018.05-x86_64_arm-linux-gnueabihf arm-toolchain
```
Now untar the libusb sources and cross-compile/install them:
```sh
cd /usr/local/
tar xf /tmp/libusb-1.0.x.tar.bz2
unzip /tmp/libusb-compat-0.1-master.zip
cd libusb-1.0.x
./configure --host=arm-linux-gnueabihf --prefix=/usr/local/arm-toolchain/arm-linux-gnueabihf CC=/usr/local/arm-toolchain/bin/arm-linux-gnueabihf-gcc --enable-udev=false
make install
cd ../libusb-compat-0.1.x
./configure --host=arm-linux-gnueabihf --prefix=/usr/local/arm-toolchain/arm-linux-gnueabihf CC=/usr/local/arm-toolchain/bin/arm-linux-gnueabihf-gcc
make install
cd ../arm-toolchain/arm-linux-gnueabihf/include/
ln -s libusb-1.0/libusb.h .
```
The release branch 1.0.9/0.1.4 was compiled with the above method while the 1.0.21/0.1.5 branch was compiled with buildroot which is a much longer process of configuration/compiling but does produce libusb with libudev support (which may not be required for your project).

The *--enable-udev=false* parameter may be removed if you have libudev.so (arm) in your toolchain (not the case with linaro toolchains)
