# How to setup Chocolatey test environment on Ubuntu 21.10 ?

Hello ! I'm Xavier Jouvenot and in this post, we are going to see to setup Chocolatey test environment on Ubuntu 21.10.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) 😉

## Let's dive directly in the solution !

### Pre-requirements

I order to be able to run the Chocolatey test environment, you need to first install 2 tools: VirtualBox and Vagrant.

To install VirtualBox on your Ubuntu 21.10, you can go to this [VirtualBox website](https://www.virtualbox.org/wiki/Linux_Downloads) to download the installer, or download it directly [here](https://download.virtualbox.org/virtualbox/6.1.30/virtualbox-6.1_6.1.30-148432~Ubuntu~eoan_amd64.deb).
Once the installer downloaded, run it to have a VirtualBox ready to be used.

Then, to install Vagrant, then you will have to use your terminal !
You can either install the version available by default on the system by using the command `sudo apt-get install vagrant` or you can install the latest version of vagrant with the following commands:
```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install vagrant
```

Finally, you are going to need to install some plugins in vagrant, 3 to be exact: `winrm`, `winrm-fs` and `winrm-elevated`. And you can also do that by executing the following commands:
```bash
vagrant plugin install winrm
vagrant plugin install wimrm-fs
vagrant plugin install wimrm-elevated
```

<details>
  <summary>Complete Vagrant result</summary>

```bash
sudo apt-get install vagrant

vagrant plugin install winrm
    Installing the 'winrm' plugin. This can take a few minutes...
    Fetching vagrant-libvirt-0.7.0.gem
    Successfully uninstalled vagrant-libvirt-0.7.0
    Installed the plugin 'winrm (2.3.6)'!

vagrant plugin install wimrm-fs
    Installing the 'winrm-fs' plugin. This can take a few minutes...
    Fetching vagrant-libvirt-0.7.0.gem
    Successfully uninstalled vagrant-libvirt-0.7.0
    Installed the plugin 'winrm-fs (1.3.5)'!

vagrant plugin install wimrm-elevated
    Installing the 'winrm-elevated' plugin. This can take a few minutes...
    Fetching vagrant-libvirt-0.7.0.gem
    Fetching winrm-elevated-1.2.3.gem
    Successfully uninstalled vagrant-libvirt-0.7.0
```
</details>

#### Chocolatey test environment

Once all the required tools are installed, we can focus on the test environment !

First, we need to get the GitHub repository on our machine by downloading it on [GitHub repository](https://github.com/chocolatey-community/chocolatey-test-environment) ([direct download here](https://github.com/chocolatey-community/chocolatey-test-environment/archive/refs/heads/master.zip)) or by cloning it with the following git command:

```bash
git clone https://github.com/chocolatey-community/chocolatey-test-environment.git
```

Once the repository is downloaded, and your computer have all the software from the requirements section, then you are done, you can now launch Chocolatey test environment by running the following command in the Chocolaty test environment folder, you will make a window pop up with the test environment in it:
```bash
vagrant up
```

<details>
  <summary>Complete Vagrant result</summary>

```bash
vagrant up
    Bringing machine 'default' up with 'virtualbox' provider...
    ==> default: Box 'chocolatey/test-environment' could not be found. Attempting to find and install...
        default: Box Provider: virtualbox
        default: Box Version: >= 0
    ==> default: Loading metadata for box 'chocolatey/test-environment'
        default: URL: https://vagrantcloud.com/chocolatey/test-environment
    ==> default: Adding box 'chocolatey/test-environment' (v2.0.0) for provider: virtualbox
        default: Downloading: https://vagrantcloud.com/chocolatey/boxes/test-environment/versions/2.0.0/providers/virtualbox.box
    Download redirected to host: vagrantcloud-files-production.s3-accelerate.amazonaws.com
        default: Calculating and comparing box checksum...
    ==> default: Successfully added box 'chocolatey/test-environment' (v2.0.0) for 'virtualbox'!
    ==> default: Preparing master VM for linked clones...
        default: This is a one time operation. Once the master VM is prepared,
        default: it will be used as a base for linked clones, making the creation
        default: of new VMs take milliseconds on a modern system.
    ==> default: Importing base box 'chocolatey/test-environment'...
    ==> default: Cloning VM...
    ==> default: Matching MAC address for NAT networking...
    ==> default: Checking if box 'chocolatey/test-environment' version '2.0.0' is up to date...
    ==> default: Setting the name of the VM: chocolatey-test-environment_default_1637421341511_10005
    Vagrant is currently configured to create VirtualBox synced folders with
    the `SharedFoldersEnableSymlinksCreate` option enabled. If the Vagrant
    guest is not trusted, you may want to disable this option. For more
    information on this option, please refer to the VirtualBox manual:

    https://www.virtualbox.org/manual/ch04.html#sharedfolders

    This option can be disabled globally with an environment variable:

    VAGRANT_DISABLE_VBOXSYMLINKCREATE=1

    or on a per folder basis within the Vagrantfile:

    config.vm.synced_folder '/host/path', '/guest/path', SharedFoldersEnableSymlinksCreate: false
    ==> default: Clearing any previously set network interfaces...
    ==> default: Preparing network interfaces based on configuration...
        default: Adapter 1: nat
    ==> default: Forwarding ports...
        default: 5985 (guest) => 55985 (host) (adapter 1)
        default: 3389 (guest) => 3389 (host) (adapter 1)
        default: 22 (guest) => 2222 (host) (adapter 1)
        default: 5986 (guest) => 55986 (host) (adapter 1)
    ==> default: Running 'pre-boot' VM customizations...
    ==> default: Booting VM...
    ==> default: Waiting for machine to boot. This may take a few minutes...
        default: WinRM address: 127.0.0.1:55985
        default: WinRM username: vagrant
        default: WinRM execution_time_limit: PT2H
        default: WinRM transport: negotiate
    ==> default: Machine booted and ready!
    ==> default: Checking for guest additions in VM...
    ==> default: Mounting shared folders...
        default: /vagrant => /home/xav/.work/chocolatey-test-environment
        default: /packages => /home/xav/.work/chocolatey-test-environment/packages
    ==> default: Running provisioner: shell...
        default: Running: shell/PrepareWindows.ps1 as C:\tmp\vagrant-shell.ps1
        default: IE Enhanced Security Configuration (ESC) has been disabled. Required for One-Click deploy to work appropriately.
        default: IE first run welcome screen has been disabled. Required for One-Click deploy to work appropriately.
        default: Setting Windows Update service to Manual startup type.
    ==> default: Running provisioner: shell...
        default: Running: shell/InstallNet4.ps1 as C:\tmp\vagrant-shell.ps1
    ==> default: Running provisioner: shell...
        default: Running: shell/InstallChocolatey.ps1 as C:\tmp\vagrant-shell.ps1
        default: Mode                LastWriteTime     Length Name                              
        default: ----                -------------     ------ ----                              
        default: d----        11/20/2021   3:18 PM            chocInstall                       
        default: Downloading https://community.chocolatey.org/api/v2/package/chocolatey/0.11.3 to C:\Users\ADMINI~1\AppData\Local\Temp\chocolatey\chocInstall\chocolatey.zip
        default: Download 7Zip commandline tool
        default: Downloading https://community.chocolatey.org/7za.exe to C:\Users\ADMINI~1\AppData\Local\Temp\chocolatey\chocInstall\7za.exe
        default: Extracting C:\Users\ADMINI~1\AppData\Local\Temp\chocolatey\chocInstall\chocolatey.zip to C:\Users\ADMINI~1\AppData\Local\Temp\chocolatey\chocInstall...
        default: 7-Zip (a) 18.06 (x86) : Copyright (c) 1999-2018 Igor Pavlov : 2018-12-30
        default: Scanning the drive for archives:
        default: 1 file, 4039400 bytes (3945 KiB)
        default: Extracting archive: C:\Users\ADMINI~1\AppData\Local\Temp\chocolatey\chocInstall\chocolatey.zip
        default: --
        default: Path = C:\Users\ADMINI~1\AppData\Local\Temp\chocolatey\chocInstall\chocolatey.zip
        default: Type = zip
        default: Physical Size = 4039400
        default: Everything is Ok
        default: Files: 92
        default: Size:       10569062
        default: Compressed: 4039400
        default: Installing chocolatey on this machine
        default: DEBUG: Initialize-Chocolatey
        default: DEBUG: Host version is 4.0, PowerShell Version is '4.0' and CLR Version is 
        default: '4.0.30319.34014'.
        default: DEBUG: Get-ChocolateyInstallFolder
        default: DEBUG: Set-ChocolateyInstallFolder
        default: DEBUG: Running Install-ChocolateyEnvironmentVariable -variableName 
        default: 'ChocolateyInstall' -variableValue '' -variableType 'User' 
        default: DEBUG: Running Set-EnvironmentVariable -Name 'ChocolateyInstall' -Value '' 
        default: -Scope 'User' 
        default: DEBUG: Test-ProcessAdminRights: returning True
        default: DEBUG: Administrator installing so using Machine environment variable target 
        default: instead of User.
        default: DEBUG: Running Install-ChocolateyEnvironmentVariable -variableName 
        default: 'ChocolateyInstall' -variableValue '' -variableType 'Machine' 
        default: DEBUG: Test-ProcessAdminRights: returning True
        default: DEBUG: Running Set-EnvironmentVariable -Name 'ChocolateyInstall' -Value '' 
        default: -Scope 'Machine' 
        default: Creating ChocolateyInstall as an environment variable (targeting 'Machine') 
        default:   Setting ChocolateyInstall to 'C:\ProgramData\chocolatey'
        default: WARNING: It's very likely you will need to close and reopen your shell 
        default:   before you can use choco.
        default: DEBUG: Running Install-ChocolateyEnvironmentVariable -variableName 
        default: 'ChocolateyInstall' -variableValue 'C:\ProgramData\chocolatey' -variableType 
        default: 'Machine' 
        default: DEBUG: Test-ProcessAdminRights: returning True
        default: DEBUG: Running Set-EnvironmentVariable -Name 'ChocolateyInstall' -Value 
        default: 'C:\ProgramData\chocolatey' -Scope 'Machine' 
        default: DEBUG: Registry type for ChocolateyInstall is/will be String
        default: DEBUG: 
        default: using System;
        default: using System.Runtime.InteropServices;
        default: namespace Win32
        default: {
        default:     public class NativeMethods
        default:     {
        default:     [DllImport("user32.dll", SetLastError = true, CharSet = CharSet.Auto)]
        default: public static extern IntPtr SendMessageTimeout(
        default:     IntPtr hWnd, uint Msg, UIntPtr wParam, string lParam,
        default:     uint fuFlags, uint uTimeout, out UIntPtr lpdwResult);
        default:     }
        default: }
        default: DEBUG: Running Update-SessionEnvironment 
        default: DEBUG: Create-DirectoryIfNotExists
        default: DEBUG: Ensure-Permissions
        default: DEBUG: Test-ProcessAdminRights: returning True
        default: DEBUG: Removing existing permissions.
        default: Restricting write permissions to Administrators
        default: DEBUG: Current user no longer set due to possible escalation of privileges - 
        default: set $env:ChocolateyInstallAllowCurrentUser="true" if you require this.
        default: DEBUG: Set Owner to Administrators
        default: DEBUG: Default Installation folder - removing inheritance with no copy
        default: DEBUG: Allow users to append to log files.
        default: DEBUG: Create-DirectoryIfNotExists
        default: We are setting up the Chocolatey package repository.
        default: The packages themselves go to 'C:\ProgramData\chocolatey\lib'
        default:   (i.e. C:\ProgramData\chocolatey\lib\yourPackageName).
        default: A shim file for the command line goes to 'C:\ProgramData\chocolatey\bin'
        default:   and points to an executable in 'C:\ProgramData\chocolatey\lib\yourPackageName'.
        default: Creating Chocolatey folders if they do not already exist.
        default: WARNING: You can safely ignore errors related to missing log files when 
        default:   upgrading from a version of Chocolatey less than 0.9.9. 
        default:   'Batch file could not be found' is also safe to ignore. 
        default:   'The system cannot find the file specified' - also safe.
        default: DEBUG: Create-DirectoryIfNotExists
        default: DEBUG: Create-DirectoryIfNotExists
        default: DEBUG: Install-ChocolateyFiles
        default: DEBUG: Removing install files in chocolateyInstall, helpers, redirects, and 
        default: tools
        default: DEBUG: Attempting to move choco.exe to choco.exe.old so we can place the new 
        default: version here.
        default: DEBUG: Unpacking files required for Chocolatey.
        default: DEBUG: Copying the contents of 
        default: 'C:\Users\Administrator\AppData\Local\Temp\chocolatey\chocInstall\tools\chocola
        default: teyInstall' to 'C:\ProgramData\chocolatey'.
        default: DEBUG: Ensure-ChocolateyLibFiles
        default: DEBUG: Create-DirectoryIfNotExists
        default: chocolatey.nupkg file not installed in lib.
        default:  Attempting to locate it from bootstrapper.
        default: DEBUG: First the zip file at 
        default: 'C:\Users\ADMINI~1\AppData\Local\Temp\chocolatey\chocInstall\chocolatey.zip'.
        default: DEBUG: Then from a neighboring chocolatey.*nupkg file 
        default: 'C:\Users\Administrator\AppData\Local\Temp\chocolatey\chocInstall\tools/../../'
        default: .
        default: DEBUG: Copying 
        default: 'C:\Users\ADMINI~1\AppData\Local\Temp\chocolatey\chocInstall\chocolatey.zip' to
        default:  'C:\ProgramData\chocolatey\lib\chocolatey\chocolatey.nupkg'.
        default: DEBUG: Install-ChocolateyBinFiles
        default: DEBUG: Installing the bin file redirects
        default: DEBUG: Attempting to copy C:\ProgramData\chocolatey\redirects\choco.exe to 
        default: C:\ProgramData\chocolatey\bin\choco.exe
        default: DEBUG: Added command choco
        default: DEBUG: Attempting to copy C:\ProgramData\chocolatey\redirects\chocolatey.exe to
        default:  C:\ProgramData\chocolatey\bin\chocolatey.exe
        default: DEBUG: Added command chocolatey
        default: DEBUG: Attempting to copy C:\ProgramData\chocolatey\redirects\cinst.exe to 
        default: C:\ProgramData\chocolatey\bin\cinst.exe
        default: DEBUG: Added command cinst
        default: DEBUG: Attempting to copy C:\ProgramData\chocolatey\redirects\clist.exe to 
        default: C:\ProgramData\chocolatey\bin\clist.exe
        default: DEBUG: Added command clist
        default: DEBUG: Attempting to copy C:\ProgramData\chocolatey\redirects\cpack.exe to 
        default: C:\ProgramData\chocolatey\bin\cpack.exe
        default: DEBUG: Added command cpack
        default: DEBUG: Attempting to copy C:\ProgramData\chocolatey\redirects\cpush.exe to 
        default: C:\ProgramData\chocolatey\bin\cpush.exe
        default: DEBUG: Added command cpush
        default: DEBUG: Attempting to copy C:\ProgramData\chocolatey\redirects\cuninst.exe to 
        default: C:\ProgramData\chocolatey\bin\cuninst.exe
        default: DEBUG: Added command cuninst
        default: DEBUG: Attempting to copy C:\ProgramData\chocolatey\redirects\cup.exe to 
        default: C:\ProgramData\chocolatey\bin\cup.exe
        default: DEBUG: Added command cup
        default: DEBUG: Attempting to copy C:\ProgramData\chocolatey\redirects\cver.exe to 
        default: C:\ProgramData\chocolatey\bin\cver.exe
        default: DEBUG: Added command cver
        default: DEBUG: Attempting to copy C:\ProgramData\chocolatey\redirects\RefreshEnv.cmd to
        default:  C:\ProgramData\chocolatey\bin\RefreshEnv.cmd
        default: DEBUG: Added command RefreshEnv
        default: DEBUG: Initialize-ChocolateyPath
        default: DEBUG: Initializing Chocolatey Path if required
        default: DEBUG: Test-ProcessAdminRights: returning True
        default: DEBUG: Administrator installing so using Machine environment variable target 
        default: instead of User.
        default: DEBUG: Running Install-ChocolateyPath -pathToInstall 
        default: 'C:\ProgramData\chocolatey\bin' -pathType 'Machine' 
        default: DEBUG: Running Update-SessionEnvironment 
        default: PATH environment variable does not have C:\ProgramData\chocolatey\bin in it. Adding...
        default: DEBUG: Test-ProcessAdminRights: returning True
        default: DEBUG: Running Set-EnvironmentVariable -Name 'Path' -Value 
        default: '%SystemRoot%\system32;%SystemRoot%;%SystemRoot%\System32\Wbem;%SYSTEMROOT%\Sys
        default: tem32\WindowsPowerShell\v1.0\;C:\Program Files (x86)\Git\cmd;C:\Program Files 
        default: (x86)\Git\bin;C:\ProgramData\chocolatey\bin;' -Scope 'Machine' 
        default: DEBUG: Registry type for Path is/will be ExpandString
        default: DEBUG: Running Update-SessionEnvironment 
        default: DEBUG: Process-ChocolateyBinFiles
        default: DEBUG: Host version is 4.0, PowerShell Version is '4.0' and CLR Version is 
        default: '4.0.30319.34014'.
        default: DEBUG: Add-ChocolateyProfile
        default: DEBUG: Creating 'C:\Users\Administrator\Documents\WindowsPowerShell'
        default: WARNING: Not setting tab completion: Profile file does not exist at 
        default: 'C:\Users\Administrator\Documents\WindowsPowerShell\Microsoft.PowerShell_profil
        default: e.ps1'.
        default: DEBUG: Install-DotNet4IfMissing called with $forceFxInstall=False
        default: DEBUG: Initializing Chocolatey files, etc by running Chocolatey...
        default: DEBUG: Get-ChocolateyInstallFolder
        default: DEBUG: Chocolatey execution completed successfully.
        default: Chocolatey (choco.exe) is now ready.
        default: You can call choco from anywhere, command line or powershell by typing choco.
        default: Run choco /? for a list of functions.
        default: You may need to shut down and restart powershell and/or consoles
        default:  first prior to using choco.
        default: DEBUG: Remove-OldChocolateyInstall
        default: Ensuring chocolatey commands are on the path
        default: Ensuring chocolatey.nupkg is in the lib folder
        default: Chocolatey v0.11.3
        default: autoUninstaller was enabled by default. Explicitly setting value.
        default: Enabled autoUninstaller
        default: Chocolatey v0.11.3
        default: Enabled allowGlobalConfirmation
        default: Chocolatey v0.11.3
        default: Enabled logEnvironmentValues
        default: Chocolatey v0.11.3
        default: Disabled showDownloadProgress
    ==> default: Running provisioner: shell...
        default: Running: shell/NotifyGuiAppsOfEnvironmentChanges.ps1 as C:\tmp\vagrant-shell.ps1
        default: 1
        default: SUCCESS: Specified value was saved.
    ==> default: Running provisioner: shell...
        default: Running: shell/PostSetup.ps1 as C:\tmp\vagrant-shell.ps1
        default: Chocolatey v0.11.3
        default: Installing the following packages:
        default: kb2919442
        default: By installing, you accept licenses for the packages.
        default: [NuGet] Installing 'KB2919442 1.0.20160915'.
        default: [NuGet] Successfully installed 'KB2919442 1.0.20160915'.
        default: KB2919442 v1.0.20160915 [Approved]
        default: kb2919442 package files install completed. Performing other installation steps.
        default:  The install of kb2919442 was successful.
        default:   Software install location not explicitly set, it could be in package or
        default:   default install location of installer.
        default: Chocolatey installed 1/1 packages. 
        default:  See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
        default: Chocolatey v0.11.3
        default: Installing the following packages:
        default: kb2919355
        default: By installing, you accept licenses for the packages.
        default: [NuGet] Attempting to resolve dependency 'KB2919442 (≥ 1.0.20160915)'.
        default: [NuGet] Installing 'KB2919355 1.0.20160915'.
        default: [NuGet] Successfully installed 'KB2919355 1.0.20160915'.
        default: KB2919355 v1.0.20160915 [Approved]
        default: kb2919355 package files install completed. Performing other installation steps.
        default:  The install of kb2919355 was successful.
        default:   Software install location not explicitly set, it could be in package or
        default:   default install location of installer.
        default: Chocolatey installed 1/1 packages. 
        default:  See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
        default: Chocolatey v0.11.3
        default: Installing the following packages:
        default: kb2999226
        default: By installing, you accept licenses for the packages.
        default: [NuGet] Attempting to resolve dependency 'kb2919355 (≥ 1.0.20160915)'.
        default: [NuGet] Attempting to resolve dependency 'KB2919442 (≥ 1.0.20160915)'.
        default: [NuGet] Attempting to resolve dependency 'chocolatey-windowsupdate.extension (≥ 1.0.2)'.
        default: [NuGet] Installing 'chocolatey-windowsupdate.extension 1.0.4'.
        default: [NuGet] Successfully installed 'chocolatey-windowsupdate.extension 1.0.4'.
        default: chocolatey-windowsupdate.extension v1.0.4 [Approved]
        default: chocolatey-windowsupdate.extension package files install completed. Performing other installation steps.
        default:  Installed/updated chocolatey-windowsupdate extensions.
        default:  The install of chocolatey-windowsupdate.extension was successful.
        default:   Software installed to 'C:\ProgramData\chocolatey\extensions\chocolatey-windowsupdate'
        default: [NuGet] Installing 'KB2999226 1.0.20181019'.
        default: [NuGet] Successfully installed 'KB2999226 1.0.20181019'.
        default: KB2999226 v1.0.20181019 [Approved] - Possibly broken
        default: kb2999226 package files install completed. Performing other installation steps.
        default:  The install of kb2999226 was successful.
        default:   Software install location not explicitly set, it could be in package or
        default:   default install location of installer.
        default: Chocolatey installed 2/2 packages. 
        default:  See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
        default: Chocolatey v0.11.3
        default: Installing the following packages:
        default: kb3035131
        default: By installing, you accept licenses for the packages.
        default: [NuGet] Attempting to resolve dependency 'chocolatey-windowsupdate.extension (≥ 1.0.4)'.
        default: [NuGet] Installing 'KB3035131 1.0.3'.
        default: [NuGet] Successfully installed 'KB3035131 1.0.3'.
        default: KB3035131 v1.0.3 [Approved]
        default: kb3035131 package files install completed. Performing other installation steps.
        default:  The install of kb3035131 was successful.
        default:   Software install location not explicitly set, it could be in package or
        default:   default install location of installer.
        default: Chocolatey installed 1/1 packages. 
        default:  See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
        default: Chocolatey v0.11.3
        default: Installing the following packages:
        default: kb3118401
        default: By installing, you accept licenses for the packages.
        default: [NuGet] Attempting to resolve dependency 'chocolatey-windowsupdate.extension (≥ 1.0.4)'.
        default: [NuGet] Attempting to resolve dependency 'KB2919355 (≥ 1.0.20160915)'.
        default: [NuGet] Attempting to resolve dependency 'KB2919442 (≥ 1.0.20160915)'.
        default: [NuGet] Installing 'KB3118401 1.0.5'.
        default: [NuGet] Successfully installed 'KB3118401 1.0.5'.
        default: KB3118401 v1.0.5 [Approved]
        default: kb3118401 package files install completed. Performing other installation steps.
        default:  The install of kb3118401 was successful.
        default:   Software install location not explicitly set, it could be in package or
        default:   default install location of installer.
        default: Chocolatey installed 1/1 packages. 
        default:  See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
    ==> default: Running provisioner: shell...
        default: Running: inline PowerShell script
        default: SUCCESS: Specified value was saved.
        default: Testing package if a line is uncommented.
        default: Exit code was 0
```
</details>

Now, you are all setup up 🙂
You can go test your Chocolatey packages without continuing this post, but if you want, you can stay and read about how I created a script to automatically to all the previous steps I have described !

#### Configuring your computer automatically

So, after making the steps described previously in this post, on my machine, I said to myself: "You know what would be cool! Setting up the environment automatically on a CI process so that I can automatically check if the packages I maintain or create are actually good to be uploaded !" (yeah, I don't have anything else to do 😝).

So I have created a [basic shell script](https://github.com/Xav83/tutorials/blob/main/Chocolatey%20test%20environment%20on%20Ubuntu/setup.sh), and versioned it and uploaded it on GitHub, which was the easy part.
Then, I created an Azure Pipeline process, in order to run the script automatically, in a Microsoft-hosted agent. But when doing so, I hit a wall of errors 😢 Some errors due to the absence of GUI, some other coming from the number of cores on Azure Pipelines, and finally a huge one related to the VT-x feature which is not enabled. That last one is currently the one stopping the script from getting executed, and stopping me from accomplishing the idea I had.

So if you know how to get around this last issue, you can contact me or comment below this post, and I will be very happy to get this small idea a great conclusion. For now, it will stay in this unaccomplished state.

---------

Thank you all for reading this article,

And until my next article, have a splendid day 😉

## Interesting links

- [Setup shell script](https://github.com/Xav83/tutorials/blob/main/Chocolatey%20test%20environment%20on%20Ubuntu/setup.sh)
- [VirtualBox website](https://www.virtualbox.org)
- [Vagrant website](https://www.vagrantup.com/)
- [Chocolatey test environment GitHub repository](https://github.com/chocolatey-community/chocolatey-test-environment)
- [10xlearner website](www.10xlearner.com)

