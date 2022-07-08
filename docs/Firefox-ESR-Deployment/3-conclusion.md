# Conclusion and Closing Notes
Last Updated 07/08/22: Alex Farr

- [Return to Index](0-index.md)

---
## Abstract

This article contains a retrospective descriptive analysis of the Firefox: ESR packaged solution after completion. 

---
## Content

#### Process Description

The challenge as stated in [Preparation and Environment Notes](prep.md) was to create a PowerShell script which is capable of silent installation and uninstallation of Firefox ESR. The script also needs to make the necessary changes to the Firefox installation to set the default homepage for all users to the [UNG Main Page](https://www.ung.edu). 

The Firefox MSI package was downloaded into `res` in the project directory. The challenge recommends investigating [PSAppDeployToolkit](https://www.psappdeploytoolkit.com/) for the solution, which has been downloaded to `res` as well. PSADT contains a folder structure named `Toolkit` with a set of dependencies, configs, and supporting scripts, as well as a main logic script `Deploy-Application.ps1` and an exe wrapper by the same name. 

According to the documentation for PSADT, the toolkit folder is a portable framework that will be the skeleton of the final packaged deployment. The toolkit folder was copied into `src\Firefox ESR`, then `src\FirefoxESR\Files` and `src\FirefoxESR\SupportFiles` directories were added to the package, and the Firefox ESR MSI package was copied from `res` into `src\FirefoxESR\Files`. All external resources required by the initial scope are assembled in the workspace at this point and work can begin. 

Following these setup procedures, the modifications to `Deploy-Application.ps1` that are described in [Main Process Worknotes](process.md) can be carried out, the details of which will be omitted here for organizational clarity. 

#### Supplemental Configuration

Given that version detection built into the PowerShell script for PSADT would not only be probably another 20+ lines, but would also limit our ability to keep the deployment environment in compliance if it is only executed when the package is deployed. As such, a supporting PowerShell script should be built into the SCCM Package as a custom script detection method so that the version state can be polled from the client independently of the install process. This allows for responsive compliance manag updating in the case of a user initiated rollback

#### Notes

As pointed out in the Issues heading of [Main Process Worknotes](process.md), this particular MSI has a problem with uninstallation. It was not possible in the time allotted to investigate in-depth how to work through these issues, as they appear to be rooted in how the MSI sets up it's registry values during installation and would likely require modification of the MSI itself. When attempting to uninstall Firefox ESR using:

```
Execute-MSI -Action Uninstall -Path 'Firefox Setup 102.0esr.msi'
```

the following error is generated in the output logs:

```
[Uninstallation] :: Skipping removal of application [Mozilla Firefox ESR (x64 en-US)] because unable to discover MSI ProductCode from application's registry Uninstall subkey [Mozilla Firefox 102.0 ESR (x64 en-US)]
```

This appears to stem from the fact that PSADT expects an MSI code to be in `UninstallString` instead of a path to a helper exe. Currently PSADT is passing a filepath to `msiexec.exe -x` instead of the MSI code it needs to run correctly.  

Likewise, when attempting to target the `UninstallString` key of Firefox's Uninstall entry in the registry using: 

```
$uninstallPath = Get-ItemPropertyValue -Path 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\Mozilla Firefox 102.0 ESR (x64 en-US)' -Name UninstallString

Execute-Process -Path $uninstallPath -Parameters '/S'
```

the following error is generated in the output logs: 

```
[Uninstallation] :: Function failed, setting exit code to [60002]. 
Error Record:
-------------At C:\Users\leToads\Firefox-ESR-Deployment\src\FirefoxESR\AppDeployToolkit\AppDeployToolkitMain.ps1:3055 char:8
+ ...         If (([IO.Path]::IsPathRooted($Path)) -and ([IO.Path]::HasExte ...
+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Error Inner Exception(s):
-------------------------
```

