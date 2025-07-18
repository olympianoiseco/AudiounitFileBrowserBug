# Audio Unit File Browser Bug Demo

This repository demonstrates a bug with UIDocumentBrowserViewController when used inside an Audio Unit extension.

## Bug Description

When attempting to present a UIDocumentBrowserViewController from within an AUv3 extension's view controller, the document browser may not behave correctly. 

Specifically, in Logic Pro, the UIDocumentBrowserViewController is unable to receive touch events when a keyboard is attached to the iPad. 

## How to Reproduce

1. **Build and Install the Extension**: 
   - Open `AUv3Filter.xcodeproj` in Xcode
   - Select the "AUv3Filter iOS" scheme (not the extension scheme)
   - Build and run on a physical iOS device (required for Audio Unit extensions)

2. **Test in a Host App**:
   - Have iPad Connected to keyboard case (I'm using the Apple brand keyboard / trackpad)
   - Open Logic for iPad
   - Load the "AUv3FilterDemo" audio unit 
   - Tap the "Show Folder Picker" button in the Audio Unit's interface

3. **Expected vs Actual Behavior**:
   - **Expected**: UIDocumentBrowserViewController presents and allows folder selection
   - **Actual**: UIDocumentBrowserViewController does not allow touches.  Disconnecting the keyboard restores the touches.  

## Demo App Overview

This is based on Apple's sample "Creating Custom Audio Effects" project with added UIDocumentBrowserViewController functionality. The sample app shows you how to create a custom audio effect plug-in using the latest Audio Unit standard (AUv3).

## Modified Components

The following components have been modified to demonstrate the file browser bug:

### AUv3FilterDemoViewController.swift
- Added `folderBrowserButton` outlet
- Added `showFolderBrowser(_:)` action method that presents UIDocumentBrowserViewController
- Added UIDocumentBrowserViewControllerDelegate extension to handle folder selection
- All file browser code is conditionally compiled for iOS only (`#if os(iOS)`)

### MainInterface.storyboard
- Added "Show Folder Picker" button connected to the new action method

## Project Structure

The project has targets for both iOS and macOS. Each platform's main app target has two supporting targets: `AUv3FilterExtension`, which contains the plug-in packaged as an Audio Unit extension, and `AUv3FilterFramework`, which bundles the plug-in's code and resources.

## Testing Notes

- This issue has only been demonstrated on iPadOS using a connected keyboard (Apple brand) while in Logic Pro and some other hosts.
- The bug does not appear in other hosts such as Loopy Pro or AUM
