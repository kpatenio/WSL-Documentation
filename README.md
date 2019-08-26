# WSL-Documentation

This is a repository containing personal notes, which will be based on my own experiences using Windows Subsystem for Linux.

## Personal Setup
- Windows 10 Home (1903)
- WSL 1.0 - Ubuntu 18.04.2 LTS “[Bionic Beaver](http://releases.ubuntu.com/18.04/)”

For the time being, I will not be using [PPAs](#PPAs) to install software.

## The First Step
It is a good idea to run `sudo apt update` before attempting to install anything else. This command updates the [package index of apt](https://help.ubuntu.com/lts/serverguide/apt.html), meaning that we update what versions of packages are found by `apt`. It reads `source.list` within the `/etc/apt` directory to find information on what versions to download and install; note that [repositories](#what-are-repositories) are defined in this specific file.

## Repositories && PPAs (Personal Package Archives)
### What are repositories?
According to [Network World](networkworld.com/article/3305810/how-to-list-repositories-on-linux.html):
> A Linux repository is a **storage location from which your system retrieves and installs OS updates and applications**. Each repository is a collection of software hosted on a **remote server and intended to be used for installing and updating software packages** on Linux systems. When you run commands such as “sudo apt update” or “sudo apt upgrade”, you may be pulling package information and package updates from a number of repositories.

Ubuntu, specifically, has four official repositories, according to [It's Foss](https://itsfoss.com/ppa-guide/):
> Each Ubuntu version has its own official set of four repositories:
> 1. Main – Canonical-supported free and open-source software.
> 2. Universe – Community-maintained free and open-source software.
> 3. Restricted – Proprietary drivers for devices.
> 4. Multiverse – Software restricted by copyright or legal issues.

### PPAs
Personal package archives, essentially, are packages reserved for _non-standard_ updates. PPAs are stored and created in [LaunchPad](https://wiki.ubuntu.com/Launchpad). Launchpad enables users to create their own PPAs, report bugs, and collaborate - basically, to share their own distribution of software. 

[It's Foss has a great explanation of why PPAs are useful](https://itsfoss.com/ppa-guide/):
>  Why not just distribute a DEB package that can be installed graphically?
>
> The answer lies in the update procedure. If you install a software using a DEB package, there is no **guarantee that the installed software will be updated to a newer version when you run `sudo apt update` && `sudo apt upgrade`**.
>
> It’s because the apt upgrade procedure relies on the sources.list. If there is no entry for a software, it doesn’t get the update via the standard software updater.
>
> So does it mean software installed using DEB never gets an update? No, not really. It depends on how the package was created.
>
> Some developers automatically add an entry to the sources.list and then it is updated like a regular software. Google Chrome is one such example.

## Installing `nodejs`

NodeJS can be installed multiple ways, including via package managers (although what version of node you get upon installation depends on the package manager you are using and its package index). This method assumes we are installing NodeJS via `apt` only.

For the full and official documentation, see [here](https://github.com/nodesource/distributions/blob/master/README.md). 

### Node 10.x
```
# For ubuntu
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
```
Note that `-y` allows us to say yes in the "confirmation" prompt that appears whenever we try to install a program using `apt`.

> ⚠️ **Danger** ⚠️: installing software via `curl -sL https://example.com/some-script.sh | sudo -E bash -` means you're
> allowing an external script (e.g., <https://deb.nodesource.com/setup_10.x>) to run on your computer with `sudo`, which
> usually means, it has **full control** over your system and its data. **Only do this if you 100% trust the source of the script** and if you can **verify its authenticity**.
>
> A malicious user could literally write `rm -rf --no-preserve-root /` and wipe your computer this way! 😵💀
>
> For this reason, it's best to download the script first, inspect it line-by-line to make sure it isn't upto any funny buisness, the run it.
>
>     curl -sL https://deb.nodesource.com/setup_10.x -o setup_10.x.sh
>     less setup_10.x.sh   # take a look at the file
>     sudo -E bash setup_10.x.sh  # Now we can run the script

#### Personal Experience
I initially ran `sudo apt install nodejs`, but this gave me node version 8.x.

The reason it installs ~8.x is because...

 1. apt consults its package index
 2. the package index finds the package `nodejs` in the `Universe` respository.
 3. The `Universe` repository **locks versions** for each Ubuntu release. This is so that if you install say Ubuntu 18.04 LTS on many different , you get a consistent set of packages, and have a lower likelihood of things breaking due to updates.
 4. [The version of NodeJS locked for Ubuntu 18.04 is NodeJS 8.x](https://packages.ubuntu.com/bionic/nodejs)

To update to a newer version, I had to use [nvm](#installing-nvm). To use `nvm` however, I also had to install [npm](#installing-npm). (Comment from Eddie: that's so weird... `nvm` is a series of Bash scripts; it should _not_ require npm at all! 🤔🤔🤔)

## Installing `npm`
### Using no PPAs
I installed `npm` using the following commands:
1. `sudo apt install npm`
2. `sudo npm install npm@latest -g`

I did not get the latest version of npm—the version locked for [Ubuntu Bionic is 3.5.2](https://packages.ubuntu.com/bionic/npm). I had to run the second command to upgrade to the latest version.

`npm` runs _in_ `nodejs`. Hence, a required package to install `npm` is `nodejs`. You can see all of `npm`'s dependent packages on the Ubunutu 18.04 Bionic's package page for `npm`: <https://packages.ubuntu.com/bionic/npm#pdeps>

### Using PPAs
There are several good resource explaining the steps for installing npm using PPAs, like [this](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-18-04) and [this](https://tecadmin.net/install-latest-nodejs-npm-on-ubuntu/).

Note about `apt-get`: see [here](https://itsfoss.com/apt-vs-apt-get-difference/).

## Installing `nvm`
Official documentation can be found [here](https://gist.github.com/d2s/372b5943bce17b964a79#installing-nodejs-with-nvm-to-linux--macos--wsl). Also see [Installing nodejs](installing-nodejs).

### Python
By default, Bionic Beaver comes with [Python 3.6](https://packages.ubuntu.com/bionic/python/). This is for compatibility reasons. To install Python 3.7, I had to install it separately. There are several tutorials that [show how to do this](https://linuxize.com/post/how-to-install-python-3-7-on-ubuntu-18-04/). However, considering that I did not want to use PPAs, I instead just ran the following:

```
sudo apt install python3.7
```

By doing this, I was able to get _some_ Python. I say "some" because it did not install other Python 3.7 compatible libraries that were already found on my system via Python 3.6. For example, I had to install `pip` serparately for `3.7`. Admittedly, I don't recall how I got `pip` for that specific version hehe. There are several resources however that show how to do so. For example, check out this [SO question here!](https://stackoverflow.com/questions/54633657/how-to-install-pip-for-python-3-7-on-ubuntu-18).
