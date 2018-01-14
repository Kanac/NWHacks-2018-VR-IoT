Amazon Kinesis Video Streams Producer SDK Cpp

## License

This library is licensed under the Amazon Software License.

## Introduction
Amazon Kinesis Video Streams makes it easy to securely stream video from connected devices to AWS for analytics, machine learning (ML), and other processing. 

Amazon Kinesis Video Streams Producer SDK for C/C++ makes it easy to build an on-device application that securely connects to a video stream, and reliably publishes video and other media data to Kinesis Video Streams. It takes care of all the underlying tasks required to package the frames and fragments generated by the device's media pipeline. The SDK also handles stream creation, token rotation for secure and uninterrupted streaming, processing acknowledgements returned by Kinesis Video Streams, and other tasks.  

Amazon Kinesis Video Streams Producer SDK for C/C++ contains the following sub-directories/projects:
* kinesis-video-pic - The Platform Independent Codebase which is the basic building block for the C++/Java producer SDK. The project includes multiple sub-projects for each sub-component with unit tests.
* kinesis-video-producer - The C++ Producer SDK with unit test.
* kinesis-video-producer-jni - The C++ wrapper for JNI to expose the functionality to Java/Android.
* kinesis-video-gst-demo - C++ GStreamer sample application.
* kinesis-video-native-build - Native build directory with a build script for Mac OS/Linux/Raspberry PI. This is the directory that will contain the artifacts after the build.

## Building from Source
There are few build-time tools/dependencies which need to be installed in order to build the core SDK libraries and the samples.

### Build Dependencies 
Please install the following additional build tools before proceeding (the example below is for Mac Os X).
* autoconf 2.69 (License GPLv3+/Autoconf: GNU GPL version 3 or later) http://www.gnu.org/software/autoconf/autoconf.html
* cmake 3.7/3.8 https://cmake.org/
* flex 2.5.35 Apple(flex-31)
* bison 2.4 (GNU License)
* automake 1.15.1 (GNU License)
* libtool (Apple Inc. version cctools-898)
* xCode (Mac OS) / clang / gcc (xcode-select version 2347)
* Java jdk (for Java JNI compilation)

After you've downloaded the code from GitHub, you can build it on Mac OS or Ubuntu using ./kinesis-video-native-build/install-script script. This will produce the core library, the JNI library, unit tests executable and the sample GStreamer application. The script will download and build the dependent open source components in the 'downloads' directory and link against it. 

**Important:** Set the JAVA_HOME environment variable to your version of the JDK `export JAVA_HOME=<your java home directory>` in order to build and link JNI component.

The bulk of the install script is building the open source dependencies. The project is based on CMake so the open source components building can be skipped if the system versions can be used for linking. Running 'cmake . && make' from the kinesis-video-native-build directory will build and link the SDK.

The `install-script` will take some time to bring down and compile the open source components. If anything fails or the script is interrupted, re-running it will pick up from the place where it last left off. The sub-sequent run will be building just the modified SDK components or applications which is much faster.

## Open Source Dependencies
The projects depend on the following open source components. Running `install-script` will download and build the necessary components automatically.

#### Core
* openssl (crypto and ssl) - https://github.com/openssl/openssl/blob/master/LICENSE
* curl lib - https://curl.haxx.se/docs/copyright.html
* log4cplus - https://github.com/log4cplus/log4cplus/blob/master/LICENSE
* jsoncpp - https://github.com/open-source-parsers/jsoncpp/blob/master/LICENSE

#### Unit Tests
* googletest - https://github.com/google/googletest

#### GStreamer Demo App
* gstreamer - https://gstreamer.freedesktop.org/documentation/licensing.html
* gst-plugins-base 
* gst-plugins-good 
* gst-plugins-bad 
* gst-plugins-ugly
* x264 - https://www.videolan.org/developers/x264.html

## Certificate store integration
Kinesis Video Streams Produicer SDK for C++ needs to establish trust with the backend service through TLS. This is done through validating the CAs in the public certificate store. On Linux-based models, this store is located in /etc/ssl/ directory by default. 

Please download the PEM file from 
https://www.amazontrust.com/repository/SFSRootCAG2.pem
 
to `/etc/ssl/cert.pem`. Append to the end of the file if it exists.

Many platforms come with a cert file with a lot of the well-known public certs in them.

### Launching the sample application / unit test
Define AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY environment variables with the AWS access key id and secret key:
`export AWS_ACCESS_KEY_ID={AccessKeyId}`
`export AWS_SECRET_ACCESS_KEY={SecretAccessKey}`

optionally, set `AWS_SESSION_TOKEN` if integrating with temporary token and `AWS_DEFAULT_REGION` for the region other than `us-west-2`

#### C++ Unit tests
* The executable will be built in ./kinesis-video-native-build/start. Launch it and it will run the unit test and kick off dummy frame streaming.

#### GStreamer demo app
* The GStreamer demo app will be built in kinesis-video-native-build/kinesis_video_gstreamer_sample_app. Launch it with a stream name and it will start streaming from the camera.

### Enabling verbose logs
Define `HEAP_DEBUG` and `LOG_STREAMING` C-defines by uncommenting the appropriate lines in ./kinesis-video-native-build/CMakeList.txt

### Install Steps for Ubuntu 17.x
The following are the steps to install the build-time prerequisites for Ubuntu 17.x

* Install git: sudo apt-get install git

$ git --version
git version 2.14.1




* Install cmake: sudo apt-get isnstall cmake

$ cmake --version
cmake version 3.9.1

CMake suite maintained and supported by Kitware (kitware.com/cmake).




* Install libtool: sudo apt-get install libtool (some images come preinstalled)

2.4.6-2




* Install libtool-bin: sudo apt-get install libtool-bin

$ libtool --version
libtool (GNU libtool) 2.4.6
Written by Gordon Matzigkeit, 1996

Copyright (C) 2014 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.





* Install automake: sudo apt-get install automake

$ automake --version
automake (GNU automake) 1.15
Copyright (C) 2014 Free Software Foundation, Inc.
License GPLv2+: GNU GPL version 2 or later <http://gnu.org/licenses/gpl-2.0.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Written by Tom Tromey <tromey@redhat.com>
       and Alexandre Duret-Lutz <adl@gnu.org>.





* Install bison: sudo apt-get install bison

$ bison -V
bison (GNU Bison) 3.0.4
Written by Robert Corbett and Richard Stallman.

Copyright (C) 2015 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.





* Install g++: sudo apt-get install g++

 g++ --version
g++ (Ubuntu 7.2.0-8ubuntu3) 7.2.0
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.




* Install curl: sudo apt-get install curl

$ curl --version
curl 7.55.1 (x86_64-pc-linux-gnu) libcurl/7.55.1 OpenSSL/1.0.2g zlib/1.2.11 libidn2/2.0.2 libpsl/0.18.0 (+libidn2/2.0.2) librtmp/2.3
Release-Date: 2017-08-14
Protocols: dict file ftp ftps gopher http https imap imaps ldap ldaps pop3 pop3s rtmp rtsp smb smbs smtp smtps telnet tftp 
Features: AsynchDNS IDN IPv6 Largefile GSS-API Kerberos SPNEGO NTLM NTLM_WB SSL libz TLS-SRP UnixSockets HTTPS-proxy PSL




* Install pkg-config: sudo apt-get install pkg-config

$ pkg-config --version
0.29.1





* Install flex: sudo apt-get install flex

$ flex --version
flex 2.6.1





* Install Open JDK: sudo apt-get install openjdk-8-jdk

$ java -showversion
openjdk version "1.8.0_151"
OpenJDK Runtime Environment (build 1.8.0_151-8u151-b12-0ubuntu0.17.10.2-b12)
OpenJDK 64-Bit Server VM (build 25.151-b12, mixed mode)




* Set JAVA_HOME environment variable: export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/



* Run the build script: ./install-script


## Troubleshooting.

* Ubuntu builds link against the system versions of the open source component libraries or missing .so files (./kinesis-video-native-build/start shows linkage against system versions of the open source libraries). We are working on providing fix but the immediate steps to remedy is to run `rm -rf ./kinesis-video-native-build/CMakeCache.txt ./kinesis-video-native-build/CMakeFiles` and run `./kinesis-video-native-build/install-script` to rebuild and re-link the project only.

* Raspberry PI failure to load the camera device. To check this is the case run `ls /dev/video*` - it should be file not found. The remedy is to run the following:

$ls /dev/video* {not found}

$vcgencmd get_camera {output is similar to supported=1 detected=1}

if the driver does not detect the camera then 

* Check for the camera setup and whether it's connected properly
* Run firmware update `$ sudo rpi-update` and restart

$sudo modprobe bcm2835-v4l2

$ls /dev/video* {lists the device}


* Raspberry PI timestamp/range assertion at runtime. Update the Raspberry PI firmware.

$ sudo rpi-update 

$ sudo reboot


* Raspberry PI GStreamer assertion on gst_value_set_fraction_range_full: assertion 'gst_util_fraction_compare (numerator_start, denominator_start, numerator_end, denominator_end) < 0' failed. The uv4l service running in the background. Kill the service and restart the sample app.


* Raspberry PI seg fauls after some time running on libx264.so. Rebuilding the libx264.so library and re-linking the demo application fixes the issue.


* If an USB webcam is used with a Raspberry PI and the webcam does not support 720p. The following error can occur. Reduce the resolution to 480p at line 336 and 356 should fix the problem. (change the number from 1280 and 720 to 640 and 480)

Error received from element source: Internal data stream error.
Debugging information: gstbasesrc.c(2939): gst_base_src_loop (): /GstPipeline:test-pipeline/GstV4l2Src:source:
streaming stopped, reason not-negotiated (-4)

## Release Notes
### Release 1.1.1 (December 2017)
* Fix USB webcam support
* Known issues:
    * If USB webcam doesn't support 720p, then gstreamer negotiation will fail. Trying lower resolution as mentioned in Troubleshooting may fix this issue.
### Release 1.1.0 (December 2017)
* Addition of a received application ACK notification callback
* Lifecycle management improvements
* Exposed failure on progressive back-off/retry logic
* Hardening/fixing various edge-cases
* Fixing Raspberry PI frame dropping issue
### Release 1.0.0 (November 2017)
* First release of the Amazon Kinesis Video Producer SDK for Cpp.
* Known issues:
    * Missing build scripts for Windows-based systems.
    * Missing cross-compile option.
    * Sample application/unit tests can't handle buffer pressures properly - simple print in debug log.