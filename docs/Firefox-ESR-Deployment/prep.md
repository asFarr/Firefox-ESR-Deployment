## Preparation and Environment
___

### The Challenge
#### Assignment: 
Create a PowerShell Script for installing Firefox ESR along with some documenation of your process of going about the task/thought process. 

#### Revelant Links: 
[PSAppDeployToolkit](https://www.psappdeploytoolkit.com/)
[Firefox](https://www.mozilla.org/en-US/firefox/)
[VS Code](https://code.visualstudio.com/)

#### Requirements: 
Silent Install
Silent Uninstall
Homepage for all users set to https://www.ung.edu/

#### Deployment environment:
Hyper-V nodes:
- CLIENT0: {2-core, 4GB RAM, 32GB VHD} running Windows 10 build 21H2
- CM0: {2-core, 8GB RAM, 32GB VHD} running Windows Server 2019 MEMCM build 2111
- DC0: {2-core, 8GB RAM, 32GB VHD} running Windows Server 2019 AD/DS


### Preparation steps: 
- Add initial challenge scope for reference.
- Setup workspace folder and add Obsidian vault and VSCode project folder templates. 
- Pull required online resources. 
- Copy necessary online resources into workspace. 

### Considerations: 
- Version checking
- 

### Primary Workflow: 
- Modify Deploy-Application.ps1 to fit requirements and considerations. 