The OpenRemote mobile consoles load the web applications using a web view and provide bridging of native device functionality to provide a native app experience. Make sure you've pulled the latest code of the repository.

## Android Console

Download and install [Android Studio](https://developer.android.com/studio/index.html), then open the `console/android` project. 

TODO: There is actually nothing to build right now, you have to create a custom project and a dependency on our Android console project from your own Android app. 

## iOS Console

### Developing for iOS is only possible on macOS.

Download and install [Xcode](https://itunes.apple.com/nl/app/xcode/id497799835)

Open Xcode and create a new project. In the generals part, click on the plus symbol to add a new target. Select Notification Service Extension. Close the project. 


Install cocoapods through a terminal window.
```
sudo gem install cocoapods
```
Navigate to your project directory and create a pod file.
```
pod init
```

Open up the Podfile with a text editor. Add the `ORLib` pod to both of the targets
```
workspace '<your_project>'
platform :ios, '11.2'

use_frameworks!

def shared_pods
  pod 'Firebase/Core', '~> 4.6.0'
  pod 'Firebase/Messaging', '~> 4.6.0'
  pod 'Fabric', '~> 1.10.2'
  pod 'Crashlytics', '~> 3.13.4'
end


target '<your_project>' do
  project '<your_project>'
  shared_pods
end

target 'NotificationService' do
  project '<your_project>'
  shared_pods
end

target 'ORLib' do
  project 'path_to/openremote/console/iOS/ORLib/ORLib'
  shared_pods
end

```
Save and close the Podfile.
In the terminal enter the following command
```
pod install
```
A xcworkspace file is created after installing the pod. Open this file and Xcode will start.

Click on the `Pods` icon in the project tree and then on ORLib in the targets pane. Search for Require Only App-Extension-Safe API and set it to `No`. A warning will appear which can be ignored.

Open `AppDelegate` in your project and make it inherit from `ORAppDelegate`. Remove all the code and override `applicationDidFinishLaunchingWithOptions`. Set the right project values.
```
override func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {

        ORServer.hostURL = "example.com"
        ORServer.realm = "example"

        ORAppGroup.entitlement = "group.io.openremote.example"
        
        return super.application(application, didFinishLaunchingWithOptions: launchOptions)
    }
```

In the `Main.storyboard` add a second ViewController. The initial viewcontroller should be of class ORLoginViewController. The other ViewController should be of class ORViewController.

In the Notification Service Extension target, open `NotificationViewController` and make it inherit from `ORNotificationService`. Remove all the code inside.

When using Firebase, download the GoogleService-Info.plist and add it through Xcode. It shuld be placed in the root of the project. Make sure that `copy when needed` is checked when adding.

Now your iOS app is setup to work with your OpenRemote project!
