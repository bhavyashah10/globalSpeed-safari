# Global Speed Extension for Safari

A Safari port of [Global Speed](https://github.com/polywock/globalSpeed) - a powerful browser extension for controlling video/audio playback speed and audio effects.

> **Note**: This is an unofficial Safari port for personal use. All credit goes to the original author [polywock](https://github.com/polywock). Please support the original project!


## Prerequisites

- macOS (required for Safari extension development)
- Xcode (free from Mac App Store)
- Node.js and npm
- Basic command line knowledge

## Installation Steps

### 1. Install Xcode

1. Open the Mac App Store
2. Search for "Xcode" and install it
3. Open Xcode once to accept the license agreement

### 2. Directly use `build/unpacked` from this Repository

OR

### 2. Clone the Original Global Speed Repository and Install Dependencies and Build

```bash
git clone https://github.com/polywock/globalSpeed.git
cd globalSpeed

npm install
npm run build:prod
```

This will create a `build/unpacked` folder with the compiled extension.

### 3. Convert to Safari Extension

Use Apple's conversion tool to create a Safari-compatible extension:

```bash
xcrun safari-web-extension-converter build/unpacked --app-name "Global Speed"
```

**You'll see warnings about unsupported APIs** - this is expected. Some Chrome-specific features may not work in Safari:
- `offscreen` - Advanced audio processing
- `tabCapture` - Tab audio capture
- `userScripts` - Some content script features

### 5. Open the Xcode Project

It will open automatically, if it doesn't use this

```bash
cd "Global Speed"
open "Global Speed.xcodeproj"
```

### 6. Configure Code Signing

In Xcode:

1. Click on **"Global Speed"** in the left sidebar (blue project icon)
2. Select **"Global Speed"** target
3. Go to **"Signing & Capabilities"** tab
4. Under **Team**, select your Apple ID (add account if needed)
5. Select **"Development"**
6. Check **"Automatically manage signing"**
7. **Repeat for "Global Speed Extension" target**

**Important**: Make sure both targets are set to **macOS only** (not iOS). If you see iOS errors:
- Look for "Supported Destinations" 
- Uncheck iOS and iPad, keep only macOS

### 7. Handle Keychain Popups

When building, macOS will ask for your password multiple times:
- **Always click "Always Allow"** (not just "Allow")
- This prevents the popup from appearing repeatedly

If this does not work then try:

```bash
security unlock-keychain ~/Library/Keychains/login.keychain-db
```

### 8. Build and Run

1. In Xcode, press the **Play button (▶️)** or press `Cmd + R`
2. A simple app window will open with instructions
3. The extension is now loaded into Safari

### 9. Enable in Safari

1. Open Safari
2. Go to **Safari → Settings → Advanced**
3. Check **"Show Develop menu in menu bar"**
4. In the menu bar, click **Develop → Allow Unsigned Extensions** (for first-time setup)
5. Go to **Safari → Settings → Extensions**
6. Enable **"Global Speed Extension"**
7. Grant any requested permissions

### 10. Use the Extension

- The Global Speed icon should appear in Safari's toolbar
- Click it to access speed controls and settings
- Visit any website with video/audio and test it out!

## Known Limitations

Since Safari doesn't support all Chrome APIs, some features may not work:

- **Advanced audio effects** (offscreen API not supported)
- **Tab audio capture** (tabCapture API not supported)
- Some content script injection features

Basic playback speed control and most features should work fine.

## Development Tips

### Keep Extension Running

- Once signed, you don't need to click "Allow Unsigned Extensions" every time
- To reload changes: rebuild in Xcode (`Cmd + R`)

### Update to Latest Version

To update when the original extension releases new versions:

```bash
cd globalSpeed
git pull origin master
npm install
npm run build:prod
```

Then rebuild in Xcode.

### Troubleshooting

**Extension doesn't appear in Safari:**
- Make sure you clicked "Allow Unsigned Extensions" in Develop menu
- Restart Safari completely (`Cmd + Q`, then reopen)
- Check that both targets are signed in Xcode

**Keychain keeps asking for password:**
- Click "Always Allow" instead of "Allow"
- Open Keychain Access app → find "Apple Development" certificate → Access Control → "Allow all applications to access this item"

**Build errors about iOS:**
- Select each target in Xcode
- Make sure only macOS is selected in "Supported Destinations"

## File Structure (After Conversion)

```
Global Speed/
├── Global Speed.xcodeproj          # Xcode project file
├── Global Speed/                    # Main app wrapper
│   ├── AppDelegate.swift
│   ├── ViewController.swift
│   └── ...
└── Global Speed Extension/          # Safari extension
    ├── Resources/                   # Extension files (copied from build/unpacked)
    └── SafariWebExtensionHandler.swift
```


## Credits

- **Original Extension**: [polywock/globalSpeed](https://github.com/polywock/globalSpeed)
- **Safari Port**: Community contribution for personal use
