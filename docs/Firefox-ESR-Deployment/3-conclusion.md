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

#### Notes

As pointed out in the Issues heading of [Main Process Worknotes](process.md), this particular MSI has a problem with uninstallation. It was not possible in the time allotted to investigate in-depth how to work through these issues, as they appear to be rooted in how the MSI sets up it's registry values during installation and would likely require modification of the MSI itself. When attempting to uninstall Firefox ESR using:

```
Execute-MSI -Action Uninstall -Path 'Firefox Setup 102.0esr.msi'
```

the following error is generated in the output logs:

```

```
