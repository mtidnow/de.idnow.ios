# SDK
# Table of Contents

- [Requirements](#requirements)
- [Installation](#installation)
  - [CocoaPods](#cocoapods)
  - [xCode9](#xcode9)
  - [Manually](#manually)
- [Settings](#settings)
  - [transactionToken](#transactiontoken)
  - [companyID](#companyid)
  - [environment](#environment)
  - [showErrorSuccessScreen](#showerrorsuccessscreen)
  - [showVideoOverviewCheck](#showvideooverviewcheck)
  - [forceModalPresentation](#forcemodalpresentation)
  - [modalPresentationStyle](#modalpresentationstyle)
  - [apiHost](#apihost)
  - [websocketHost](#websockethost)
  - [connectionType](#connectiontype)
- [Branding](#branding)
  - [Colors](#colors)
    - [defaultTextColor](#defaulttextcolor)
    - [primaryBrandColor](#primarybrandcolor)
    - [proceedButtonBackgroundColor](#proceedbuttonbackgroundcolor)
    - [proceedButtonTextColor](#proceedbuttontextcolor)
    - [photoIdentRetakeButtonBackgroundColor](#photoidentretakebuttonbackgroundcolor)
    - [photoIdentRetakeButtonTextColor](#photoidentretakebuttontextcolor)
    - [textFieldColor](#textfieldcolor)
    - [failureColor](#failurecolor)
    - [successColor](#successcolor)
    - [CallQualityCheckScreen](#callqualitycheckscreen)
    - [cqcOuterRingColor](#cqcouterringcolor)
    - [proceedButtonTextColor](#proceedbuttontextcolor)
    - [cqcDefaultInnerRingColor](#cqcdefaultinnerringcolor)
    - [cqcPoorQualityInnerColor](#cqcpoorqualityinnercolor)
    - [cqcModerateQualityInnerColor](#cqcmoderatequalityinnercolor)
    - [cqcExcellentQualityInnerColor](#cqcexcellentqualityinnercolor)
  - [StatusBar](#statusbar) 
    - [enableStatusBarStyleLightContent](#enablestatusbarstylelightcontent)
  - [Fonts](#fonts) 
    - [fontNameRegular](#fontnameregular)
    - [fontNameMedium](#fontnamemedium)
    - [fontNameLight](#fontnamelight)
    - [fontNameRegular](#fontnameregular)
- [PushNotifications](#pushnotifications)
- [Usage](#usage)
- [Localization](#localization)
  - [Example](#example) 
  
## Requirements
- Xcode 8.0 or later
- Deployment Target: iOS 8.0 or later
- Supported Devices: iPhone (4s + later), iPod Touch (5 + later), iPad (2 + later)
- Cocoapods installed
- Device with Wifi / 3G / LTE

## Installation

### CocoaPods
- Add the following pod dependencies to your podfile:
```
pod 'IDnowSDK'
```

- Then, run the following command:
```
pod install
```

- Import SDK by using "@import IDnowSDK"

### xCode9
As of XCode 9 there might be a chance that you experience a problem with your host apps AppIcon in case you use CocoaPods.
One of the symptoms is that the AppIcon will not be visible if you run your app either on the simulator or a real device. 
There are many reasons why assets/resources are not present (wrong format, transparency, wrong size, ...) but one might be that the auto generated shell script which builds the pods resources misses a flag telling actool the name of the app-icon. 

If you encounter this try to add the following to your projects Podfile exchanging <YOUR_PRODUCT> with the name of your project:

```
post_install do |installer|
    copy_pods_resources_path = "Pods/Target Support Files/Pods-<YOUR_PRODUCT>/Pods-<YOUR_PRODUCT>-resources.sh"
    string_to_replace = '--compile "${BUILT_PRODUCTS_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}"'
    assets_compile_with_app_icon_arguments = '--compile "${BUILT_PRODUCTS_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}" --app-icon "${ASSETCATALOG_COMPILER_APPICON_NAME}" --output-partial-info-plist "${BUILD_DIR}/assetcatalog_generated_info.plist"'
    text = File.read(copy_pods_resources_path)
    new_contents = text.gsub(string_to_replace, assets_compile_with_app_icon_arguments)
    File.open(copy_pods_resources_path, "w") {|file| file.puts new_contents }
end
```

### Manually 
- Add the following pod dependencies to your podfile:
```
pod 'AFNetworking', '~> 4.0.1'
pod 'FLAnimatedImage', '~> 1.0'
pod 'SocketRocket', '~> 0.5.1'
pod 'Masonry', '~> 1.1.0'
pod 'libPhoneNumber-iOS', '~> 0.9'
```
- Download the current release from and copy the idnow-sdk folder to your project directory
- Or add the repo as a git submodule (git lfs required. For the initial checkout do git lfs pull)
- Drag idnow-sdk folder into your Xcode project
- Add to your "Link binary with libraries" section
```
AudioToolbox.framework
VideoToolbox.framework
AVFoundation.framework
CoreMedia.framework
GLKit.framework
OpenGLES.framework
SystemConfiguration.framework
Webkit.framework
StoreKit.framework
Accelerate.framework
```   
- import 'IDnowSDK.h' // --> Objective-C Project
- or import IDnowSDK // --> Swift Project

__Note__: To get the sample projects work, you have to call "pod install" to install dependencies.

## Settings
The settings that should be used for the identification process provided by IDnow.

#### transactionToken
A token that will be used for instantiating a photo or video identification.

#### companyID
Your company id provided by IDnow.

#### environment
Optional: The environment that should be used for the identification (DEV, TEST, LIVE)
The default value is `IDnowEnvironmentNotDefined`. 
The used environment will then base on the prefix of the transaction token (DEV -> DEV, TST -> Test, else -> Live).
You can use the special IDnowEnvironmentCustom to define a custom IDnow installation. If this is done, you need to set the apiHost and websocketHost.

#### showErrorSuccessScreen
Optional: If set to `false`, the Error-Success-Screen provided by the SDK will not be displayed.
The default value of this property is `true`.

#### showVideoOverviewCheck
Optional: If set to `false`, the video overview check screen will not be shown befsore starting a video identification.
The default value of this property is `true`.

#### forceModalPresentation
Optional: If set to `true`, the UI for the identification will always be displayed modal. 
By default the value of this property is `false` and the identification UI will be pushed on an existing navigation controller if possible.

#### modalPresentationStyle
Optional: Specifies the presentation style for the modal ident viewcontroller.
E.g. Can be set to `UIModalPresentationCurrentContext` to allow presenting ident view controller within a popover on an iPad.
 
#### apiHost
The target server url for REST calls if custom server is used.

#### websocketHost
The target server url for websocket calls if custom server is used.

#### connectionType
The connection type to use to talk the backend. (Websocket (default) or long polling)

## Branding
Warning: Branding is only allowed if you have the permissions from IDnow.

### Colors

#### defaultTextColor
Optional color, that replaces the default text color.
Default: A nearly black color
Recommendation: Should be some kind of a dark color that does not collide with white color.

#### primaryBrandColor
Optional color, that replaces the default brand color.
Default: defaultTextColor
Used in headlines, checkboxes, links, alerts etc.
Recommendation: Should be a color that does not collide with white color.

#### proceedButtonBackgroundColor
Optional color, that replaces the proceed button background color.
Default: An orange color

#### proceedButtonTextColor
Optional color, that replaces the proceed button text color.
Default value: White color

#### photoIdentRetakeButtonBackgroundColor
Optional color, that replaces the photo ident retake button background color.
Default value: defaultTextColor

#### photoIdentRetakeButtonTextColor
Optional color, that replaces the photo ident retake button text color.
Default value: proceedButtonTextColor

#### textFieldColor
Optional color, that replaces the default color of textfield backgrounds and borders
Default: defaultTextColor

#### failureColor
Optional color, that replaces the text color in the result screen, when an identification failed.
Default: A red color

#### successColor
Optional color, that replaces the text color in the result screen, when an identification was successful.
Default: A green color

#### CallQualityCheckScreen

#### cqcOuterRingColor
Optional color that replaces default dark gray for the outer ring indicator on the quality check screen.
Default: dark gray

#### cqcDefaultInnerRingColor
Optional color that replaces default light gray for the inner ring indicator on the quality check screen.
Default: light gray

#### cqcPoorQualityInnerColor
Optional color that replaces default bright red for the inner ring indicator in case bad network quality on the quality check screen.
Default: bright red

#### cqcModerateQualityInnerColor
Optional color that replaces default bright orange for the inner ring indicator in case moderate network quality on the quality check screen.
Default: bright orange

#### cqcExcellentQualityInnerColor
Optional color that replaces default strong yellow for the inner ring indicator in case excellent network quality on the quality check screen.
Default: strong yellow (almost green).

### StatusBar

#### enableStatusBarStyleLightContent
Optional: Forces the light status bar style to match dark navigation bars.
If you tint your navigation bar with a dark color by adjusting navigation bar appearance (e.g. a blue color) you can set this value to true. The statusbar style will then be adjusted to light in screens where the navigation bar is visible.


### Fonts

#### fontNameRegular
An optional font name that can be used to replace the regular font used by the SDK.
Default: System Font: Helvetica Neue Regular (< iOS 9), San Francisco Regular (>= iOS 9)

#### fontNameMedium
An optional font name that can be used to replace the medium font used by the SDK.
Default: System Font: Helvetica Neue Medium (< iOS 9), San Francisco Medium (>= iOS 9)

#### fontNameLight
An optional font name that can be used to replace the light font used by the SDK.
Default: System Font: Helvetica Neue Light (< iOS 9), San Francisco Light (>= iOS 9)

## PushNotifications

In order to use push notifications via the IDnow SDK it is neccessary that your own AppDelegate inherits from 
the provided IDnowAppDelegate. This is neccessary since the callbacks form Apple concerning registration and 
reception of push notifications is soley handled through the AppDelegate which is not part of our SDK. In case your
own AppDelegate implements interfaces present in the IDnow SDK please make sure to make a 
call the super classes (IDnowAppDelegate) implementation as well.

Additionally we will need the production certifcate/key pair to send notifications via push to your app via
our backend.

```objective-c

// header
@interface YourAppDelegate : IDnowAppDelegate

@end

// implementation
@implementation YourAppDelegate

- (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
{
    [super application:application didFailToRegisterForRemoteNotificationsWithError:error];
}

- (void)application:(UIApplication *)application didRegisterUserNotificationSettings:(UIUserNotificationSettings *)notificationSettings
{
	[super application:application didRegisterUserNotificationSettings:notificationSettings];
}

- (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(nonnull NSData*)deviceToken
{
	[super application:application didRegisterForRemoteNotificationsWithDeviceToken:deviceToken];
}

- (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(nullable NSDictionary *)launchOptions
{
	[super application:application didFinishLaunchingWithOptions:launchOptions];
}

- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
{
	[super application:application didReceiveRemoteNotification:userInfo];
}

- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
                                                       fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))completionHandler
{
	[super application:application didReceiveRemoteNotification:userInfo
										 fetchCompletionHandler:completionHandler];
}

@end


```

## Usage

```objective-c

// Setup IDnowAppearance
IDnowAppearance *appearance = [IDnowAppearance sharedAppearance];
    
// Adjust colors
appearance.defaultTextColor = [UIColor blackColor];
appearance.primaryBrandColor = [UIColor blueColor];
appearance.proceedButtonBackgroundColor = [UIColor orangeColor];
appearance.failureColor = [UIColor redColor];
appearance.successColor = [UIColor greenColor];
    
// Adjust statusbar
appearance.enableStatusBarStyleLightContent = YES;
    
// Adjust fonts
appearance.fontNameRegular = @"AmericanTypewriter";
appearance.fontNameLight = @"AmericanTypewriter-Light";
appearance.fontNameMedium = @"AmericanTypewriter-CondensedBold";

// To adjust navigation bar / bar button items etc. you should follow Apples UIAppearance protocol.

// Setup IDnowSettings
IDnowSettings *settings = [IDnowSettings settingsWithCompanyID:@"yourCompanyIdentifier"];
settings.transactionToken = @"DEV-TXTXT";

// Initialise and start identification
IDnowController *idnowController = [[IDnowController alloc] initWithSettings: settings];

// Initialize identification using blocks 
// (alternatively you can set the delegate and implement the IDnowControllerDelegate protocol)
[idnowController initializeWithCompletionBlock: ^(BOOL success, NSError *error, BOOL canceledByUser)
{
		if ( success )
		{
		      // Start identification using blocks
			  [idnowController startIdentificationFromViewController: self 
			  withCompletionBlock: ^(BOOL success, NSError *error, BOOL canceledByUser)
			  {
					  if ( success )
					  {
					      // identification was successfull
					  }
					  else
					  {
					      // identification failed / canceled
					  }
				}];
		}
		else if ( error )
		{
		      // Present an alert containing localized error description
			  UIAlertController *alertController = [UIAlertController alertControllerWithTitle: @"Error" 
			  message: error.localizedDescription 
			  preferredStyle: UIAlertControllerStyleAlert];
			  UIAlertAction *action = [UIAlertAction actionWithTitle: @"Ok" 
			  style: UIAlertActionStyleCancel 
			  handler: nil];
			  [alertController addAction: action];
			  [self presentViewController: alertController animated: true completion: nil];
		}
	}];
```

You can also change some of the optional settings:

```objective-c
// Optionally disable success and error screens
settings.showErrorSuccessScreen = NO;
settings.showVideoOverviewCheck = NO;
settings.forceModalPresentation = YES;
settings.showIdentTokenOnCheckScreen = YES;

// Optionally enable custom server with long polling
settings.environment = IDnowEnvironmentCustom;
settings.apiHost = @"https://api.yourserver.com";
settings.websocketHost = @"https://websocket.yourserver.com";
settings.connectionType = IDnowConnectionTypeLongPolling;
```

## Localization
Warning: Adapting localizations is only allowed if you have the permissions from IDnow.

In case you would like to change the localization used by the IDnow SDK at runtime you can do:
```
settings.userInterfaceLanguage = @"de"; // this field accepts the following languages (de,en,it,es,pt)
```

English and German Localizations are provided by the SDK (IDnowSDKLocalization.bundle)
You can overwrite localisation in your own Localizable.strings files.


### Example
```
//Defined in the IDnowSDKLocalization.bundle
"NAVIGATION_ITEM_TITLE_DEFAULT" = "IDnow";

//Overwrite in your Localizable.strings file:
"NAVIGATION_ITEM_TITLE_DEFAULT" = "Video Ident";
```
