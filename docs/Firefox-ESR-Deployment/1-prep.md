# Preparation and Environment Notes
Last Updated 07/06/22: Alex Farr

- [Return to Index](0-index.md)

___
## Abstract

This article is intended to document the preparation steps necessary and the environment setup required for testing purposes in the packaging of Firefox: ESR.

___
## Content

#### The Challenge
##### Assignment: 
Create a PowerShell Script for installing Firefox ESR along with some documenation of your process of going about the task/thought process. 

##### Requirements: 
Silent Install
Silent Uninstall
Homepage for all users set to https://www.ung.edu/


#### Testing environment:
Hyper-V nodes:
- CLIENT0: {2-core, 4GB RAM, 32GB VHD} running Windows 10 build 21H2
- CLIENT1: {2-core, 4GB RAM, 32GB VHD} running Windows 10 build 21H2


#### Preparation steps: 
- Setup workspace folder and add Obsidian vault and VSCode project folder templates. 
- Add initial challenge scope for reference.
- Pull required online resources: PSADT, and Firefox ESR MSI. 
- Assemble the directory structure and resource files into the deployment package.
- Create a git repository around the package, and push to GitHub.
- Clone repository into Hyper-V dev workspace. 


#### Considerations: 
- Version checking
- Registry key, Group Policy Object, or autoconfig file for default homepage requirements
- MSI flags for Firefox installer


___
## Resources

- [PSAppDeployToolkit](https://www.psappdeploytoolkit.com/)
- [Firefox](https://www.mozilla.org/en-US/firefox/)
- [VS Code](https://code.visualstudio.com/)