# EPCEncoderiOS
Build the EPCEncoder lib and framework for iOS, includes Converter.

Note: most of this project leverages this great tutorial to create a framework:

http://www.raywenderlich.com/65964/create-a-framework-for-ios

I got the framework and the static library to work.  I opted to use the framework version because it packed all targets.
See the RFID apps: ValiTag, and RapiTag

libEPCEncoder.a was an intermediate step of the above tutorial, so I just dumped them all into the build
directory.  The completed framework is there as well.  Note, you need to build the Framework target in XCode
to get all this.  If you just build EPCEncoder, you won't get the framework or the lib in the build directory.

TPM - 11/18/15

With the release of XCode 7 for iOS 9, you have to choose your platform.  Before you could use the same framework
for both iOS and OSX.  No longer.  I've split this into two now, one framework for iOS and another for OSX.
