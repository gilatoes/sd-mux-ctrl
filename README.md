# sd-mux-ctrl
The SD Mux Control software prerequisite:
# Original SDWire
https://git.tizen.org/cgit/tools/testlab/sd-mux/
Information:
https://docs.dasharo.com/transparent-validation/sd-wire/getting-started/
https://wiki.tizen.org/SDWire

# FTDI driver 
https://www.intra2net.com/en/developer/libftdi/download.php

For Ubuntu 16.04 LTS, the apt-get install FTDI library is version 1.2. but SDWire requires >1.4 version to be build using cmake. Therefore the FTDI library needs to be built.

Other version of Ubuntu might comes with FTDI > 1.4 and can skip the build process.

## Building the FTDI driver
Building the FTDI library will requires the following libraries and dependencies:
```
sudo apt-get install build-essential
sudo apt-get install git-core
sudo apt-get install cmake
sudo apt-get install doxygen
sudo apt-get install libusb-1.0-devel
sudo apt-get install libconfuse-dev
sudo apt-get install swig python-dev
sudo apt-get install libboost-all-dev
```

## Get the and build the source:
```
git clone git://developer.intra2net.com/libftdi
cd libftdi

cd libftdi
mkdir build
cd build
cmake  -DCMAKE_INSTALL_PREFIX="/usr" ../
make
sudo make install
```
# Building the SDWire library
# Install the prerequisite for the SDWire library:
[comment]: <> (sudo apt-get install libftdi1-dev)
```
sudo apt-get install libpopt-dev
sudo apt-get install cmake
```

# Build the SDWire library:
```
git clone https://github.com/gilatoes/sd-mux-ctrl
cd sd-mux
mkdir build
cd build
cmake ..
sudo make install
```

## Connect the SDWire microUSB to the host and read out the serial number, vendor and product ID
```
dmesg -w
```

## Locate the values
```
[26022.053124] usb 2-2.2: new full-speed USB device number 5 using uhci_hcd
[26022.300217] usb 2-2.2: New USB device found, idVendor=0403, idProduct=6015
[26022.300219] usb 2-2.2: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[26022.300220] usb 2-2.2: Product: FT200X USB I2C
[26022.300220] usb 2-2.2: Manufacturer: FTDI
[26022.300221] usb 2-2.2: SerialNumber: DK6ZSFJ9
[26023.332780] usbcore: registered new interface driver usbserial_generic
[26023.332790] usbserial: USB Serial support registered for generic
[26023.336939] usbcore: registered new interface driver ftdi_sio
[26023.336945] usbserial: USB Serial support registered for FTDI USB Serial Device
[26023.336968] ftdi_sio 2-2.2:1.0: FTDI USB Serial Device converter detected
[26023.336982] usb 2-2.2: Detected FT-X
[26023.339730] usb 2-2.2: FTDI USB Serial Device converter now attached to ttyUSB0

```

# Configure the SDWire memory:
```
sudo sd-mux-ctrl --device-serial=DK6ZSFJ9 --vendor=0x0403 --product=0x6015 --device-type=sd-wire --set-serial=sd-wire_11
```

# Reconnect the USB and check the list command
```
sudo sd-mux-ctrl --list
```

## Expected Output:
```
xxx@ubuntu:~$ sudo sd-mux-ctrl --device-serial=sd-wire_11 --show-serial
Number of FTDI devices found: 1
Dev: 0, Manufacturer: SRPOL, Serial: sd-wire_11, Description: sd-wire

```

## Connect to RPI
```
sudo sd-mux-ctrl --device-serial=sd-wire_11 --rpi
```

## Connect to Host
```
sudo sd-mux-ctrl --device-serial=sd-wire_11 --host
```

# Installing the Original RPI image
Use Etcher to copy the Raspberry Pi image into the SD card.
Download the image from:
https://www.raspberrypi.com/software/operating-systems/

For RPI lite,  the file is 2022-0404-raspios-bulleye-armhf-lite.img.xz
Use the Etcher graphical software to copy the image to the SD card.

## Install Etcher 

```
curl -1sLf \
   'https://dl.cloudsmith.io/public/balena/etcher/setup.deb.sh' \
   | sudo -E bash
```

Update and install the etcher repo
```
sudo apt-get update
sudo apt-get install balena-etcher-electron
```

Remove ether
```
sudo apt-get remove balena-etcher-electron
rm /etc/apt/sources.list.d/balena-etcher.list
apt-get clean
rm -rf /var/lib/apt/lists/*
apt-get update
```

