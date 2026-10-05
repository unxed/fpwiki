# macOS Monterey changes for developers

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 New features
    * 1.1 AppKit
    * 1.2 Mac Catalyst
    * 1.3 Networking
    * 1.4 Privacy
    * 1.5 Virtualisation
  * 2 Deprecations
  * 3 Monterey Release Notes



## New features

Monterey (macOS 12) was released 25 October 2021. 

### AppKit

  * NSButtons no longer highlight using the accent color when clicked. Before using any subclass that performs custom drawing, inspect the button cell’s _interiorBackgroundStyle_ property to determine if the bezel draws in a normal or emphasized state. (macOS 12)


  * NSButtons now support custom tinting with the _bezelColor_ property. This property — previously applied only in the Touch Bar — now functions for all in-window buttons using the _NSButtonTypeMomentaryPushIn_ style. (macOS 12)


  * NSSlider: When clicking on the track, for linear sliders, circular sliders, or dial, the slider now animates to the new value. You can now tint sliders with colorful track fills using the _trackFillColor_ property. This property now functions for all in-window sliders. (macOS 12)


  * NSSegmentedControls controls now support custom tinting via the _selectedSegmentBezelColor_ property. This property now functions for all in-window segmented control styles that draw colorful selected segments. (macOS 12)


  * NSPopover has a new animation when appearing and dismissing. (macOS 12)


  * SF Symbols now support layered symbol images. Layered symbol images use an updated data format to annotate each path element of the symbol with a level in a hierarchy: primary, secondary, tertiary, and so on. At draw time, AppKit can assign different colors to each layer of the symbol using new APIs on NSImage.SymbolConfiguration. Many system-provided symbol images redesigned to include layered versions. (macOS 12)


  * To enable secure coding for a restorable state, implement _applicationSupportsSecureRestorableState_. (macOS 12)


  * NSMenu: The new user preference _Automatically hide and show the menu bar in full screen_ in Dock & Menu Bar preferences is enabled by default. In macOS Monterey 12, the user can choose to disable this setting to always show the menu bar in full screen spaces. Standard full screen windows are resized to fit below the menu bar. Some applications assume a full screen-sized window, and don’t work well with this setting. (macOS 12)


  * NSTableView 20-point spacing precedes each group row to make the separation between sections more visible. Source lists have a similar, but smaller, 13-point spacing. This applies to the _NSTableView.Style.inset_ and _NSTableViewStyleFullWidth_ effective styles in applications linked against the macOS 12 SDK. _NSTableView.Style.plain_ doesn’t display the spacing. The floating group row transition — previously a push transition — instantly replaces the new floating group row. It occurs when the x-height of the group row’s label meets the bottom of a currently floating group row. (macOS 12)


  * NSOutlineView: The disclosure button now aligns with the cell view’s firstBaselineOffsetFromTop. The system overrides it in _NSTableCellView_ so it matches the baseline of its textField (assuming textfield sets the baseline). Overriding the property and returning 0 reverts to the previous behaviour. When collapsing several items, the _NSOutlineViewSelectionDidChangeNotification_ only posts once. Previously, it posted once per collapsed item. (macOS 12)


  * NSOpenPanel: Setting _canChooseFiles_ to false automatically disabled files, but all of the directory contents were sent to _panel(_:shouldEnable:)_ or _panel:shouldShowFilename:_. In macOS Monterey 12, files aren’t sent to the delegate methods unless _canChooseFiles_ is true. In addition, file packages are excluded unless _treatsFilePackagesAsDirectoriesis_ is true. This change allows optimization in the delegate method. (macOS 12)


  * TextKit 2 introduces a new text layout engine and its associated API. You can use TextKit 2 alongside the existing TextKit API. (macOS 12)



### Mac Catalyst

Users of iPad and iPhone apps running on a Mac with Apple silicon may enable or disable the Touch Alternatives feature via the application's preferences panel. The Touch Alternatives preference is automatically added to all iPad and iPhone apps when running on a Mac with Apple silicon. This feature makes it possible to use the keyboard to simulate swipes, taps, and drags, as well as device tilt; it also maps touches from the trackpad directly into the app when you hold down the Option key. 

Developers can automatically enable this feature for their app by default, by adding a properly configured `com.apple.uikit.inputalternatives.plist` to the application bundle. An onboarding dialog is shown on first launch in this case. 
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "<http://www.apple.com/DTDs/PropertyList-1.0.dtd>">
    <plist version="1.0">
    <dict>
       <key>defaultEnablement</key>
       <true/>
       <key>version</key>
       <real>1</real>
       <key>requiredOnboarding</key>
       <array> 
           <string>Tap</string>
           <string>Arrow Swipe</string>
           <string>Scroll Drag</string>
           <string>Tilt</string>
           <string>Trackpad Capture</string>
       </array>
    </dict>
    </plist>
    

Tailor the requiredOnboarding array to onboard the Touch Alternatives that only make the most sense for your app. While only the subset of features specified in the array is visually presented to the user during onboarding, all Touch Alternatives are active when enabled in this manner. (macOS 12) 

### Networking

  * The default Accept-Language header that URLSession sends has an updated format and corrected values for multiple locales. In addition to the preferred language, the header also includes the current system language as a fallback if it differs from the preferred language. This behavior affects apps that link against macOS 12. (macOS 12)


  * URLSession now includes async functions. (macOS 12)



### Privacy

  * With share current location, users can easily share their current location with an app just once, without giving the developer further access after that session. Developers can customize the share current location button, and integrate it directly into their apps. (macOS 12)


  * With enhanced Photos limited library access, developers can offer smart functionality — like a recent photos folder for specific albums — even when a user has only granted limited access. (macOS 12)


  * With secure paste, developers can let users paste from a different app without having access to what was copied until the user takes action to paste it into their app. When developers use secure paste, users will be able to paste without being alerted via the pasteboard transparency notification, helping give them peace of mind. (macOS 12)



### Virtualisation

  * Interfaces that previously took unsigned long long arguments or returned unsigned long long results have been updated to use the uint64_t type instead. (macOS 12)



## Deprecations

  * MKPinAnnotationView and MapPin are deprecated. (macOS 12)


  * Support for clear text HTTP URL schemes for Proxy Automatic Configuration (PAC) is now deprecated. Use only HTTPS URL schemes for PAC. This affects all PAC configurations including, but not limited to, configurations set via Settings, System Preferences, profiles, and URLSession APIs such as connectionProxyDictionary, and CFNetworkExecuteProxyAutoConfigurationURL(_:_:_:_:). If you configure a clear text HTTP PAC URL, the system may upgrade it to HTTPS during PAC file loads. Web Proxy Auto-Discovery (WPAD) Protocol via DNS isn’t affected. Dynamic Host Configuration Protocol (DHCP) Option 252 WPAD may attempt to upgrade clear text HTTP URLs to HTTPS during PAC file loads. (macOS 12)


  * If an application uses Python 2.7, macOS now triggers an alert indicating that the developer must update the app to ensure it will work in future versions of macOS. (macOS 12)


  * The output Combine publisher property for the message stream is deprecated. The PhotogrammetrySession.outputs property which uses the new Swift feature AsyncSequence replaces it. (macOS 12)


  * Fn (Function) shortcut modifier is deprecated and reserved for system usage. (macOS 12)


  * NSURLSession instance method streamTaskWithNetService is deprecated. (macOS 12)



## Monterey Release Notes

  * [Apple: macOS Release Notes](<https://developer.apple.com/documentation/macos-release-notes/>).
  * [Apple: AppKit Release Notes for macOS Monterey 12](<https://developer.apple.com/documentation/macos-release-notes/appkit-release-notes-for-macos-12>)
  * [Apple WWDC 202:1 What's new in AppKit](<https://www.wwdcnotes.com/notes/wwdc21/10054/>)

---

_Source: [https://wiki.freepascal.org/macOS_Monterey_changes_for_developers](https://web.archive.org/web/20241202161852/https://wiki.freepascal.org/macOS_Monterey_changes_for_developers)_
