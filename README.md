# Building the Strongswan for Android on OSX (High Sierra)

# Reference

* [Building the strongSwan VPN Client for Android](https://wiki.strongswan.org/projects/strongswan/wiki/AndroidVPNClientBuild)
* [strongswan android编译过程](https://blog.csdn.net/lllkey/article/details/79993609)


# Install Build Tools and Library

* Refer to [source:HACKING](https://wiki.strongswan.org/projects/strongswan/repository/entry/HACKING)
* Install gmp
```
    $ brew install gmp
```

NOTE. If you use bison 2.x which come with OSX, it will fail to build "settings_parser.y", so please ensure the bison is version 3.x by "bison -V".

To install the new bison and make it will be executed in building process, 
you can do:

```
    $ brew install bison
    $ brew link bison --force
    $ source ~/.bash_profile
```

# Prepare Strongswan and OpenSSL (BoringSSL) Source Tree

For building in OSX, try to put the source in case-insensitive filesystem (the
default filesystem), or when use the Android Studio to build the app, it will
complains the file system setting is not the same as IDE.

```
    # Make source folder
    $ mkdir -p ~/proj/tmp && cd ~/proj/tmp

    # Get strongswan latest source
    $ git clone git://git.strongswan.org/strongswan.git

    # Get boringssl source 
    $ cd ./strongswan/src/frontends/android/app/src/main/jni
    $ git clone git://git.strongswan.org/android-ndk-boringssl.git -b ndk-static openssl

    # Optionally, clear the tree
    $ cd ~/proj/tmp/strongswan

    # See Alexandre Duret Lutz's Autotools tutorial for aclocal (I did not read....)
    $ aclocal -I m4
    $ ./autogen.sh
    $ ./configure
    $ make dist

    Done
```
Take a note, if we want to clean the source tree:
```
    $ make maintainer-clean
```

# Install and Setup Android Studio

* Install [Android SDK and NDK](https://developer.android.com/studio/).  The Android Studio will be installed to "~/Library/Android".
* Launch Androind Studio, click "Open an existing Andriod Studo project" and select folder "~/proj/tmp/strongswan/src/frontends/android".  
* The initial launch of Android Studio, the NDK directory is not known. Select "File-->Project Structure" to install or setup NDK path (ex. ~/Library/Android/sdk/ndk-bundle).

# Build Native Part

* Modify ~/proj/tmp/strongswan/src/frontends/android/app/src/main/jni/Application.mk
```
    #APP_ABI := armeabi arm64-v8a x86 x86_64 mips mips64
    APP_ABI := armeabi-v7a arm64-v8a x86 x86_64 
```
* Build
```
    $ cd ~/proj/tmp/strongswan/src/frontends/android/app/src/main/jni
    $ ~/Library/Android/sdk/ndk-bundle/ndk-build
```

# Building the App

* Select "Build-->Make Project".

# Run APP

* In Androind Studio, select "Tools-->AVD Manager" to create a virtual devie.
* Select "Run-->Run 'app'"


Edward Yang, June 16, 2018.







