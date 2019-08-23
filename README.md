# WSL-Documentation

This is a repository containing personal notes, which will be based on my own experiences using Windows Subsystem for Linux.

## Personal Setup
- Windows 10 Home (1903)
- WSL 1.0 - Ubuntu 18.04.2 LTS

## The First Step
It is a good idea to run `sudo apt update` before attempting to install anything else. This command updates the [package index of apt](https://help.ubuntu.com/lts/serverguide/apt.html), meaning that we update what versions of packages are found by `apt`. It reads `source.list` to find information on what versions to download and install; note that [repositories](#what-are-repositories)

## Repositories && PPAs (Personal Package Archives)
### What are repositories?
According to [Network World](networkworld.com/article/3305810/how-to-list-repositories-on-linux.html):
> A Linux repository is a **storage location from which your system retrieves and installs OS updates and applications**. Each repository is a collection of software hosted on a **remote server and intended to be used for installing and updating software packages** on Linux systems. When you run commands such as “sudo apt update” or “sudo apt upgrade”, you may be pulling package information and package updates from a number of repositories.



### PPAs
https://askubuntu.com/questions/4983/what-are-ppas-and-how-do-i-use-them

## Installing `nodejs`
NodeJS can be installed multiple ways, including via package managers like npm (although what version of node you get upon installation depends on the package manager you are using). This method assumes we are installing NodeJS via the terminal only.

### Node 10.x
```
# For ubuntu
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
```
Note that `-y` allows us to say yes in the "confirmation" prompt that appears whenever we try to install a program using `apt`.

#### Personal Experience
I initially ran 

Official documentation can be found [here](https://github.com/nodesource/distributions/blob/master/README.md).

### Python
TODO
