# Chocolatey and Gist

Hello ! I'm Xavier Jouvenot and in this small post, we are going to see how to link a gist to a chocolatey package maintainer repository.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) 😉

## Introduction

I assume that if you click to see this post, you already know what [Chocolatey] and [Gist] are, but if this is not the case I encourage to go google about them.

In this post, we are going to focus on the [Chocolatey Automatic Package Updater Module](https://github.com/majkinetor/au), aka AU, and how to link it to a Gist document so you can have a public report expliciting the state of all the packages that you are maintaining. 🙂

## Setting it up

### Appveyor report upload

Once you have created you own fork of the [chocolatey-packages-template](https://github.com/chocolatey-community/chocolatey-packages-template) repository, let's go in the [.appveyor.yml](https://github.com/chocolatey-community/chocolatey-packages-template/blob/master/.appveyor.yml) file and look to the two following lines

```yml
gist_id:

gist_id_test:
```

Those are the fields that are going to interest us.
Indeed, by giving then the id of our Gist, it will link the result of your tests in appveyor to your Gist.

To identify the id of your Gist, all you have to do, is to create a new Gist, name it `Update-AUPackages.md` and take the id from the url of your gist.
Know that your Gist url is form like `https://gist.github.com/name/id` so all you have to do is to get the last component of the url and paste it after each of the `gist` fields. 🙂

### Link your report to your README

You can also create a link in your `README.md` file to your Gist report by adding a badge.
To do so, you can copy-paste the following line at the top of you read me (after replacing `<your/gist/url>` by your actual gist report url):

```md
[![Update Status](https://img.shields.io/badge/Update-Status-blue.svg)](<your/gist/url>)
```

Moreover, if you want, you can go to [img.shields.io](https://img.shields.io/) to create your own custom badge 😉

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [Chocolatey Automatic Package Updater Module](https://github.com/majkinetor/au)
- [My Update AU Packages](https://gist.github.com/Xav83/c4eb33a8aa993629f3d047747322e809)
- [10xlearner website](www.10xlearner.com)

