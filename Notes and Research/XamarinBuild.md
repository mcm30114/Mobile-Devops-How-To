# Xamarin Build Config for Visual Studio 2015

## Tools
* Android SDK
* Visual Studio 2015
* Mac with Xcode

## Android Steps

* Install Xamarin Components to Visual Studio
	1. On VS, go to Tools, Extensions and Updates.
	2. Search for Xamarin and Install.
	3. Can also be triggered by attempting to create an Android project on VS.

* Make sure that the SDK Tools are installed.
	1. After creating an Android project, open the Android SDK Manager from the VS toolbar.
	2. On the components, install the SDK-tools if they aren't installed.

* Where to test?
	1. You can create a virutal device (AVD) from the Android Emulator Manager on the toolbar.
	2. You can also connect a device and test using a device.

* Testing on a device
	1. Make sure that the device has Developer mode activate. You can activate from the Settings menu.
	2. On Settings, go to About -> Software Information -> More
	3. Tap on Build number repeatedly until you get Developer Mode activated.
	4. Connect the device and allow it to be recognized by the PC.
	5. To validate that it has been verified open the Android ADB Command Prompt from the VS toolbar.
	6. Use the command "adb devices" and validate that your device appears on the list.

* Run
	1. Once everything has been validated, you can run the application directly from VS.
	2. Open the drop-down on the right side of the Run button.
	3. Select the connect device to deploy and test.

## iOS Steps