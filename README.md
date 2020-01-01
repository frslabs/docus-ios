
# Docus SDK

![version](https://img.shields.io/badge/version-v1.0.0-blue)

Docus allows you to scan documents that doesn’t require OCR of the document. It can identify edges, crop document at its edges and collate them together as a single file that can then be stored in your server. 

You can find the release history at [Changelog](CHANGELOG.md)

# Table Of Content
- [Features](#Features)
- [Prerequisite](#prerequisite)
- [Requirements](#requirements)
- [Permission](#Permission)
- [Installation](#installation)
- [Usage example](#Usage-example)
- [Docus Result](#Docus-Result)
- [Docus Parameters](#Docus-parameters)
- [Docus Error Codes](#Docus-error-codes)
- [Help](#help)

## Features

- [x] Auto capture of documents
- [x] Scan any kind of document in seconds
- [x] Scan multi-page documents
- [x] Auto-crop scanned documents

## Prerequisites

You will need a valid license to use the Docus SDK, which can be obtained by contacting `support@frslabs.com` . 

Depending on the license - offline or online - you have opted for, the ping functionality to billing servers will be disabled or enabled. For instance, if you have opted for the offline SDK model, then there will be no server ping needed to our billing server to bill you. However, if you have chosen a transaction based pricing, then after each transaction, a ping request will be made to our billing server. This cannot be overrided by the App. A point to note is that if the ping transaction fails for any reason, the whole transaction will be void without any results from the SDK.

Once you have the license, follow the below instructions for a successful integration of Docus SDK onto your iOS Application

## Requirements

- iOS 10.0+
- Xcode 11.2

## Permission

In Info.plist file add following code to allow your application to access iPhone's camera:
``<key>NSCameraUsageDescription</key>
<string>Allow access to camera</string>``

## Installation

#### CocoaPods
You can use [CocoaPods](http://cocoapods.org/) to install `Docus` by adding it to your `Podfile`:

```ruby
platform :ios, '10.0'
use_frameworks!
pod 'Docus'
```

To get the full benefits import `Docus` wherever you import UIKit

``` swift
import UIKit
import Docus
```
#### Supported Tessdata installation
1. Download and drop ```model.trainneddata``` in tessdata folder of your project.
2. Congratulations! 

## Usage example

```swift
import Docus

let scanner = DocScannerController(delegate: self)
scanner.licenceKey = "Your Licence Key"
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
  
## Docus Error Codes

Error codes and their meaning are tabulated below

| Code          | Message                 |
| -------------- | ---------------------- |
| 805  | Licence expired             |
| 806  | Invalid licence             |

  
## Help

For any queries/feedback , contact us at `support@frslabs.com` 

