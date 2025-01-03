---
layout: default
title: GP Support Act
description: GP Support Act is a configurable AppleScript script to support your <a href="https://gigperformer.com/">Gig Performer</a> habit. Use it to check that your system is ready for the gig, then start up apps you need for your gig, and finally starts Gig Performer. It can do a long list of checks and actions that get your system ready for GP.
gitrepo: https://github.com/musios-app/gp-support-act
tags: gig-performer utility script
image: assets/images/gig-performer-icon-512x512.jpg
---

# GP Support Act

GP Support Act is a configurable AppleScript script to support your Gig Performer habit. Use it to check that your system is ready for the gig, then start up apps you need for your gig, and finally starts Gig Performer. 

<div class="alert alert-warning" role="alert">
<b>Notes</b>
<li>This utility is for MacOS only because it uses AppleScript</li>
<li>This version is an early release thats needs wider testing</li>
</div>


### Capabilities

<div class="next-list-check"></div>

* Check connections
  * Internet connections and that specific sites are reachable
  * External storage
  * Audio devices
  * MIDI devices
  * USB devices
  * Bluetooth devices
* Open applications, files and web pages
  * Chart files, playlists, lyrics...
  * MuseScore, Ultimate Guitar, lyrics
  * BOME and other MIDI Utilities
  * Web pages for lyrics, sheet music
  * Documents such as Word, Excel, Notes
  * Open documents like playlists, lyrics, etc.
* Other environment setup
  * Switch to light or dark mode
* Start Gig Performer with your Gig file
* And most other tasks you need that can be run from the command line

The implementation is intended to be resilient to errors. For example, if an external drive is missing, an audio device is not connected, or a web page is unreachable, then the script will offer you the option to continue or stop. (There are some limits to this that can be addressed in future versions.)

### Sample Configuration

This script shows some of things that can be done with support act. 
You can change and add actions to suit your rig and performance needs.
If there's a problem with your environment (like a missing file or device), then the script will warn you and give the optioin to stop and fix, or continue.

```applescript
-- Setup for Friday night gig
checkNetAccess("www.musescore.com")
checkFileOrFolderAccessible("/Volumes/ExternalSSD/Instruments")
checkAudioDeviceConnected("EVO8")
checkUSBDeviceConnected("XPIANO73")
openDocument("/Users/musios/Documents/Bome MIDI Translator/Presets/numa-x-piano73.bmtp")
openWebPage("Google Chrome", "https://musescore.com/official_scores/scores/6937415")
openDocument("/Users/musios/charts/Let me entertain you - Robbie Williams.pdf")
openDocument("/Users/musios/Documents/Gig Performer/Gig Files/practice.gig")

-- Utility functions not shown here
...
```

### Notes on AppleScript syntax

AppleScript is written to be human-readable. You don't need programming experience to modify the script. Two things that might help to know:

1. Text after "--" is a comment (that's 2 hyphens)
2. The `'¬'` character is a 'line continuation' character meaning that the current line continues on to the next line. I use it for arrays with many items. 


## Start using GP Support Act

[GP support act](https://github.com/musios-app/gp-support-act/releases) is maintained in GitHub in the [`gp-support-act`](https://github.com/musios-app/gp-support-act) repository.

1. Download the ZIP file and unzip the contents
2. Make your own copy of `gp-support-act.applescript` 
3. Open your copy in Apple's "Script Editor" application
4. Configure the script for your environment (see below)
5. Run the script


### Configuration: your script for your environment

The script is written in AppleScript and is intended to be easy to modify to your environment and needs.

Change the "Configuration" section at the top. There are examples and comments.

The utility scripts are the second half of the file. You can modify these if you need to, but they are intended to be general-purpose.

If you're comfortable with AppleScript and shell script, then jump right in. The detail below is intended for users who are new to AppleScript and/or the command line.



### Copying file names to the script

If you need to copy file names to the script, then you can drag and drop the file into the Script Editor window. The full path will be copied to the script.

Alternatively, you can Copy the file in Finder then paste into the Script Editor window. The full path will be copied to the script.  You may need to remove the `\` backslash characters and/or add double quotes around the filename.


## Utility functions

### `initialize()`

Always initialize at the start of the script. This sets up some of the utilities.

```applescript
initialize()
```


### `progress_start(numSteps, description, descript_add)`

Sets up the progress bar which appears at the bottom of the Script Editor.
Each utility automatically updates the progress bar during execution.

Note: the number of steps doesn't need to be exact (unless you really care!).

Example: 

```applescript
progress_start(30, "GP Support Act", "Getting ready to perform!")
```


### `setLightMode() / setDarkMode()`

Switch your desktop to light or dark mode.

```applescript
setDarkMode()
setLightMode()
```


### `checkNetAccess(<web address>)`

This function checks that the computer has internet access and that a specific site is reachable. You must provide a web address to check. For general checks a site like `www.google.com` is a good choice. If your performance relies on a specific site, then use that.

```applescript
checkNetAccess("www.google.com")
checkNetAccess("www.musescore.com")
checkNetAccess("127.0.0.1")
```

### `cloudDownload(<file-path>)`

Cloud storage like Dropbox, Google Drive, and iCloud have defauly mode of storing files in the cloud to space on your drive. But for music performance you want as much content stored on your disk as practice to avoid delays.

This function need to force a download files to your local drive so your performance isn't slowed.

Depending upon the size of the file or directory, this may take a while. It's recommended that you use the "Keep Local" option in your cloud storage app to keep the files on your computer.

```applescript
cloudDownload("/Users/musios/Hey Bulldog - The Beatles")
```

### `checkFileOrFolderAccessible(<disk-path>)`

Check that external disks are connected to ensure that plugin data, sheet music and other content is accessible before starting GP.

```applescript
checkFileOrFolderAccessible("/Volumes/MusicSSD/Instruments")
```

Note: `checkFileOrFolderAccessible` does not force a download of cloud files. For that, use `cloudDownload()`.


### `checkAudioDevice(<device-name>)`

Check that a specific audio device is connected. 

```applescript
checkAudioDeviceConnected("EVO8")
checkAudioDeviceConnected("FLOW8")
checkAudioDeviceConnected("MacBook Air Speakers")
checkAudioDeviceConnected("MacBook Air Microphone")
```

Note: this check does NOT distinguish between input and output devices (but this doesn't normally matter).

To find out the exact names of your audio devices try one of these...

1. Use the `listConnectedAudioDevices()` function in the script,
2. Or, open the System Information app and navigate to the "Audio" section. The names of the devices are what you need to use in the script.
3. Or, use the `system_profiler SPAudioDataType` command in a terminal


### `checkUSBDevice(<device-name>)`

Check that a USB device is connected by providing the exact name.

```applescript
checkUSBDeviceConnected("XPIANO73")
checkUSBDeviceConnected("Stream Deck Plus")
```

To find the exact names of your USB devices:

```applescript
listConnectedUSBDevices()
```

Or, open the MacOS System Information app and navigate to the "USB" section.

Or, open a terminal and run `system_profiler SPUSBDataType` 
and `system_profiler SPBluetoothDataType` command in a terminal


### `checkBluetoothDevice(<device-name>)`

Check that a Bluetooth device is connected by providing the exact name.

```applescript
checkBluetoothDeviceConnected("FS-1-WL")
checkBluetoothDeviceConnected("Seaboard Block JU4X")
```

To find the exact names of your USB devices:

```applescript
listConnectedBlueoothDevices()
```

Or, open the MacOS System Information app and navigate to the "Bluetooth" section.

Or, open a terminal and run `system_profiler SPBluetoothDataType` 

### `disableBluetooth() / enableBluetooth()`

Enable or disable Bluetooth.

<div class="alert alert-warning" role="alert">
CAUTION: if you are using Bluetooth mouse and keyboard, then make sure you have a method to re-enable Bluetooth. The built-in mouse and keyboard are fine for a Macbook.  For Mac Mini and iMac, keep a cable handy to connect the mouse and keyboard directly to your Mac.
</div>

```applescript
disableBluetooth()
enableBluetooth()
```


### `openWebPage(<browser>, <web-address>)`

Open a web page in your preferred browser, such as "Safari", "Google Chrome", "Arc", or "Firefox". Use the full name that appears in your Application list. 

```applescript
openWebPage("Firefox", "https://musescore.com/official_scores/scores/6937415")
openWebPage("Google Chrome", "https://tabs.ultimate-guitar.com/tab/royal-blood/figure-it-out-official-2007289")
```

Note: Firefox is preferrable to Chromium-based browsers (Google Chrome, Safari, Arc, Brave...) because of it's lower memory footprint.


### `openDocument(<document-path>)`

The `openDocument` function opens a document in the default application that Finder would use. For example, a PDF will typically open in Preview, a text file in TextEdit, a spreadsheet in Excel and so on. 

Files for musical applications also work (if they work in Finder). For example, `.gig` files for Gig Performer, `.mscz` files for MuseScore, `.bmtp` files for BOME MIDI Translator, and many more.

```applescript
openDocument("/Users/musios/charts/I'll forget you.pdf")
openDocument("/Users/musios/charts/Celebration - Kool & the Gang/Celebration.mscz")
openDocument("/Users/musios/charts/Song List.xlsx")
openDocument("/Users/musios/charts/text-doc.txt")
openDocument("/Users/musios/Documents/Bome MIDI Translator/Presets/Korg.bmtp")
```

Notes: 

* Many songs names contain quote characters like single apostrophe. These may cause script issues. If so, consider removing the quotes character from the filename and script
* `openDocument` does not work with web pages
* It does not work with regular expressions to match multiple files. If you need to open multiple files, then use a terminal command (below) to open them all at once.




### `runTerminalCommand(<command>)`

### `itermCommand(<command>)`

The `runTerminalCommand` and `itermCommand` functions each run a command in a terminal window - the first in the MacOS Terminal app and the second in iTerm2 (a popular alternative). 

This is useful for commands that need to be monitored or stopped (like web servers, MIDI utilities) as the terminal window will stay open after the command has run.

```applescript
runTerminalCommand("echo 'howdy'")

itermCommand("open -a 'Bome MIDI Translator Pro' '/Users/musios/Documents/Bome MIDI Translator/Presets/Casio PX-5S.bmtp'")

-- Open a folder full of PDF files in Preview
itermCommand("open -a Preview /Users/musios/charts/*.pdf")
```


### Finally, start Gig Performer

Once all the checks are complete, you can start Gig Performer with a specific Gig file. 

```applescript
openDocument("/Users/musios/Documents/Gig Performer/Gig Files/demo.gig")
```


## Support & Feedback

This is a new project. I'm keen to hear your feedback and suggestions.

The [GitHub issues page for gp-support-act](https://github.com/musios-app/gp-support-act/issues) is the best place questions, suggestions, bugs and requests. 

Alternatively, post a message on the Gig Performer forum. I'm there as "[Andrew](https://community.gigperformer.com/u/andrew/summary)".



### GP Support Act - the script

```applescript
{% include_relative gp-support-act.applescript %}
```


<script>
  document.addEventListener('DOMContentLoaded', function() {
    const preElements = document.querySelectorAll('.rouge-table .rouge-code pre');
    preElements.forEach((pre, index) => {
      const rougeCode = pre.closest(".rouge-code")
      const button = document.createElement('button');
      button.className = 'rouge-code-copy-button';
      button.innerHTML = '<i class="fa-regular fa-copy"></i>'

      button.addEventListener('click', () => {
          const code = pre.innerText;
          navigator.clipboard.writeText(code).then(() => {
              button.classList.add('success');
              setTimeout(() => {
                button.classList.remove('success');
              }, 250)
          }).catch(err => {
              console.error('Failed to copy: ', err);
          });
      });

      pre.parentNode.insertBefore(button, pre.nextSibling);
    });
  });

</script>



