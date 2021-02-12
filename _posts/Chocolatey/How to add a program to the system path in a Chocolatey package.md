# How to add a program to the system path in a Chocolatey package

Hello ! I'm Xavier Jouvenot and in this small post, we are going to see how to add a program to the system path in a Chocolatey package.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) 😉

## Problematic

When creating a Chocolatey package, you may end up installing a program either in a totally different place than chocolatey packages binary folder, or [excluding the executables you want to install from getting shims](http://10xlearner.com/2021/02/12/how-to-exclude-an-executable-from-getting-shimming-in-a-chocolatey-package/). In such case, the program installed may not be directly accessible via the command line as it will not be present in one path included in the `PATH` variable of the system.

## Solution  

To correct this problem, all you have to do is to include one of the two following lines:
```powershell
Install-ChocolateyPath -PathToInstall "C:\actual\path\to\the\executable" -PathType User
# or, if the executable is getting shims are installed in the same place of the install script
Install-ChocolateyPath -PathToInstall $PSScriptRoot -PathType User
```

Pretty easy solution isn't it ?
You only need to know where your program is getting installed and everything will work fine 😉

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [PSScriptRoot documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_automatic_variables?view=powershell-7.1#psscriptroot)
- [Install-ChocolateyPath documentation](https://docs.chocolatey.org/en-us/create/functions/install-chocolateypath)
- [Excluding an executable from getting shims](http://10xlearner.com/2021/02/12/how-to-exclude-an-executable-from-getting-shimming-in-a-chocolatey-package/)
- [10xlearner website](www.10xlearner.com)

