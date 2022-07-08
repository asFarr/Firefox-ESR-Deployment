# Main Process Worknotes
Last Updated 07/06/22: Alex Farr

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
## Contents

#### First Pass

After environment and workspace are set up, open `src/FirefoxESR/Deploy-Application.ps1` in VSCode. 

Under the variable declaration section, modify lines 64-72 to the relevant application information for Firefox.

Modify the `-CloseApps` flag on line 119 in the preinstallation section, and line 151 in pre-uninstallation to attempt to close any currently active Firefox processes.

Under the installation section, modify line 130 to run the Firefox MSI.

Under the post-installation section, add lines 138 and 139 which drop in the autoconfig files to `C:\Program Files\Mozilla Firefox\` and `..\defaults\pref\`  in order to force default the homepage to ung.edu. Then modify line 142 to reflect the proper application name in the notification box upon completion. 

Modify line 161 in the uninstallation section to run the uninstall helper exe from the absolute install path.

Testing the install and uninstall implementations proved successful overall. 

#### Issues

The `Execute-Process` call for uninstallation on line 161 is using an absolute path to the helper exe which is less than ideal, however the PSADT `Execute-MSI` route referencing the packaged MSI fails to locate the installed application through registry checking, as does resolving the same literal path through the registry that PSADT should have searched for by using `Get-ItemPropertyValue` and then providing that to the `-Path 
 flag of `Execute-MSI`.


---
## Resources
- [Mozilla Docs](https://developer.mozilla.org/en-US/docs/web)
- [File & Registry locations | Firefox Support Forum - Mozilla](https://support.mozilla.org/en-US/questions/1172259)
- [Need possible registry enty to set home page for Firefox, Safari, and Chrome.](https://community.spiceworks.com/topic/137153-need-possible-registry-enty-to-set-home-page-for-firefox-safari-and-chrome)
- [Scripting : Change default homepage in IE and Firefox - ITNinja](https://www.itninja.com/question/change-default-homepage-in-ie-and-firefox)
- [Set Homepage via GPO - Microsoft Tech Community](https://techcommunity.microsoft.com/t5/enterprise/set-homepage-via-gpo/td-p/1476388)
- [GPO Homepage for all browsers - The Spiceworks Community](https://community.spiceworks.com/topic/886352-gpo-homepage-for-all-browsers)
- [Setting the Default Firefox Homepage with Autoconfig](http://mike.kaply.com/2012/08/29/setting-the-default-firefox-homepage-with-autoconfig/)
- [Customizing Firefox Using AutoConfig](https://support.mozilla.org/en-US/kb/customizing-firefox-using-autoconfig)
- [about Group Policy Settings - PowerShell | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_group_policy_settings)
- [Managing Group Policy with PowerShell](https://powershellmagazine.com/2012/05/14/managing-group-policy-with-powershell/)
- [Configure a Group Policy with PowerShell - Learn IT And DevOps Daily](https://www.ntweekly.com/2020/08/07/configure-a-group-policy-with-powershell/)
- [Installing Applications with PowerShell App Deployment Toolkit - Part 1](https://replicajunction.github.io/2015/04/07/installing-applications-with-psadt-part1/)