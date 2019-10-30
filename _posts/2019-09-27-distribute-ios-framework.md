---
layout: post
title: Cách tạo ra 1 iOS framework để distribute ra bên ngoài
---
So you have a solution to a problem that many other iOS developers have and you want to help them, but you also want to protect your code and build a business? Binary framework, that’s what you want.

You might have noticed that you can’t see the source code of the UIKit frameworks you use to build your iOS apps on top of every day. That’s because Apple ships the UIKit as a binary framework in its iOS SDK.

If you’re distributing a framework that contains intellectual property, this is the way to do it.

 

## Binary Frameworks

A binary framework is already compiled source code with resources with a defined interface that you can use in your apps. It comes in two flavors: a static library and dynamic framework. In this article, we’ll focus on dynamic frameworks.

I will walk you through creating an iOS binary framework and (spoiler alert: here comes the real tricky part…) distributing one.

We have quite the experience with this as we have been distributing the iOS Instabug SDK as a binary framework for many years now, through many changes (static to dynamic) and tools (migrating to CocoaPods), and from recently shipping one monolithic framework to two frameworks.

So let’s get to it.

 

 

## Contents
 

#### SETUP

* Creating a Demo App

#### CREATING A BINARY FRAMEWORK

* Explaining What Just Happened
* Adding a Very Basic Function
* Using Your Binary Framework in Your Demo App
* Recapping What You Just Did

#### DISTRIBUTING A BINARY FRAMEWORK

* Exporting Framework
* Creating a Second Test App
* Integrating Your Binary Framework in Your Test App
* Publishing Your Test App
* Recapping What You Just Did

#### ONE MORE THING



## Setup
 

A good practice is to always have a demo app for your binary framework. This will make testing your changes easy and quick in an environment you already familiar with. Let’s start by creating a simple iOS app.

 

 

## Creating a Demo App
Open Xcode.

From the top menu select New.
![xcode](/img/2019-09-27/1.png){:class="img-responsive"}

From the submenu, select Project.
![xcode](/img/2019-09-27/2.png){:class="img-responsive"}


Choose Single View App and press Next.

![xcode](/img/2019-09-27/3.png){:class="img-responsive"}

Set Product Name to “HelloDemoApp” and press Next.

![xcode](/img/2019-09-27/4.png){:class="img-responsive"}

Choose your preferred destination for the new project.

Congratulations, you just created your demo app!

 
## Creating a Binary Framework
 

Now that you have your demo app in place, let’s add your new binary framework.

Go to ```HelloDemoApp.Xcodeproj``` — the first file in the project navigator panel on the far left.

![xcode](/img/2019-09-27/5.png){:class="img-responsive"}

If the left-side menu isn’t expanded, tap on this button at the top left:![xcode](/img/2019-09-27/6.png){:class="img-responsive"}

You will find ```TARGETS``` and listed are your apps. That’s how Xcode organizes its files: ```Workspace > Project > Targets```.

Targets have compiled sources (source files) and source files can be shared with other targets in the same project.

Each target produces a Product. In the case of an app, the product is your app.

At the bottom left you will find these icons: ![xcode](/img/2019-09-27/7.png){:class="img-responsive"} Tap on the ![xcode](/img/2019-09-27/8.png){:class="img-responsive"} to add a new target.

Scroll to the ```Framework & Library``` section.

![xcode](/img/2019-09-27/9.png){:class="img-responsive"}

Choose ```Cocoa Touch Framework``` then press ```Next```.

Set the Product Name to “HelloLoggingFramework” and make sure the language is set to Objective-C. (Don’t be scared — this article is about the setup, not the code. If you’re curious about why we use Objective-C, check out Why My Team Doesn’t Use Swift And Can’t Anytime Soon.)

![xcode](/img/2019-09-27/10.png){:class="img-responsive"}

Press ```Finish```.

 
## Explaining What Just Happened
You should now see two new targets below your app in the left-side menu: ```HelloLoggingFramework``` and ```HelloLoggingFrameworkTests```.

You should also see two new directories in the project navigator panel on the far left with the same names. Xcode has generated two new targets for you: the framework target and a unit test target for that framework.

If you expand the ```HelloLoggingFramework``` directory, you will find two files: ```Info.plist``` and ```HelloLoggingFramework.h``` — the latter is the umbrella header file that will be used to communicate between your framework and the host app (written in Swift or Objective-C).

 

![xcode](/img/2019-09-27/11.png){:class="img-responsive"}

 
## Adding a Very Basic Function
Next, let’s add a class to your new binary framework.

 

### Create a new class

We will create a new class to use in your app. Let’s call it ```HelloLogger```

Right click on the ```HelloLoggingFramework``` directory in the project navigator panel.

Choose ```New File```… from the menu.

 

![xcode](/img/2019-09-27/12.png){:class="img-responsive"}

 

Choose ```Cocoa Touch Class``` and press ```Next```.

![xcode](/img/2019-09-27/13.png){:class="img-responsive"}

Make sure you set:

Class: ```“HelloLogger”```
Subclass of: ```“NSObject”```
Language: ```“Objective-C”```
![xcode](/img/2019-09-27/14.png){:class="img-responsive"}

Press ```Next``` then ```Create```.

Done!

 

### Add a single method to that class

Now we will add the method ```helloWithText:``` that will just append “Hello” to the beginning of the text and print it in the console.

Open the ```HelloLogger.h``` file and add this line:
``` objc
- (void)helloWithText:(NSString *)text;
```
![xcode](/img/2019-09-27/15.png){:class="img-responsive"}

Then, open ```HelloLogger.m``` and add this snippet:
``` objc
- (void)helloWithText:(NSString *)text {
  NSLog(@"Hello, %@", text);
}
```
![xcode](/img/2019-09-27/16.png){:class="img-responsive"}

Done with that, too! Good progress.

 

 

### Using Your Binary Framework in Your Demo App
Next, we will use your new binary framework in your app.

Open ```ViewController.swift``` in your HelloDemoApp directory in the project navigator panel.

Add an import statement for your binary framework at the top of the file:

```import HelloLoggingFramework```
![xcode](/img/2019-09-27/17.png){:class="img-responsive"}

Now, try to create an instance of your class in ```viewDidLoad``` and add this line:
```swift
let helloLogger = HelloLogger()
```
![xcode](/img/2019-09-27/18.png){:class="img-responsive"}

OOPS!

It says ```Use of unresolved identifier 'HelloLogger'```. We didn’t have any problems importing your binary framework, so that’s fine, but we can’t use its only class. Why?

That’s because any class you create in a binary framework by default has access scope to ```Project```, which isn’t accessible outside that target. To fix this:

 

### Make your class public

Select the ```HelloLoggingFramework``` target from the left-side menu.

Select ```Build Phases``` from the top bar.

![xcode](/img/2019-09-27/19.png){:class="img-responsive"}

Expand the ```Headers``` section.

Drag your class ```HelloLogger.h``` from Project to Public.

Now let’s go back to ```ViewController.swift``` from the far left project navigator panel and check that error again.

Still there. ?

Remember that ```HelloLoggingFramework.h```? Let’s take a look at it.

![xcode](/img/2019-09-27/20.png){:class="img-responsive"}

It says:


> In this header, you should import all the public headers of your framework using statements like 
``` objc
#import <HelloLoggingFramework/PublicHeader.h>
 ```

```HelloLoggingFramework/PublicHeader.h``` is what’s called an “umbrella header” of your binary framework. It’s your interface with the outside world and it dictates that to expose any class, you need to import its header there.

So let’s add this:
``` objc
#import <HelloLoggingFramework/HelloLogger.h>
```
![xcode](/img/2019-09-27/21.png){:class="img-responsive"} Now, let’s head back to ```ViewController.swift```. The error is gone. ?

Next, let’s actually use that class.

Add this line:
```swift
helloLogger.hello(withText: "World")
```
![xcode](/img/2019-09-27/22.png){:class="img-responsive"}

Run the demo app by clicking the ![xcode](/img/2019-09-27/23.png){:class="img-responsive"} icon in the top left of Xcode, and check the console at the bottom right.

![xcode](/img/2019-09-27/24.png){:class="img-responsive"}

Good job!

 

## Recapping What You Just Did
* You created a demo app.
* You created a binary framework to use in your app.
* You added a class to this binary framework.
* You exposed that class to use in your app.
* You called that class from your binary framework to your app.
Now you have a perfect setup for developing your binary framework, but how about sharing that functionality with others without sharing the code?

 

 

## Distributing a Binary Framework
 

In this section, you will distribute your binary framework to a second app, but first we need to export your binary framework.

 

 

## Exporting Your Framework
Select your ```HelloLoggingFramework``` target from the Schemes menu at the top of Xcode.

 

![xcode](/img/2019-09-27/25.png){:class="img-responsive"}

 

From the ```Product``` menu at the top bar, choose ```Clean```, then choose ```Build```.

![xcode](/img/2019-09-27/26.png){:class="img-responsive"}

Find your binary framework by expanding the ```Products``` directory in the project navigator panel. Right click on ```HelloLoggingFramework.framework```, then press ```Show in Finder```.

 

![xcode](/img/2019-09-27/27.png){:class="img-responsive"}

 

Grab your binary framework file ```HelloLoggingFramework.framework``` and save it somewhere you can access later.

 

 

## Creating a Second Test App
You will now create another iOS app.

It’s a simple ```Single View App```, like the one you created before.

Let’s call it ```“TestApp”```. This app will be for simulating your users’ integrations.

![xcode](/img/2019-09-27/28.png){:class="img-responsive"}

Run your test app  to make sure everything is working fine.

All good? Great.

 

 

## Integrating Your Binary Framework in Your Test App
Drag your binary framework file ```HelloLoggingFramework.framework``` and drop it into your ```TestApp``` directory in the project navigator panel.

You should see this prompt:

![xcode](/img/2019-09-27/29.png){:class="img-responsive"}

Press ```Finish```.

Now, go to ```ViewController.swift``` and use your binary framework like before.

Add an ```import``` statement for your binary framework at the top of the file:
```swift
import HelloLoggingFramework
```
Then, add this snippet in viewDidLoad:

```swift
let logger = HelloLogger()
logger.hello(withText: "world")
```
![xcode](/img/2019-09-27/30.png){:class="img-responsive"}

Run your app. 

If you see this error below, it’s because your dynamic framework needs to be embedded in your app.
```
dyld: Library not loaded: @rpath_HelloLoggingFramework.framework_HelloLoggingFramework
Referenced from: _Users_yousefhamza_Library_Developer_CoreSimulator_Devices_70678C21-1274-4422-9472-661307C51C24_data_Containers_Bundle_Application_EF6F9477-63E9-4024-AD90-4AEC3EF3A8FA_TestApp.app_TestApp
Reason: image not found
``` 

You can do this by selecting ```TestApp.Xcodeproj``` from your project navigator.

Then, select your ```TestApp``` target.

Add + your binary framework in the Embedded Binaries section.

![xcode](/img/2019-09-27/31.png){:class="img-responsive"}

Run your test app. 

Everything works and console logs are just as expected. ?

![xcode](/img/2019-09-27/32.png){:class="img-responsive"}

 

 

## Publishing Your Test App
When you publish your framework out there for other people, you should consider their usage end to end.

In the Schemes menu at the top of Xcode, choose a physical device or ```Generic iOS Device```.

From the top bar, choose ```Product > Archive```.

![xcode](/img/2019-09-27/33.png){:class="img-responsive"}

Error?

```
ld: warning: ignoring file /Users/yousefhamza/Repos/TestApp/TestApp/HelloLoggingFramework.framework/HelloLoggingFramework, file was built for x86_64 which is not the architecture being linked (arm64): /Users/yousefhamza/Repos/TestApp/TestApp/HelloLoggingFramework.framework/HelloLoggingFramework
Undefined symbols for architecture arm64:
"_OBJC_CLASS_$_HelloLogger", referenced from:
objc-class-ref in ViewController.o
ld: symbol(s) not found for architecture arm64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
 ```

The decryption of that says that your binary framework was built for architecture x86_64 (the simulator) and we are now building for a device, whose architecture is arm64, and Xcode can’t find it in your binary framework.

If we just went ahead and built our framework on a device to get the arm64 architecture, we would get that error when trying to build the test app on the simulator.

So what do we do?

 

## Introduce an aggregated target

You have been introduced to the framework target, now we will introduce a new target: the aggregated target.

An aggregated target is an Xcode target that lets you build a group of targets at once. It doesn’t have any Products itself, like an app for an app target or a framework for framework targets. It doesn’t have build rules either, but it can have a ```Run Script``` build phase or a ```Copy Files``` build phase only.

Let’s go back to our original project (demo app + framework).

In the top bar, select ```File > New > Target```.

In the ```Cross-platform``` section under Other, you will find ```Aggregate```. Press ```Next```.

![xcode](/img/2019-09-27/34.png){:class="img-responsive"}

Let’s just call the Product Name: ```“Framework”```. Press ```Finish```.

First, we need it to run with configuration release.

Open the Schemes menus and choose your new ```Framework``` scheme.

![xcode](/img/2019-09-27/35.png){:class="img-responsive"}

Then, open it again and choose ```Edit Scheme```….

From Run, set Build Configuration to ```Release```.

![xcode](/img/2019-09-27/36.png){:class="img-responsive"}

Now, in the just created Framework aggregated target, let’s go to the ```Build Phases``` tab.

![xcode](/img/2019-09-27/37.png){:class="img-responsive"}

Add + your binary framework to the ```Target Dependencies``` section.

![xcode](/img/2019-09-27/38.png){:class="img-responsive"}

Now for a new trick. We will add a build phase from that + button above ```Target Dependencies```.

![xcode](/img/2019-09-27/39.png){:class="img-responsive"}

Choose ```New Run Script Phase```.

Let’s write that script!

 

 

## LIPO

LIPO will be our friend in this section. It’s what will actually fix our previous error by combining the binaries of your framework built for the simulator (```arch x86_64```) and for the device (```arch arm64```).

Note that in this step, we will archive the frameworks, not only build them. We need to do this to get the “. bcsymbolmap” files. These are needed with the framework and its dYSMs so when Apple rebuilds your framework on iTunes Connect, you can get the final dYSMs of your framework for your favorite crash reporting tool to symbolicate your crash reports.

In the following steps, we will be working with lots of paths, so it’s easier to create and store them in some variables. Add the following snippet to the top of the script:
```shell
FRAMEWORK_NAME="HelloLoggingFramework"

SIMULATOR_LIBRARY_PATH="${BUILD_DIR}/${CONFIGURATION}-iphonesimulator/${FRAMEWORK_NAME}.framework"

DEVICE_LIBRARY_PATH="${BUILD_DIR}/${CONFIGURATION}-iphoneos/${FRAMEWORK_NAME}.framework"

DEVICE_BCSYMBOLMAP_PATH="${BUILD_DIR}/${CONFIGURATION}-iphoneos"

DEVICE_DSYM_PATH="${BUILD_DIR}/${CONFIGURATION}-iphoneos/${FRAMEWORK_NAME}.framework.dSYM"
SIMULATOR_DSYM_PATH="${BUILD_DIR}/${CONFIGURATION}-iphonesimulator/${FRAMEWORK_NAME}.framework.dSYM"

UNIVERSAL_LIBRARY_DIR="${BUILD_DIR}/${CONFIGURATION}-iphoneuniversal"

FRAMEWORK="${UNIVERSAL_LIBRARY_DIR}/${FRAMEWORK_NAME}.framework"

OUTPUT_DIR="./HelloLoggingFramework-Aggregated"
 ```

Then add this line to your script:
```shell
Xcodebuild -project ${PROJECT_NAME}.Xcodeproj -scheme ${FRAMEWORK_NAME} -sdk iphonesimulator -configuration ${CONFIGURATION} clean install CONFIGURATION_BUILD_DIR=${BUILD_DIR}/${CONFIGURATION}-iphonesimulator
```

This builds your framework for the target simulator.

 

Add the following line to your script, too:
```shell
Xcodebuild -project ${PROJECT_NAME}.Xcodeproj -scheme ${FRAMEWORK_NAME} -sdk iphoneos -configuration ${CONFIGURATION} clean install CONFIGURATION_BUILD_DIR=${BUILD_DIR}/${CONFIGURATION}-iphoneos
```
This builds your framework for the device.

 

After creating the frameworks, let’s continue with your script.

Let’s clean up the final directories:
```shell

rm -rf "${UNIVERSAL_LIBRARY_DIR}"

mkdir "${UNIVERSAL_LIBRARY_DIR}"

mkdir "${FRAMEWORK}"

rm -rf "$OUTPUT_DIR"
mkdir -p "$OUTPUT_DIR"
 ```

Now, we take one of the framework files to our universal folder:
```shell
cp -r "${DEVICE_LIBRARY_PATH}/." "${FRAMEWORK}"
```
Note that here, we actually want all the framework files from the device product, but it’s the binary only that we need to merge.

 

Now for the real magic, ```lipo```, add this snippet:
```shell
lipo "${SIMULATOR_LIBRARY_PATH}/${FRAMEWORK_NAME}" "${DEVICE_LIBRARY_PATH}/${FRAMEWORK_NAME}" -create -output "${FRAMEWORK}/${FRAMEWORK_NAME}" | echo
cp -r "${FRAMEWORK}" "$OUTPUT_DIR"
```
That will merge the binaries of the simulator product framework and the device product simulator and copy the result to our output directory.

What we just created here is called a “fat” binary. That’s why the tool that extracts single architectures out of a fat binary (or adds architectures to a single binary) is called ```lipo```, as in “liposuction”.

 

Now we actually need to do the same with dYSMs files and add the result to that framework at the output directory.
```shell
cp -r "${DEVICE_DSYM_PATH}" "$OUTPUT_DIR"
lipo -create -output "$OUTPUT_DIR/${FRAMEWORK_NAME}.framework.dSYM/Contents/Resources/DWARF/${FRAMEWORK_NAME}" \
"${DEVICE_DSYM_PATH}/Contents/Resources/DWARF/${FRAMEWORK_NAME}" \
"${SIMULATOR_DSYM_PATH}/Contents/Resources/DWARF/${FRAMEWORK_NAME}" || exit 1
 ```

And this magic snippet:
```shell
UUIDs=$(dwarfdump --uuid "${DEVICE_DSYM_PATH}" | cut -d ' ' -f2)
for file in `find "${DEVICE_BCSYMBOLMAP_PATH}" -name "*.bcsymbolmap" -type f`; do
fileName=$(basename "$file" ".bcsymbolmap")
for UUID in $UUIDs; do
if [[ "$UUID" = "$fileName" ]]; then
cp -R "$file" "$OUTPUT_DIR"
dsymutil --symbol-map "$OUTPUT_DIR"/"$fileName".bcsymbolmap "$OUTPUT_DIR/${FRAMEWORK_NAME}.framework.dSYM"
fi
done
done
```
What this does is two things:

1. Copy the “.bcsymbolmap” file to our output directory.
2. Fix an issue where some symbols on your crash reports after symbolication appear hidden.

 

Here’s the full script:
```shell
FRAMEWORK_NAME="HelloLoggingFramework"

SIMULATOR_LIBRARY_PATH="${BUILD_DIR}/${CONFIGURATION}-iphonesimulator/${FRAMEWORK_NAME}.framework"

DEVICE_LIBRARY_PATH="${BUILD_DIR}/${CONFIGURATION}-iphoneos/${FRAMEWORK_NAME}.framework"

DEVICE_BCSYMBOLMAP_PATH="${BUILD_DIR}/${CONFIGURATION}-iphoneos"

DEVICE_DSYM_PATH="${BUILD_DIR}/${CONFIGURATION}-iphoneos/${FRAMEWORK_NAME}.framework.dSYM"
SIMULATOR_DSYM_PATH="${BUILD_DIR}/${CONFIGURATION}-iphonesimulator/${FRAMEWORK_NAME}.framework.dSYM"

UNIVERSAL_LIBRARY_DIR="${BUILD_DIR}/${CONFIGURATION}-iphoneuniversal"

FRAMEWORK="${UNIVERSAL_LIBRARY_DIR}/${FRAMEWORK_NAME}.framework"

OUTPUT_DIR="./HelloLoggingFramework-Aggregated"

Xcodebuild -project ${PROJECT_NAME}.Xcodeproj -scheme ${FRAMEWORK_NAME} -sdk iphonesimulator -configuration ${CONFIGURATION} clean install CONFIGURATION_BUILD_DIR=${BUILD_DIR}/${CONFIGURATION}-iphonesimulator

Xcodebuild -project ${PROJECT_NAME}.Xcodeproj -scheme ${FRAMEWORK_NAME} -sdk iphoneos -configuration ${CONFIGURATION} clean install CONFIGURATION_BUILD_DIR=${BUILD_DIR}/${CONFIGURATION}-iphoneos

rm -rf "${UNIVERSAL_LIBRARY_DIR}"

mkdir "${UNIVERSAL_LIBRARY_DIR}"

mkdir "${FRAMEWORK}"

rm -rf "$OUTPUT_DIR"
mkdir -p "$OUTPUT_DIR"

cp -r "${DEVICE_LIBRARY_PATH}/." "${FRAMEWORK}"

lipo "${SIMULATOR_LIBRARY_PATH}/${FRAMEWORK_NAME}" "${DEVICE_LIBRARY_PATH}/${FRAMEWORK_NAME}" -create -output "${FRAMEWORK}/${FRAMEWORK_NAME}" | echo
cp -r "${FRAMEWORK}" "$OUTPUT_DIR"

cp -r "${DEVICE_DSYM_PATH}" "$OUTPUT_DIR"
lipo -create -output "$OUTPUT_DIR/${FRAMEWORK_NAME}.framework.dSYM/Contents/Resources/DWARF/${FRAMEWORK_NAME}" \
"${DEVICE_DSYM_PATH}/Contents/Resources/DWARF/${FRAMEWORK_NAME}" \
"${SIMULATOR_DSYM_PATH}/Contents/Resources/DWARF/${FRAMEWORK_NAME}" || exit 1

UUIDs=$(dwarfdump --uuid "${DEVICE_DSYM_PATH}" | cut -d ' ' -f2)
for file in `find "${DEVICE_BCSYMBOLMAP_PATH}" -name "*.bcsymbolmap" -type f`; do
fileName=$(basename "$file" ".bcsymbolmap")
for UUID in $UUIDs; do
if [[ "$UUID" = "$fileName" ]]; then
cp -R "$file" "$OUTPUT_DIR"
dsymutil --symbol-map "$OUTPUT_DIR"/"$fileName".bcsymbolmap "$OUTPUT_DIR/${FRAMEWORK_NAME}.framework.dSYM"
fi
done
done
 ```

Now run  your aggregated Framework target and check your project directory.

![xcode](/img/2019-09-27/40.png){:class="img-responsive"}

Let’s go back to our TestApp:

* Remove the old framework.
* Add this new one to the Embedded Binaries section just like we did before.
* Make sure you ```Clean``` first from ```Product```, then run. 
* Try on both simulator and a device and check.
Simulator works!

iOS Device… well, it depends.

 

### Architectures

Remember when we talked about ```x86_64``` for simulator and ```arm64``` for device? Well ```arm64``` isn’t the only architecture in iOS devices. This is the architecture of iOS devices that have 64bit chips to support older devices like the iPhone 5. You need to support architecture ```armv7```, but if your deployment target is below iOS 11, it will not include it in the produced architectures.

So if you need to, do this:

Open your binary framework project ```HelloDemoApp.Xcodeproj```, then navigate to the project menu.

Choose ```HelloLoggingFramework``` from Targets.

Open the ```Build Settings``` tab and press ```All```.

![xcode](/img/2019-09-27/41.png){:class="img-responsive"}

Under Deployment, change the ```iOS Deployment Target``` to a version below iOS 11.

![xcode](/img/2019-09-27/42.png){:class="img-responsive"}

Amazing! ?

Now you have a fully production ready framework. That’s your canvas. Add the functionality you want and (hopefully) others want, and share it with them.

 

 

## Recapping What You Just Did
* You exported your binary framework from your development project.
* You built a test app.
* You integrated your binary framework in the test app.
* You created an aggregated target.
* You used LIPO to make your binary framework support both simulators and devices.
 

 

## One More Thing

If you upload your app to the Apple App Store, you will get an e-mail with a warning:

```"Too many symbol files" when submitting to App Store.```
That’s happening because your framework now has all the architectures (```armv7, arm64```) and your user app could just be supporting arm64. You will need to strip the extra architectures when uploading to iTunes.

If you published your framework with CocoaPods, CocoaPods will take care of the above for you. If not, you will have to add one extra step: adding a Run Script Phase to Build Phases, like what we did with the aggregated framework before, with the following script:
```shell
################################################################################
#
# Copyright 2015 Realm Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################

# This script strips all non-valid architectures from dynamic libraries in
# the application's `Frameworks` directory.
#
# The following environment variables are required:
#
# BUILT_PRODUCTS_DIR
# FRAMEWORKS_FOLDER_PATH
# VALID_ARCHS
# EXPANDED_CODE_SIGN_IDENTITY

# Signs a framework with the provided identity
code_sign() {
# Use the current code_sign_identitiy
echo "Code Signing $1 with Identity ${EXPANDED_CODE_SIGN_IDENTITY_NAME}"
echo "/usr/bin/codesign --force --sign ${EXPANDED_CODE_SIGN_IDENTITY} --preserve-metadata=identifier,entitlements $1"
/usr/bin/codesign --force --sign ${EXPANDED_CODE_SIGN_IDENTITY} --preserve-metadata=identifier,entitlements "$1"
}

echo "Stripping frameworks"
cd "${BUILT_PRODUCTS_DIR}/${FRAMEWORKS_FOLDER_PATH}"

for file in $(find . -type f -perm +111); do
# Skip non-dynamic libraries
if ! [[ "$(file "$file")" == *"dynamically linked shared library"* ]]; then
continue
fi
# Get architectures for current file
archs="$(lipo -info "${file}" | rev | cut -d ':' -f1 | rev)"
stripped=""
for arch in $archs; do
if ! [[ "${VALID_ARCHS}" == *"$arch"* ]]; then
# Strip non-valid architectures in-place
lipo -remove "$arch" -output "$file" "$file" || exit 1
stripped="$stripped $arch"
fi
done
if [[ "$stripped" != "" ]]; then
echo "Stripped $file of architectures:$stripped"
if [ "${CODE_SIGNING_REQUIRED}" == "YES" ]; then
code_sign "${file}"
fi
fi
done
 ```

 

## Summary
 

I hope you came eager to distribute your awesome idea that contains IP in nice packaging: iOS binary frameworks. Now you have the knowledge to go ahead and expand your development horizons. There’s more that you can create on the iOS platform than just apps! Good luck.

Nguồn:
> [link](https://instabug.com/blog/ios-binary-framework/)