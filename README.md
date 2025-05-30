
# DOCUS iOS SDK

![version](https://img.shields.io/badge/version-v1.4.4-blue)

Docus allows you to scan documents that doesn’t require OCR of the document. It can identify edges, crop document at its edges and collate them together as a single file that can then be stored in your server. 

You can find the release history at [Changelog](CHANGELOG.md)

‼ ATTENTION ‼ → BREAKING CHANGE introduced at Docus SDK `v1.3.6`. We have introduced a new license format. If you are using versions prior to `v1.3.6` and intend to update to `v1.3.6` or above please contact `support@frslabs.com` for an updated license.

# Table Of Content
- [Features](#Features)
- [Prerequisites](#Prerequisites)
- [Minimum Requirements](#Minimum-Requirements)
- [Permission](#Permission)
- [Installation](#Installation)
- [Usage example](#Usage-example)
- [Docus Result](#Docus-Result)
- [Docus Parameters](#Docus-parameters)
- [Docus Error Codes](#Docus-error-codes)
- [Help](#help)

## Features

-  Auto capture of documents - The SDK will detect document edges and automatically capture the document image.
-  Auto-crop scanned documents - The SDK automatically crops the image once the edges are detected. The SDK also allows the user to manually crop if needed.
-  Scan any document - The SDK will allow the user to scan any document placed within the bounding box of the scanner.
-  Scan multi-page documents - The user can scan multiple pages and the SDK will automatically collate the images into a folder in application document directory.

## Prerequisites

You will need a valid license and Netrc credentials to use the Docus SDK, which can be obtained by contacting support@frslabs.com.

Depending on the license - offline or online - you have opted for, the ping functionality to billing servers will be disabled or enabled. For instance, if you have opted for the offline SDK model, then there will be no server ping needed to our billing server to bill you. However, if you have chosen a transaction based pricing, then after each transaction, a ping request will be made to our billing server. This cannot be overrided by the App. A point to note is that if the ping transaction fails for any reason, the whole transaction will be void without any results from the SDK.

Once you have the license, follow the below instructions for a successful integration of Docus SDK onto your iOS Application.

## Minimum Requirements

- Xcode 13.0
- iOS 13.0+
- Swift 5.0

## Permission

In Info.plist file add following code to allow your application to access iPhone's camera:
``<key>NSCameraUsageDescription</key>
<string>Allow access to camera</string>``

## Installation

#### CocoaPods
You can use [CocoaPods](http://cocoapods.org/) to install `Docus` by adding it to your `Podfile`:

```ruby
source 'https://gitlab.com/frslabs-public/ios/docus_ios.git'
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '12.0'
target '<Your Target Name>' do
use_frameworks!
pod 'Docus', '1.4.4'
end
```
###### Save/Edit Netrc settings to install custom pod

You will need a valid netrc credentials to install docus from maven, which can be obtained by contacting `support@frslabs.com`. 

1. Create or edit .netrc file under current user's home directory
2. Write the below lines into that file, replace <YOUR_USERNAME> and <YOUR_PASSWORD> with your credentials which is shared through email and save the file.
```ruby
machine www.repo2.frslabs.space
login <YOUR_USERNAME>
password <YOUR_PASSOWRD>
```
3. In terminal enter below command to install the pod 

    ```bash
    $ pod install  (or) $ pod update  (or)  $ pod install --repo-update
    ```
4. Connect with physical device to build and run Docus, It will not build/run in simulator due to camera dependency.


To get the full benefits import `Docus` wherever you import UIKit

``` swift
import UIKit
import Docus
``` 

## Usage example

```swift
import Docus

let scanner = DocScannerController(delegate: self)
scanner.licenceKey = "Your Licence Key"
scanner.modalPresentationStyle = .fullScreen
scanner.noOfPages = "no of pages needed"   // maximum digits is 19 
present(scanner, animated: true)
    
```
#### Handling the result

```swift
class  ViewController: UIViewController, DocScannerControllerDelegate {
    func docScanner(_ scanner: DocScannerController, didFinishScanningWithResults results: docScannerResults) {
        print("DocumentsResult: ", results.savedImages)
        scanner.dismiss(animated:  true, completion:  nil)
    }
    func docScanner(_ scanner: DocScannerController, didCancel cancel: String) {
        scanner.dismiss(animated:  true, completion:  nil)
    }
    
    func docScanner(_ scanner: DocScannerController, didFailWithError error: String) {
        print("ErrorCode: ", error)
        scanner.dismiss(animated: true, completion: nil)
    }
}
``` 

## Docus Result

```swift
let images = getImageFromDocumentDirectory(docScanImgs: results.savedImages)
resultImages.append(images)

func getImageFromDocumentDirectory(docScanImgs : [String]) -> [UIImage] {
    let fileManager = FileManager.default
    var docusImages = [UIImage]()
    for eachImg in docScanImgs {
        let fileArray = eachImg.components(separatedBy: "/")
        let finalFileName = fileArray.last
        let path = (NSSearchPathForDirectoriesInDomains(.documentDirectory, .userDomainMask, true)[0] as NSString).appendingPathComponent("docus_images")
        let url = NSURL(string: path)
        let imagePath = (url!).appendingPathComponent(finalFileName!)
        let urlString: String = imagePath!.absoluteString
        if fileManager.fileExists(atPath: urlString) {
            let image = UIImage(contentsOfFile: urlString)
            docusImages.append(image!)
        } else {
             print("No Image")
        }
    }
    return docusImages
}
     
```
## Docus Parameters

- `scanner.licenceKey = "LICENCE KEY"`   ***(Required)***
  
  Accepts the Docus licence key as a `String`
  
- `scanner.noOfPages = "no of pages needed"`   ***(Required)***
  
  Accepts number of pages upto 19 digits as a `Int`
  
## Docus Error Codes

Error codes and their meaning are tabulated below

| CODE | DESCRIPTION                  |
| ---- | ---------------------------- |
| 801  | Invalid Page limit    |
| 803  | Camera permission denied    |
| 804  | Docus interrupted            |
| 805  | Docus SDK License has expired             |
| 807 | Unable to unlock pdf document |
| 806  | Docus SDK License is invalid             |
| 901  | No internet connection           |
| 902  | Image upload failed             |
| 903  | Transaction failed / Unable to ping  |



  
## Help

For any queries/feedback , contact us at `support@frslabs.com` 

