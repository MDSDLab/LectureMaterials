# Infrastructure

During the laboratory sessions, we are using modeling and programming environments. We prepared three ways to use those tools.

## Install Instructions

The most simple solution is to install and run the modeling tools on our computer directly. In the following, we give detailed step-by-step installation instructions.

### Java
- Install OpenJDK 17. (Windows use Temurin jdk, Linux adoptium)

### Eclipse
- Install Eclipse Modeling Tools from https://www.eclipse.org/downloads/packages/release/2023-06/r/eclipse-modeling-tools

- Install some packages
    - using Help | Install | Install new software
    - select 2023-06 releases
    - Select the following packages:    
        - Xtext complete SDK
        - Xtend IDE
        - EMF - Eclipse Modeling Framework Xcore SDK
        - VIATRA Query and Transformation SDK
    - Continue and finish with the Next and Finish buttons, wait for the installation to finish, and press Restart when asked.

- Make sure that the default encoding is UTF-8: Window | Preferences | General | Workspace | Text file encoding.
- Similarly, make sure the New text file line delimiter is Unix.
- If you have problems with typing with Hungarian keyboard layout and hotkeys, go to Windows | Preferences | General | Keys and remove bindings with CTRL+ALT.
- Set up Java execution environment in Eclipse: Windows | Preferences | Java | Installed JREs | Add. Select the installation path of your SDK (like `/usr/lib/jvm/java-17-openjdk-amd64`, without bin and lib)
- Under "Installed JREs" set up the "Execution Environment", select JavaSE-17 on the left-hand side, and select "java-17-<your installed version>". If you see "jre [perfect match]" only on the right-hand side, restart eclipse.
- Press Apply and Close.

### IntelliJ IDEA
- Install IntelliJ IDEA Community Edition https://www.jetbrains.com/idea/download/
- On Linux, unpack the tar.gz file, navigate into ~/bin, and run the ./idea.sh command to run IntelliJ IDEA.
- When creating a new project, select the JDK that you have installed in the first section.
- For an existing project, set the JDK in the following places:
    - File | Project Settings | Project | SDK
    - File | Project Settings | Modules | Dependencies
    - Apply and Close
- Install the ANTLR plugin at File | Settings | Plugins --> search for "antlr" in Marketplace, install, then restart the IDE.

## Virtual Machine
We also prepared a virtual machine where all the modeling environments are preinstalled.

- Install VirtualBox 7.0.10: https://www.virtualbox.org/wiki/Downloads
- Download virtual machine using the following link: https://share.mit.bme.hu/index.php/s/4te6srbKkRXeYZ2/download?path=%2F&files=BME-MDSD-23-v1.ova.
- Start the virtual machine in VirtualBox.
- Use the following `username`/`password `: `cloud`/`LaborImage`

## Cloud

If there are any difficulties with the virtual machine, we can provide a limited number of cloud computers.


