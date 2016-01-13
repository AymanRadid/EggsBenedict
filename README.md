# EggsBenedict

[![Build Status](https://travis-ci.org/JPMartha/EggsBenedict.svg)](https://travis-ci.org/JPMartha/EggsBenedict) [![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage) [日本語](./Documentation/ja/)

__EggsBenedict__ is a library for sharing picture with Instagram app written in Swift.

<img src="./Images/EggsBenedict.gif" width=272>

This library is following Instagram's sharing flow.

> __Instagram's documentation__

> - [Document Interaction](https://www.instagram.com/developer/mobile-sharing/iphone-hooks/#document-interaction)

If the custom URL schemes `instagram://` can be opened direct users on the iOS device, the flow is as follows.

1. Save temporary image file named  `jpmarthaeggsbenedict` (JPEG format) in `tmp/` directory using the filename extension `.ig` or `.igo`.
2. Display the menu for copying to Instagram app.
3. If users tap the "Copy to Instagram" icon, open Instagram app with its filter screen.

  > The image is preloaded and sized appropriately for Instagram. For best results, Instagram prefers opening a JPEG that is 640px by 640px square. If the image is larger, it will be resized dynamically.

#### _\- By the way, why was it named "EggsBenedict"?_

The reason is because I like Eggs Benedict 😋

## Availability

- Swift 2.1
- Xcode 7.2
- iOS 8.0 and later

## Adding EggsBenedict.framework to your project

#### [Carthage](https://github.com/Carthage/Carthage) (preferred)

1. Create a [Cartfile](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#cartfile), and add `github "JPMartha/EggsBenedict" ~> 0.9.7`.
2. Run `$ carthage update --platform iOS` in your project directory.
3. On your application targets’ “Build Phases” settings tab, in the “Link Binary With Libraries” section, click the “+” icon and add `EggsBenedict.framework` from the Carthage/Build folder on disk.
4. On your application targets’ “Build Phases” settings tab, click the “+” icon and choose “New Run Script Phase”. Create a Run Script with the following contents: 
  ```
  /usr/local/bin/carthage copy-frameworks
  ```
  and add the "Input Files" to EggsBenedict.framework:
  ```
  $(SRCROOT)/Carthage/Build/iOS/EggsBenedict.framework
  ```
  
  This script works around an [App Store submission bug](http://www.openradar.me/radar?id=6409498411401216) triggered by universal binaries and ensures that necessary bitcode-related files are copied when archiving.
  
#### [CocoaPods](https://cocoapods.org)

1. Create a [Podfile](https://guides.cocoapods.org/using/the-podfile.html), and add the following contents:

  ```
  use_frameworks!
  pod 'EggsBenedict', '~> 0.9.7'
  ```
  
2. Run `$ pod install` in your project directory.

## Getting started

1. On your application Info.plist, add `LSApplicationQueriesSchemes` key.

  Key                                           |Type    |Value
  ------------------------------------|--------|-----------
  LSApplicationQueriesSchemes | Array | instagram

2. Create an instance of the `SharingFlow` class with the `SharingFlowType` enumeration.

  ```swift
  let sharingFlow = SharingFlow(type: .IGOExclusivegram)
  ```
  
  For more information, see [SharingFlow Class Reference](/Documentation/SharingFlowClassReference.md).

3. Call the `presentOpenInMenuWithImage:inView:documentInteractionDelegate:completion:` method with two required parameters and two optional parameters.

  ```swift
  sharingFlow.presentOpenInMenuWithImage(YourImage, inView view: YourView, documentInteractionDelegate: nil) { (result) -> Void in
      // Handling Errors
  }
  ```
  
  - Handling Errors Example
    
    ```swift
    switch result {
    case .Success(let imagePath):
        print("Success: \(imagePath)")
    case .Failure(let error):
        print("Error: \(error)")
    }
    ```

For more information, see [SharingFlow Class Reference](/Documentation/SharingFlowClassReference.md).

## Remove temporary image file

To remove temporary image file in `tmp/` directory, call the `removeTemporaryImage:` method of the created instance.

```swift
sharingFlow.removeTemporaryImage { (result) -> Void in
    // Handling Errors
}
```
  
  - Handling Errors Example
  
  ```swift
  switch result {
  case .Success(let imagePath):
      print("Success: \(imagePath)")
  case .Failure(let error):
      print("Error: \(error)")
  }
  ```

For more information, see [SharingFlow Class Reference](/Documentation/SharingFlowClassReference.md).

## Documentation

- [EggsBenedict Framework Reference](/Documentation)

## License

__EggsBenedict__ is released under the [MIT License](LICENSE).
