# EPCEncoderiOS
Build the EPCEncoder lib and framework for iOS, includes Converter.

Note: most of this project leverages this great tutorial to create a framework:

http://www.raywenderlich.com/65964/create-a-framework-for-ios
https://www.raywenderlich.com/65964/create-a-framework-for-ios

I got the framework and the static library to work.  I opted to use the framework version because it packed
all targets.  See the RFID apps: ValiTag, RapiTag, PosiTag, and TagIt...

libEPCEncoder.a was an intermediate step of the above tutorial, so I just dumped them all into the build
directory.  The completed framework is there as well.  Note, you need to build the Framework target in XCode
to get all this.  If you just build EPCEncoder, you won't get the framework or the lib in the build directory.

TPM - 11/18/15

With the release of XCode 7 for iOS 9, you have to choose your platform.  Before you could use the same
framework for both iOS and OSX.  No longer.  I've split this into two now, one framework for iOS and another
for OSX.

TPM - 11/18/16

Wow, literally one year later :)

So with XCode 8 for iOS 10, I'm getting a couple of ditto errors at the end of the framework build script,
but the directories it's complaining about are there, and the framework is complete (header and .m files 
copied, as is the library with all the proper sym links created to the latest version).

So, just build the EPCEncoder target, then right click on the libEPCEncoder.a Product in the left nav pane
under Products and select Show in Finder.

This takes you to the directory where the EPCEncoder.framework is also located (check the build date).
Open one of your dependent projects (ValiTag, RapiTag, etc) and delete the old framework.  Now drag and drop
the new framework into that project and viola, you are good to go!

Now,as I looked at this more, I noticed the "build" subdirectory in the project contained a framework that was
out of date. So I deleted it, ran the build again, XCode complained that I needed to specify a Signing entity
for the EPCEncoderTests (I added timmilne), and then I was able to select Product -> Scheme -> Framework and
build the framework.  Now it shows up in the local build subdirectory of the project, and you can copy it from
here to the dependent projects.

Note, I came across the wenderlich tutorial again (it really is good), as well as another that shows how to
create a Cocoa Pod that would better manage dependencies:

https://www.raywenderlich.com/65964/create-a-framework-for-ios
https://www.raywenderlich.com/126365/ios-frameworks-tutorial

Also note: as I was reviewing the first tutorial above, I noted that the fat build for the framework only
includes 5 frameworks:

arm7: Used in the oldest iOS 7-supporting devices
arm7s: As used in iPhone 5 and 5C
arm64: For the 64-bit ARM processor in iPhone 5S
i386: For the 32-bit simulator
x86_64: Used in 64-bit simulator

So, at some point you may need to update this for new architectures.


TPM - 11/20/16

Compiling for the iPhone 6, 7 and 7 plus was throwing an error that the EPCEncoder was missing the Arm64
architecture (as I suspected).

So I found this:
http://stackoverflow.com/questions/26552855/xcode-6-1-missing-required-architecture-x86-64-in-file

There is a setting it recommends:
Framework -> Build Settings -> Architecture -> Build Active Architecture Only
(set to No for framework and EPCEncoder)

Recompile the framework and library, and then copy it over to TagIt and it now works.  You can check that
the right architectures are there by right clicking on libEPCEncoder.a in the left nav pane, selecting
Show in Finder, then figuring out where it is (or the one in the local project's build directory) and 
running: lipo - info libEPCEncoder.a, and it should show you the architectures (armv7 and arm64)')

Now, if you want to make changes to the build script, scroll down to one of the comments in the link above:

Many use the build scripts found either here:
http://www.raywenderlich.com/41377/creating-a-static-library-in-ios-tutorial or here:
https://gist.github.com/sponno/7228256 for their run script in their target.

I was pulling my hair out trying to add x86_64, i386, armv7s, armv7, and arm64 to the Architectures section,
only to find lipo -info targetname.a never returning these architectures after a successful build.

In my case, I had to modify the target runscript, specifically step 1 from the gist link, to manually include
the architectures using -arch.

Step 1. Build Device and Simulator versions
xcodebuild -target ${PROJECT_NAME} ONLY_ACTIVE_ARCH=NO -configuration ${CONFIGURATION} -sdk iphoneos  BUILD_DIR="${BUILD_DIR}"
BUILD_ROOT="${BUILD_ROOT}" xcodebuild -target ${PROJECT_NAME} -configuration ${CONFIGURATION} -sdk iphonesimulator -arch x86_64 -arch i386 -arch armv7 -arch armv7s -arch arm64 BUILD_DIR="${BUILD_DIR}" BUILD_ROOT="${BUILD_ROOT}"

TPM here again - I think what's happening is the build settings is ARCHS_STANDARD, and eventually that
includes everything.  By setting that, and then setting build active architecture only to no you get all
the standard ones, eventually.  So I may not need to worry about this, just rebuild this on occasion.'


