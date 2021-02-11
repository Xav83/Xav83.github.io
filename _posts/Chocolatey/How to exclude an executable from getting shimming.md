# How to exclude an executable from getting shimming in a Chocolatey package

Hello ! I'm Xavier Jouvenot and in this small post, we are going to see how to exclude an executable from getting shimming in a Chocolatey package.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) 😉

## Problematic

When creating or interacting with Chocolatey packages, you may end up in a situation where the program installed via Chocolatey either does display nor finish its process.
This can be very inconvenient, as the program does do what you ask it to do 😝

The problem comes from Chocolatey feature "[EXECUTABLE SHIMMING](https://docs.chocolatey.org/en-us/features/shim)", which doesn't always is well set and then, do not wait for your program to finish running.
It can usually happen when the program has a graphical interface, but you want to make it execute some process via the command line without any regard for the graphical interface.

We are going to detail 2 solutions : one if you are a user of the Chocolatey package, which is a *workaround* to the chocolatey package behavior, and another one if you are a Chocolatey package maintainer, to make your packages behave appropriately 😉

## Solution  

### Workaround

The solution, as a Chocolatey package user, is to pass a specific option to the command line you use:
```sh
my_program.exe --shimgen-waitforexit
```

The flag [shimgen-waitforexit](https://docs.chocolatey.org/en-us/features/shim#options-and-switches) is exactly made for that kind of situation.
But I also encourage you to notify the maintainers of the package so that they can implement the solution below, directly in the package itself.

### Proper correction

If your package has this kind of error, there is a way to make correct it for good 🙂
By including the next line in you install script, you will be able to exclude the executables of your package from getting shims, which will correct the problem:
```powershell
Get-ChildItem $PSScriptRoot\*.exe | ForEach-Object { New-Item "$_.ignore" -type file -force | Out-Null }
```

Let's describe a little bit what is happening here !

First, we start by getting all the `.exe` files in the install folder of the Chocolatey Package.
Then, for each executable found, we create a new empty file with the same name but which end up with the extension `.exe.ignore`.
This extension will say to Chocolatey to now shims the executable, and correct our problem at the same time 😉

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [PSScriptRoot documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_automatic_variables?view=powershell-7.1#psscriptroot)
- [Get-ChildItem documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-childitem?view=powershell-7.1)
- [New-Item documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/new-item?view=powershell-7.1)
- [Out-Null documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/out-null?view=powershell-7.1)
- [ForEach-Object documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/foreach-object?view=powershell-7.1)
- [shimgen-waitforexit option](https://docs.chocolatey.org/en-us/features/shim#options-and-switches)
- [chocolatey excluding executable](https://chocolatey.org/courses/creating-chocolatey-packages/shims#excluding-executables)
- [10xlearner website](www.10xlearner.com)

