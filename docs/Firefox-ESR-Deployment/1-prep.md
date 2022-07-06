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
- Add initial challenge scope for reference.
- Setup workspace folder and add Obsidian vault and VSCode project folder templates. 
- Pull required online resources. 
- Copy necessary online resources into workspace. 


#### Considerations: 
- Version checking
- Registry key, Group Policy Object, or autoconfig file for default homepage requirements
- Keep brainstorming more considerations to watch out for...


#### Primary Workflow: 
- Modify `Deploy-Application.ps1` to fit requirements and considerations. Refer to [Main Process Worknotes](2-process.md).


#### Testing and Conclusions:
- Document testing approach, as well as advantages and limitations of the implementation. Refer to [Conclusion and Closing Notes](3-conclusion.md).

___
## Resources
- [PSAppDeployToolkit](https://www.psappdeploytoolkit.com/)
- [Firefox](https://www.mozilla.org/en-US/firefox/)
- [VS Code](https://code.visualstudio.com/)