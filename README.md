# ios_sdk
iOS integration SDK of https://www.admitad.com/

## Setting your project

Make sure that your project's deployment target is iOS 9.0 or higher.

Your project should be set up to make interaction with URL Schemes and Universal Links possible.

To add *AdmitadSDK* to your project you have two options:
1. Manual Installation
2. Installation via CocoaPods

### Manual Installation

*AdmitadSDK* makes use of *Alamofire* framework. In order to use AdmitadSDK please add Alamofire to your project.
The link below contains fully descriptive manual on Alamofire installation process:
[Alamofire GitHub Page](https://github.com/Alamofire/Alamofire)

To add *AdmitadSDK* itself please follow these steps:
1. Clone this repository or download zip-file.
2. Locate *RepoName/AdmitadSDK/AdmitadSDK* folder. This folder should contain two subfolders:  *Internal* and *Public*.
![Locating AdmitadSDK Directory](https://raw.githubusercontent.com/AdmitadSDK/ios_sdk/master/images/Directory.png)
3. Copy and paste the *AdmitadSDK* folder to some directory inside your project's directory.
4. Drag and drop the *AdmitadSDK* folder to Project Navigator in Xcode. Make sure that your target is checked in prompt.
![Drag and Drop](https://raw.githubusercontent.com/AdmitadSDK/ios_sdk/master/images/Drag.png)
![Checking Target](https://raw.githubusercontent.com/AdmitadSDK/ios_sdk/master/images/Target.png)
5. Build the project.

### Installation via CocoaPods

If you have CocoaPods installed (installation process is described [here](https://guides.cocoapods.org/using/getting-started.html)) do the following:
1. `cd` to the directory where your project is located.
2. run `pod init` in Terminal. A *Podfile* will be created.
3. Modify the *Podfile* to look like this:
```ruby
platform :ios, '9.0'
use_frameworks!

target '<Your Target>' do
pod 'AdmitadSDK'
end
```
4. Run `pod install`. A *.xcworkspace* will be created.
5. Close your project (if opened) and open the *.xcworkspace*.
6. If you've run into some issues installing *AdmitadSDK* via CocoaPods, try running `pod update` in Terminal.

## Alamofire version

*AdmitadSDK* uses version 4.x of *Alamofire* as a dependency. So if you use *Alamofire* in your project, it's major release number should be 4. You're free to specify any minor release number for your needs.

## Objective-C interoperability

Just add `@import AdmitadSDK;` import statement to the source files that make use of *AdmitadSDK*.

## Setting AppDelegate

1. In `application(_:didFinishLaunchingWithOptions:)` method assign *AdmitadTracker* singleton's `postbackKey` property to your Admitad Postback Key.
2. In the same method call *AdmitadTracker*'s `trackAppLaunch()`, `trackReturnedEvent()` и `trackLoyaltyEvent()`.
3. To track Universal Links usage, in `application(_:continue:restorationHandler:)` call corresondingly named `continueUserActivity` method and pass `userActivity` to it as parameter.
4. To track URL Schemes usage, in `application(_:open:options:)` call `openUrl` method and pass `url` to it as parameter.

## Event Tracking

If you have setup your *AppDelegate* right, *Installed* event is triggered automatically, *Returned* and *Loyalty* event are triggered when `trackReturnedEvent()` and `trackLoyaltyEvent()` respectively are called. To track *Confirmed Purchase* and *Paid Order* an *AdmitadOrder* object must be instantiated and passed as parameter to `trackConfirmedPurchaseEvent` or `trackPaidOrderEvent` respectively.

Methods `trackRegisterEvent()`, `trackReturnedEvent()` and `trackLoyaltyEvent()` can take user ID as parameter. Optionally you can setup *AdmitadTracker* singleton's `userId` property. If you prefer not to provide user ID in any of these ways, user ID will be generated automatically.

## Delegation and Callbacks

When working with *AdmitadSDK* from Swift environment you are provided with two mechanisms of notification on event tracking success or failure. Every event triggering method takes a completion callback. In the callback you can check if an error has occurred. *AdmitadTracker* instance also notifies its *delegate* object conforming to *AdmitadDelegate* protocol. Both ways of notification are independent from each other and can be used simultaneously and interchangeably. In Objective-C environment only the former mechanism is available.
