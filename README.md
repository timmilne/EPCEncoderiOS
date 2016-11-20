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

This takes you to the directory where the EPCEncoder.framework is also located.  Open one of your dependent
projects (ValiTag, RapiTag, etc) and delete the old framework.  Now drag and drop the new framework into
that project and viola, you are good to go!

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
