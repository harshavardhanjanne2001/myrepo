# Introduction 
This repository is the home for migrating the Dut server to .NET8.0 and make it cross-platform.

![macOS](https://img.shields.io/badge/macOS-blue?style=flat-square&logo=macos)
![ubuntu](https://img.shields.io/badge/ubuntu-blue?style=flat-square&logo=ubuntu)
![windows](https://img.shields.io/badge/windows-blue?style=flat-square&logo=windows)
![windows](https://img.shields.io/badge/linux-blue?style=flat-square&logo=linux)

# Requirements
Please use below link to get commands details and format in different Command sets:
- [Chronos.Edt7038](https://atssource.advantest.com/products/documentation/tree/main/docfx_project/articles/internal/products/dutsvr/chronos-edt7038.md)
- [Chronos.Slt7038](https://atssource.advantest.com/products/documentation/tree/main/docfx_project/articles/internal/products/dutsvr/chronos-slt7038.md)
- [Proteus.Slt7038](https://atssource.advantest.com/products/documentation/tree/main/docfx_project/articles/internal/products/dutsvr/proteus-slt7038.md)

# Structure
| File / Folder | Purpose                                                |
|---------------|--------------------------------------------------------|
| docs          | Documentation.                                         |
| e2e           | End to end tests, functional tests, integration tests. |
| perf          | Performance tests and benchmarks.                      |
| src           | Source code.                                           |
| test          | Unit tests.                                            |
| .gitignore    | Files that are ignored by Git source control.          |
| global.json   | .NET global version selector.                          |
| LICENSE       | License agreement.                                     |
| NuGet.config  | Nuget package manager configuration.                   |
| README.md     | Readme markdown file for the repo.                     |
| *.sln         | Visual Studio Solution files.                          |

# Technologies
| Tech            | Purpose                                                   |
|-----------------|-----------------------------------------------------------|
| .NET            | Low level platform and amazingly powerful. Helps us focus on what we really need to build. |
| C#              | Primary programming language.                             |
| OS              | Windows, Linus, macOS                                     |
| xUnit           | Unit and E2E Test Platform                                |
| DocFx           | Documentation Platform                                    |
| TBD             | UX Test Platform                                          |
| TBD             | CLI Platform                                              |
| Package Manager | NuGet                                                     |
| Sematic         | Package and application versioning.                       |
| CICD            | Azure Pipeline for now. GitHub Action coming soon...      |

# Setup
## Publish Folder Generation
Publishes the application and its dependencies to a folder for deployment to a hosting system.

Steps to generate publish folder:

- Take update for all the solutions from github repo.
- Restore and Build the solution by executing below commands in the developer powershell

	`dotnet restore .\DutServer.sln`

	`dotnet build .\DutServer.sln`

- Execute the below command to generate the publish folder for Linux:

    ```
    dotnet publish -c release -r ubuntu.16.04-x64 --self-contained .\DutServer.sln
    ```

    The publish folder will be generated in this location: `DutServer.Server\bin\Release\net8.0\ubuntu.16.04-x64`

- Execute the below command to generate the publish folder for Windows:

    ```
    dotnet publish -c release -r win-x64 --self-contained .\DutServer.sln
    ```

    The publish folder will be generated in this location: `DutServer.Server\bin\Release\net8.0\win-x64`
## Linux
You can run the cross-platform Dut Server on the Linux machine using the following ways.

### Self-Contained Deployment
To [run the self-contained published Dut Server targetting Linux](https://stackoverflow.com/questions/40226032/running-a-self-contained-asp-net-core-application-on-ubuntu), you must set the permissions and then run the executable.

Set permissions (one-time): ```chmod 777 ./DutServer.Server```

Run the Dut Server: ```./DutServer.Server```

### dotnet CLI (Dev and QA)
To run the Dut Server on the Linux machine using the [dotnet](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet) CLI, you need to [install the .NET SDK first](https://learn.microsoft.com/en-us/dotnet/core/install/linux-snap).

Install SDK (one-time): ```sudo snap install dotnet-sdk --classic --channel=8.0```

Run the Dut Server: ```dotnet DutServer.Server.dll```

## Debug
### Install the .NET SDK or the .NET Runtime on Ubuntu** :
  	To add the Microsoft package signing key to your list of trusted keys and add the package repository
  	 o wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
  	 o sudo dpkg -i packages-microsoft-prod.deb
  	 o rm packages-microsoft-prod.deb
  Install the SDK
  - sudo apt-get update && \ sudo apt-get install -y dotnet-sdk-8.0
  
  Update the SDK
  - Open terminal in computer/usr/share/dotnet
  - sudo apt-get install -y dotnet-sdk-8.0
### Install Git in Ubuntu :
  - sudo apt update  
  -	sudo apt install git
  
### Install VS Code :  
  -	Download Visual Studio Code from  https://code.visualstudio.com/docs/?dv=linux64_deb .  
  -	Install the VS Code software   
  -	From the Visual Code extensions install / enable below softwares
    -	C#
    -	.Net Core Tools
    -	.Net Core extension pack
### Setup Environemntal Variable in Ubuntu Environemnt 
  -	In the terminal open .bashrc file with the command **vim .bashrc**
  -	To enter into isert mode of bashrc file press ESC, i
  -	Navigate to end of the file with the down arrow and the end of the file add below commands 
    -	export ATS_CSPROJ_REFERENCE_SOURCE_VAR=project
    -	export PATH=$PATH:/home/
  -	To save and exit form the file press ESC , :wq and then enter .

### Launch Configurations in VS Code :
To run or debug a simple app in VS Code, select Run and Debug on the Debug start view or press F5 and VS Code will try to run your currently active file.

However, for most debugging scenarios, creating a launch configuration file is beneficial because it allows you to configure and save debugging setup details. VS Code keeps debugging configuration information in a launch.json file located in a .vscode folder in the  workspace (project root folder) 

To create a launch.json file, click the create a launch.json file link in the Run start view.

![image](https://user-images.githubusercontent.com/122955542/223425489-8444cb11-d1e9-40bd-b2b5-fcc0e9ef8b77.png)

add below code in the Launch.json for our dut server debugging 
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": ".NET Core Launch (console)",
            "type": "coreclr",
            "request": "launch",
            "preLaunchTask": "build",
            "program": "/home/ats/7038_Dev_Code/chronos/dut-server/src/DutServer.Server/bin/Debug/net8.0/DutServer.Server.dll",
            "args": [],
            "cwd": "/home/ats/7038_Dev_Code/chronos/dut-server/src/DutServer.Server",
            "stopAtEntry": false,
            "console": "externalTerminal"
        }
    ]
}
```
## Service Mode
To start the Dut Server in service mode set the command line arguments to `start --mode=custom --port=3001`

### Pull Codebase from Github in VS Code:
  -  In VS Code, Open New Terminal from Terminal menu
  -  To Update Chronos directory execute below commands
     -   `cd chronos/`
     -   `git init`
     -   `git pull origin Dev` (To pull from Dev Branch)

### Build Solution in VS Code:
  -  In VS Code, Open New Terminal from Terminal menu
  -  To build execute below Commands
     -  `cd chronos/dut-server/`
     - ` dotnet build DutServer.sln`
# Deployment
###  Batch Script: 
-The batch file “dut-server-batch-script” publishes zip folder and md5 checksum for Dut server deployment.

### Batch Script Location: 
- The “dut-server-batch-script”  batch file is available at the location `<rootfolder>\deploy`.  
	
### Batch Script Process: 
- The batch script cleans up the dotnet source code
- The cleaned source code is built by the batch script
- The final built version of the code is published at `<rootfolder>\dut-server\test\DutServer.Server.Tests\bin\Release\net8.0\ubuntu.16.04-x64\publish`.
- MD5 checksum file is created for the publish folder.
- The script file then zips the publish folder along with MD5 publish checksum file at `<rootfolder>\deploy\dutsvr-deploy-with-checksum\dut-server-v.2.0.0-rc` 
- The final deployment package is zipped at `<rootfolder>\dutserver\deploy\dutsvr-deploy-with-checksum` under the name `dut-server-v.2.0.0-rc.zip`.

### Batch Script Execution:
- Execute the batch script by double clicking on the script file at `<rootfolder>\deploy` to generate deployment package.



