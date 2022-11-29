# hubitat_broadlink

This provides a Hubitat driver for IR and RF support of Broadlink universal remotes.  A companion Hubitat app is provided to share codes between devices and to import saved codes in other popular formats.
<br><br>
The Broadlink device interactions are based on the work shared here: https://github.com/mjg59/python-broadlink

Thanks to arnb and Sebastien from the Hubitat forums for all of their testing, feedback, and suggestions.



# Manual Installation instructions:

Installing with Hubitat Package Manager (HPM) is recommended.  You must use HPM version 1.8.5 or later.

If you must install manually, follow these steps:

* In the *Bundles* section of hubitat, import the hubitat_broadlink.zip bundle.
* In the *Drivers Code* section of Hubitat, install the broadlinkRemoteDriver.    
* If you plan to use multiple Broadlink remotes, install the broadlinkSystemManagerApp in the *Apps Code* section of Hubitat.    

# Virtual Device configuration instructions:

* In the *Devices* section of Hubitat, add a *New Virtual Device* of type Broadlink Remote.  See below for configuration instructions.
* On the Broadlink Remote device page, set the preferences:
    * Enter the IP address of your broadlink RM device and click Save Preferences.
    * If your remote has RF functionality (typically named some variation of RM Pro), enabled the "Does your device support RF?" option.
    * Save Preferences and refresh the page.  If you see these values in Current States then you are good to move on:
        * **activity** : idle

# Usage instructions (virtual device):

**Learning IR codes**
* Press the `learnIR` button on the Hubitat device page.
* Point your IR remote at your device.  When the LED lights up, press the desired button on the IR remote.
* The **lastCode** attribute should populate with a new code of the form *'2600.....'*
* Test the code by pressing `testLearnedCode`.  This will execute the captured code.
* If `testLearnedCode` worked as desired, use `saveLearnedCode` to name and save the code.
<br>

**Learning RF codes**
<br><br>
Learning RF codes takes place in **two steps:** first, the range is swept to find the proper RF frequency to "listen" in; then, the button press action is actually captured.
* Press the `sweepRF` button on the Hubitat device page.
* Press and HOLD a button on the RF remote.  Wait up to 20 seconds.
    * If you want to cancel the sweep, press the `cancelRF` button.
    * If the sweep identifies the frequency successfully, you will see an **activity** entry that looks like "sweep successful: freq = 434000"
    * If the sweep fails (timed out), try again.  Move to a less noisy RF environment and/or closer to your RM device.  Be sure to press and hold the button the entire time until the sweep either succeeds or times out.
    * Once the frequency is identified, **you do not need to sweep again** unless you change to a different RF remote.
* Press the `learnRF` button on the Hubitat device page.  When the LED lights up, press the desired button on the RF remote.
    * The **lastCode** attribute should populate with a new code of the form "B2...", "D7...", or similar.
* Test the code by pressing `testLearnedCode`.  This will execute the captured code.
* If `testLearnedCode` worked as desired, use `saveLearnedCode` to name and save the code.
<br>

**Generating saved codes**
* Use the `sendSavedCode` command to send a code by name that you previously saved.  The Broadlink device will automatically determine whether it is an IR or RF code based on the code content.
    * For IR codes only, the optional 'reps' parameter can be used to automatically repeat the same code multiple times.  The default behavior is to only send the code once.  This parameter is ignored for RF codes.
* Use the `sendCodeData` command to send a raw hex stream code.  This is useful if you have codes stored in a companion app or driver.
* Use the `push` command to send a saved code by name.  Enter the code name as the button number parameter.
<br>

**Compatibility with previous Hubitat integrations**
* The `importCodes` command is provided as a migration path from a popular but abandoned previous integration.
    * Copy and paste the full *codeStore* contents into the input field and click the button.
    * Refresh the device page to confirm that your codes were successfully imported.
* `SendStoredCode`, `SendCode`, and `generateIR` are provided only for compatibility with existing apps.  They are **not** recommended for new applications.

# Usage instructions (system manager app):

**Install the app**
* In the *Apps* section of Hubitat, click *Add User App* to actually install the Broadlink System Manager app.
    * Wait to fully configure the app until you have installed and configured your virtual devices.

**Save codes from virtual devices into this app**

* Each virtual device has its own set of saved codes.  Use this section to collect and save codes from whichever devices that you select.
* The app has its own storage of codes, which are appended to any time you save codes into the app from your virtual devices.

**Sync codes from this app out to virtual devices**

* Any codes stored in the app, including any that you import directly to the app, can be shared out to whichever virtual devices that you select.
* Select the codes you want to sync to a virtual device, then select the device and execute the sync.  Do this multiple times to send different sets of codes to different virtual devices as needed.

**Manage saved codes and import new codes into this app**

* You can individually delete any codes stored in the app storage.
* You can also import Pronto codes and the codeStore contents from a prior Hubitat community integration.  Other formats may be able to be added by request.

# Disclaimer

I have no affiliation with any of the companies mentioned in this readme or in the code.
