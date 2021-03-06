To build SkypeExport for OS X, you need Xcode Command Line Tools, as well as Boost 1.58 (or higher) installed in /usr/local/include/boost and /usr/local/libs/

Here's how you install and build Boost on OS X:


* Install Xcode Command Line Tools (on older OS X versions you must install the whole Xcode, but on newer systems you can install just the tools):

xcode-select --install


* Get the latest Boost Unix package from http://www.boost.org/users/download/ and unpack the .tar.bz2 file:

tar --bzip2 -xf boost_*.tar.bz2
cd boost_*


* Compile and install Boost Unix (takes ages):

./bootstrap.sh
./b2 -a link=static variant=release threading=multi toolset=darwin architecture=x86 address-model=32_64 stage

This builds release-only, multithreading-aware libraries, static (no shared junk), for 32 and 64 bit intel. There is NO PowerPC build support in modern Xcode. If you want to release for PowerPC, then you must separately build libraries for that using Xcode 3 (and build with architecture=combined), which is a hassle. And if you want to build for old versions of OS X you have to install the full Xcode and download the older platform SDKs, as well as modifying your build parameters. Let's not bother with all that hassle.


* Now install the Boost library with:

./b2 link=static variant=release threading=multi toolset=darwin architecture=x86 address-model=32_64 install

It will automatically install the library in /usr/local/libs/libboost-* and /usr/local/include/boost/* -- this can be overridden with --prefix=<your path> but it's not recommended, as /usr/local/ is the default Boost path on UNIX and Linux.

You can verify that the libraries are 32/64 Intel by using "lipo -info" on the resulting libs (such as "lipo -info /usr/local/lib/libboost_signals.a"), which should say "i386 x86_64".

After the process is complete, you can then delete the installation source folder, since it only contains some Boost documentation + examples, and is of no use unless you need that kind of thing on your local drive. It's all available online anyway.


* Now just build SkypeExport by typing ./build_macosx.sh, which creates a Universal 32/64bit Intel Binary usable on OS X.

