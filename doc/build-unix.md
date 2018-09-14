UNIX BUILD NOTES
====================
Some notes on how to build ElenaCoin in Unix.

(For BSD specific instructions, see `build-*bsd.md` in this directory.)

To Build
---------------------

```bash
cd elenacoin/src
make -f makefile.unix
```

Dependencies
---------------------

These dependencies are required:

 Library     | Purpose          | Description
 ------------|------------------|----------------------
 libssl      | Crypto           | Random Number Generation, Elliptic Curve Cryptography
 libboost    | Utility          | Library for threading, data structures, etc
 libdb4.8    | Berkeley DB      | Wallet storage


Optional dependencies:

 Library     | Purpose          | Description
 ------------|------------------|----------------------
 miniupnpc   | UPnP Support     | Firewall-jumping support
 qt          | GUI              | GUI toolkit (only needed when GUI enabled)
 libqrencode | QR codes in GUI  | Optional for generating QR codes (only needed when GUI enabled)

For the versions used, see [dependencies.md](dependencies.md)

Memory Requirements
--------------------

C++ compilers are memory-hungry. It is recommended to have at least 1.5 GB of
memory available when compiling Bitcoin Core. On systems with less, gcc can be
tuned to conserve memory with additional CXXFLAGS:


    "--param ggc-min-expand=1 --param ggc-min-heapsize=32768"


## Linux Distribution Specific Instructions

### Ubuntu & Debian

Officially supported versions are Ubuntu 18.04 (recommended), Ubuntu 16.04 and Debian 9.
Compiling on older versions is possible, if "-DBOOST_NO_CXX11_SCOPED_ENUMS" is added to DEFS in makefile.

#### Dependency Build Instructions

Build requirements:

    sudo apt-get install build-essential libtool libssl-dev libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-test-dev libboost-thread-dev libboost-program-options-dev

BerkeleyDB is required for the wallet.

**For Ubuntu only:** db4.8 packages are available [here](https://launchpad.net/~bitcoin/+archive/bitcoin).
You can add the repository and install using the following commands:

    sudo apt-get install software-properties-common
    sudo add-apt-repository ppa:bitcoin/bitcoin
    sudo apt-get update
    sudo apt-get install libdb4.8-dev libdb4.8++-dev

Ubuntu and Debian have their own libdb-dev and libdb++-dev packages, but these will install
BerkeleyDB 5.1 or later, which break binary wallet compatibility with the distributed executables which
are based on BerkeleyDB 4.8.

Optional:

    sudo apt-get install libminiupnpc-dev

#### Dependencies for the GUI

If you want to build bitcoin-qt, make sure that the required packages for Qt development
are installed. Qt 5 is necessary to build the GUI.
To build without GUI pass `--without-gui`.

To build with Qt 5 you need the following:

    sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler

libqrencode (optional) can be installed with:

    sudo apt-get install libqrencode-dev

Once these are installed, they will be found by configure and a bitcoin-qt executable will be
built by default.


### Fedora

#### Dependency Build Instructions

Build requirements:

    sudo dnf install gcc-c++ libtool make openssl-devel boost-devel libdb4-devel libdb4-cxx-devel

Optional:

    sudo dnf install miniupnpc-devel

To build with Qt 5 you need the following:

    sudo dnf install qt5-qttools-devel qt5-qtbase-devel protobuf-devel

libqrencode (optional) can be installed with:

    sudo dnf install qrencode-devel

Notes
-----
The release is built with GCC and then "strip elenacoind" to strip the debug
symbols, which reduces the executable size by about 90%.

