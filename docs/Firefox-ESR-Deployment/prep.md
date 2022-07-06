# Preparation and Environment Notes
#version/v1

- [Return to Index](index.md)

___
## Abstract
This article is intended to document the preparation steps taken and the environment setup used for testing purposes.

___
## Content
#### The Challenge
##### Assignment: 
Create a PowerShell Script for installing Firefox ESR along with some documenation of your process of going about the task/thought process. 

##### Requirements: 
Silent Install
Silent Uninstall
Homepage for all users set to https://www.ung.edu/

##### Deployment environment:
Hyper-V nodes:
- CLIENT0: {2-core, 4GB RAM, 32GB VHD} running Windows 10 build 21H2
- CLIENT1: {2-core, 4GB RAM, 32GB VHD} running Windows 10 build 21H2
- CM0: {2-core, 8GB RAM, 32GB VHD} running Windows Server 2019 MEMCM build 2111
- DC0: {2-core, 8GB RAM, 32GB VHD} running Windows Server 2019 AD/DS


#### Preparation steps: 
- Add initial challenge scope for reference.
- Setup workspace folder and add Obsidian vault and VSCode project folder templates. 
- Pull required online resources. 
- Copy necessary online resources into workspace. 

#### Considerations: 
- Version checking
- Registry Key or Group Policy Object for default homepage requirement
- Keep brainstorming more considerations to watch out for...

#### Primary Workflow: 
- Modify `Deploy-Application.ps1` to fit requirements and considerations. Refer to [Main Process Worknotes](process.md).

#### Testing and Conclusions:
- Document testing approach, as well as advantages and limitations of the implementation. Refer to [Conclusion and Closing Notes](conclusion.md).

___
## Resources
- [PSAppDeployToolkit](https://www.psappdeploytoolkit.com/)
- [Firefox](https://www.mozilla.org/en-US/firefox/)
- [VS Code](https://code.visualstudio.com/)