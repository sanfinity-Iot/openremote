The OpenRemote Mobile Apps render the consoles (user interface of an OpenRemote application) using a web view and provide bridging of native device functionality to provide a native app experience. 

## Android Console

Download and install [Android Studio](https://developer.android.com/studio/index.html), then open the `console/android` project. 

TODO: There is actually nothing to build right now, you have to create a custom project and a dependency on our Android console project from your own Android app. 

## iOS Console

### Developing for iOS is only possible on macOS.

Download and install [Xcode](https://itunes.apple.com/nl/app/xcode/id497799835), go to `console/iOS` and open the `AppTemplate.xcodeproj`.

When using Firebase, download the GoogleService-Info.plist and replace the existing one.

Set the right values in ProjectConstants, for e.g. the correct server url.
When ready, close Xcode and run the `appGenerator.py` from a terminal and follow the instructions. For more information about the python script, read the `README.md` in the `console/iOS` folder.

Finally go to your project folder and run `pod install` from a terminal to download the needed libraries.
You'll see `console.xcworkspace`. Open this file from now on to work on your iOS app.
