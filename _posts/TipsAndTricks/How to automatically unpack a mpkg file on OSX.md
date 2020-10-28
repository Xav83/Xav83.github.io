# How to automatically unpack a mpkg file on OSX
 
 I'm Xavier Jouvenot and in this small post, we are going to see how to automatically unpack a mpkg file on OSX.

 _Self promotion_: You can find other articles on my [website](www.10xlearner.com) :wink:

## Problematic

When working on CI setup for some projects, I ended up having to install a program on a OSX environment which is only release via a mpkg file. 

Since you can only interact with files via command line on such environment, you can't double click to extract the elements from the mpkg file, so we need to find another way.

## Solution

After some googling, I finally encountered some articles talking about the command `installer`, and how it could be used on a mpkg file.
Once I had experimented a little bit with this command, I ended with the working following shell instruction:

```shell
sudo installer -pkg <path/to/file.mpkg> -target /
```

Maybe some explanation are necessary :laughing:

I used the `sudo` keyword since I needed the administrator rights to be able to install the program at the root of computer.
Then `-pkg` flag allows us to specify the location of the mpkg file.

Finally the `-target` flag allows us to specify where we are going to install the programs contained in the mpkg file.
By putting the root of the system (`/`), we tell the computer that we want our program to be installed into the system part of the computer.
Doing this that way, the program I experimented on ended up in the folder `/Applications` of my machine.

And like that, I have been able to automatically unpack the mpkg file on a OSX system.

---------

Thank you all for reading this article,
And until my next article, have a splendid day :wink:


## Interesting links

- [Codewall reference](https://coderwall.com/p/lncteg/install-mpkg-package-from-commandline-on-osx)

