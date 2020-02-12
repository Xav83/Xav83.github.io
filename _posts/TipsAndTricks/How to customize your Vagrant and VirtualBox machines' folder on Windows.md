# How to customize your Vagrant and VirtualBox machines' folder on Windows ?

Last week, for one of my side projects, I had to use vagrant in order to be able to run some tests on a Chocolatey packages I am currently trying to update.
But having a computer almost full, any call to `vagrant up` ended up failing because of my lack of memory space on my main hard drive. 😝

## VirtualBox and Vagrant

A small digression for those who don't know what VirtualBox or Vagrant are. 😉

[VirtualBox](https://www.virtualbox.org/) is a software made by [Oracle](https://www.oracle.com/index.html), that allows us to virtualize system and to run virtualize system on a machine. This means that you can potentially have run any version of Windows, Linux or Mac on your own machine with this software.

[Vagrant](https://www.vagrantup.com/) is a software made by [Hashicorp](https://www.hashicorp.com/), which simplifies the use of virtualization tools and offer a way to easily run and automate interactions with virtualized environments.

On a windows system, you can install both of them using [Chocolatey](https://chocolatey.org/) with powershell like that:
```powershell
choco install virtualBox
choco install vagrant
```

## Back to our problem

Having virtual machines is nice, but can requires a lot of space.

So we need a way to have [VirtualBox](https://www.virtualbox.org/) and [Vagrant](https://www.vagrantup.com/) them in a different folder than their default one, on your main hard drive.
Doing this is easy, since [VirtualBox](https://www.virtualbox.org/) and [Vagrant](https://www.vagrantup.com/) actually gives you the possibilities to change those elements (some people must have had the same problem before 😆 )

To do so, open your powershell terminal in Admin mode and run the following command with your custom folder

```powershell
VBoxManage setproperty machinefolder <your/custom/VirtualBox/folder/>
setx VAGRANT_HOME <your/custom/Vagrant/folder>
```

Personally, here how I used them

```powershell
VBoxManage setproperty machinefolder "D:/VirtualBoxVMs"
setx VAGRANT_HOME "D:/.vagrant_d
```

And now, all I have to do is run `vagrant up` to have my virtual machine running and stored in my other hard drive. 🙂

## Conclusion

[VirtualBox](https://www.virtualbox.org/) and [Vagrant](https://www.vagrantup.com/) are amazing tools with a lot of configuration possibilities, so you can adapt them to your needs and constraints.

I hope this article will give you the information you were looking for.
Personally, I needed to write this post so that future me will be able to go back to it whenever he will need to do so 😆

Thank you all for reading this article !
And until my next article, have an splendid day 😉
