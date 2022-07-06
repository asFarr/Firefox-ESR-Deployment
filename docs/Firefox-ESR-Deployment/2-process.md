# Main Process Worknotes
Last Updated 07/06/22: Alex Farr

- [Return to Index](0-index.md)

---
## Abstract
This article is intended to document the considerations and workflow behind the packaging process.

Questions: 
- How do I check for Firefox's version? Mozilla Docs?
- Where is the user's default homepage RegKey? Or how can this be managed through GPO? Autoconfig? What are the pros and cons of implementing each of them?
- What changes need to be made to `Deploy-Application.ps1`?

---
## Contents
Firefox ESR version key: `HKEY_LOCAL_MACHINE\SOFTWARE\Mozilla\Mozilla Firefox ESR\CurrentVersion`

After environment and workspace are set up, open `src/FirefoxESR/Deploy-Application.ps1` in VSCode. 

Modify lines 64-72 to reflect the correct application information. 
Modify line 120 to attempt to close currently running Firefox processes. 
TODO: Get version regkey and check
Modify line 125 to attempt to remove any previous versions of Firefox. 

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