# Main Process Worknotes
Last Updated 07/08/22: Alex Farr

- [Return to Index](0-index.md)

---
## Abstract
This article is intended to document the considerations and workflow behind the packaging process.

Questions: 
- How do I check for Firefox's version? Mozilla Docs?
		- Firefox's version is going to be checked by SCCM most effectively. This should not be done in the PowerShell script itself. 
- Where is the user's default homepage RegKey? Or how can this be managed through GPO? Autoconfig? What are the pros and cons of implementing each of them?
		- Firefox does not store a registry key for homepage. Group Policy, while effective, ultimately seems more involved than dropping a couple of autoconfig files into the install directory. 
- What MSI flags will be needed to correctly perform the silent install/uninstall in Firefox's case?
		- No particular flags are needed for calls within the script itself.  The `-NonInteractive` flag should be set while launching `Deploy-Application.exe` 
- What changes need to be made to `Deploy-Application.ps1`?
		- See below.

---
## Content

#### Implementation

After environment and workspace are set up, open `src\FirefoxESR\Deploy-Application.ps1` in VSCode and make the following changes. 

Under the variable declaration section, modify lines 64-72 to the relevant application information for Firefox: 

```
64: $appVendor = 'Mozilla'
65: $appName = 'Firefox ESR'
66: $appVersion = '102.0'
67: $appArch = 'x64'
. . .
71: $appScriptDate = '07/08/2022'
72: $appScriptAuthor = 'Alex Farr'
```

Modify the `-CloseApps` flag on line 120 in the preinstallation section, and line 152 in pre-uninstallation to attempt to close any currently active Firefox processes instead of Internet Exploder.

```
120: Show-InstallationWelcome -CloseApps 'firefox' -AllowDefer -DeferTimes 3 -CheckDiskSpace -PersistPrompt
```

```
150: Show-InstallationWelcome -CloseApps 'firefox' -CloseAppsCountdown 60
```

Under the installation section, remove the MSI autorun template materials and add line 131 to run the Firefox MSI with `Execute-MSI`.

```
131: Execute-MSI -Action Install -Path 'Firefox Setup 102.0esr.msi'
```

Under the post-installation section, add lines 139 and 140 which use `Copy-File` to drop in the autoconfig files from `src\FirefoxESR\SupportFiles` into `C:\Program Files\Mozilla Firefox\` and `..\defaults\pref\`  in order to force default the homepage to ung.edu. 

```
139: Copy-File -Path 'autoconfig.js' -Destination "C:\Program Files\Mozilla Firefox\defaults\pref"

140: Copy-File -Path 'ung.cfg' -Destination "C:\Program Files\Mozilla Firefox"
```

Then modify line 143 to reflect the proper application name in the notification box upon completion. 

```
143: Show-InstallationPrompt -Message 'Firefox ESR 102.0 has been installed successfully.' -ButtonRightText 'OK' -Icon Information -NoWait
```

Modify line 162 in the uninstallation section to run the uninstall helper exe with `Execute-Process`.

```
162: Execute-Process -Path 'C:\Program Files\Mozilla Firefox\uninstall\helper.exe' -Parameters '/S'
```

Before testing, the autoconfig files need to be built. Create `autoconfig.js` and `ung.cfg` in `src\FirefoxESR\SupportFiles`. Edit `autoconfig.js` to contain the following: 

```
1: pref("general.config.filename", "ung.cfg");
2: pref("general.config.obscure_value", 0);
```

Then modify `ung.cfg` to contain: 

```
1: // Obligatory non-executable first line.
2: defaultPref("browser.startup.homepage", "data:text/plain,browser.startup.homepage=https://www.ung.edu");
```

Since PSADT has verbose logging, refer to the Firefox logs at `C:\Windows\Logs\Software` to verify that installation and uninstallation processes run correctly during testing. 

#### Issues

The `Execute-Process` call for uninstallation on line 161 is using an absolute path to the helper exe which is less than ideal, however the PSADT `Execute-MSI` route referencing the packaged MSI fails to locate the installed application through registry checking, as does resolving the same literal path through the registry that PSADT should have searched for by using `Get-ItemPropertyValue` and then providing that to the `-Path 
 flag of `Execute-MSI`.

---
## Resources

- [Mozilla Docs](https://developer.mozilla.org/en-US/docs/web)
- [File & Registry locations | Firefox Support Forum - Mozilla](https://support.mozilla.org/en-US/questions/1172259)
- [Setting the Default Firefox Homepage with Autoconfig](http://mike.kaply.com/2012/08/29/setting-the-default-firefox-homepage-with-autoconfig/)
- [Customizing Firefox Using AutoConfig](https://support.mozilla.org/en-US/kb/customizing-firefox-using-autoconfig)
