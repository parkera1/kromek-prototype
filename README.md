# Welcome to Kromek raw data processing project

# Setup
## on Raspberry Pi

    sudo apt-get install python-dev
    sudo apt-get install python-tz

    wget http://pypi.python.org/packages/source/C/Cython/Cython-0.16.tar.gz
    tar xzf Cython-0.16.tar.gz
    cd Cython-0.16
    sudo python setup.py install

    wget http://sourceforge.net/projects/libusb/files/libusb-1.0/libusb-1.0.9/libusb-1.0.9.tar.bz2
    tar xjf libusb-1.0.9.tar.bz2
    cd libusb-1.0.9
    ./configure
    make
    sudo make install

    git clone https://github.com/gbishop/cython-hidapi.git
    cd cython-hidapi
    vi setup.py

Now we need to change the path to libusb from /usr/include/libusb-1.0 to /usr/local/include/libusb-1.0 and change the library path from ‘i386-linux-gnu’ to ‘arm-linux-gnueabihf’. Make that change then save.

    sudo cp /lib/arm-linux-gnueabihf/libudev.so.0 /usr/lib/arm-linux-gnueabihf/libudev.so
    sudo python setup.py install

## on Ubuntu PC
Follow same process as for Raspberry Pi except for cython-hidapi. Replace setup section in setup.py:

    setup(
        cmdclass = {'build_ext': build_ext},
        ext_modules = [Extension("hid", ["hid.pyx", "hid-libusb.c"],
                      libraries=["usb-1.0", "udev", "rt"])]
    )

# Usage
    Usage: radangel.py [options] <logfile>

    Options:
      -h, --help            show this help message and exit
      -d, --database        upload to mongodb database
      -c CAPTURECOUNT, --capturecount=CAPTURECOUNT
                            specify the total capture counts (default 0 meaning
                            unlimited)
      -t CAPTURETIME, --capturetime=CAPTURETIME
                            specify the capture time in seconds (default 0 meaning
                            unlimited)
      -e, --enumerate       enumerate USB HID devices only
      -p PATH, --path=PATH  specify USB HID devices path to capture

## Sample

Unlimited capture with database upload:
    sudo python radangel.py -d unlimited.log

Time capture:
    sudo python radangel.py -t 300 capture_5minutes.log

Count capture:
    sudo python radangel.py -c 1000 capture_1000counts.log
