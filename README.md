# Installing Windows 10 Education v1703 on Linux Mint 18.2

Most students know [Huskertech](http://sales.unl.edu/software) sells Windows 10 for $25. But lesser known is that students enrolled in [programs](http://cba.unl.edu/academic-programs/programs-and-degrees/) associated with the College of Business, such as:

* Accounting
* Actuarial Science
* Agribusiness
* Business Administration
* Economics
* Finance
* International Business
* Management
* Marketing
* Supply Chain Management

have access to a variety of **free** software from [Microsoft Imagine](https://en.wikipedia.org/wiki/Microsoft_Imagine) (formerly **DreamSpark**) including Windows 10. All graciously provided by [Student IT Resources](http://cba.unl.edu/people/itservices/msdnaa/login/cba-iuv.aspx) and your $110 technology fee :)

The Microsoft Imagine access portal (requires `My.UNL credentials`) can be found here:

http://cba.unl.edu/people/itservices/msdnaa/login/cba-iuv.aspx

![Imagine](https://github.com/Infinite-Actuary/Windows10-on-Linux/blob/master/images/microsoft-imagine.png)

After logging in, you can download the `windows_10_education_*.iso` (~4GB) and use it to create a bootable USB drive.

## Creating a bootable USB

Before actually succeeding, I tried:

* [Rufus](http://rufus.akeo.ie/) (using Wine)
* [UNetbootin](http://unetbootin.github.io/)
* Default utility `USB Image Writer` provided by Mint

All of which are great pieces of software, but simply didn't work for my use case. However, [WoeUSB](https://github.com/slacka/WoeUSB): "A Linux program to create Windows USB stick installer from a real Windows DVD or an image", worked great!

To actually install it I used:

```Bash
sudo add-apt-repository ppa:nilarimogard/webupd8
sudo apt update
sudo apt install woeusb
```


## Preparations - Make Space for Windows 10
* Make sure you have a [Live CD](https://en.wikipedia.org/wiki/Live_CD)!
* With a Live CD , use `Gparted` to create a (>20 GB) [NTFS](https://en.wikipedia.org/wiki/NTFS) formatted partition on `/dev/sda`

![Windows partition](https://github.com/Infinite-Actuary/Windows10-on-Linux/blob/master/images/windows10-partition.png?raw=true)

This is where your new installation of Windows will go!

## Restoring GRUB Boot Loader

Unfortunately I found that after installing Windows, it overwrites the boot sector. Thus, I no longer had access to Mint via the [GRUB](https://en.wikipedia.org/wiki/GNU_GRUB) boot loader just after the BIOS loads.

I used this wiki: https://help.ubuntu.com/community/RecoveringUbuntuAfterInstallingWindows

Along with Boot-Repair to restore the GRUB menu.

![boot-repair](http://pix.toile-libre.org/upload/original/1335260967.png)

So put your Live CD back in and install and run boot-repair with the following commands:

```bash
sudo add-apt-repository ppa:yannubuntu/boot-repair
sudo apt-get update
sudo apt-get install -y boot-repair && boot-repair
```

## Activate Windows Using Product Key

Don't forget this step, as you won't even be able to personalize your Desktop without doing it. If you didn't write it down don't worry. You can find it in the "purchase" details under your Microsoft Imagine account.

## Disabling Fast Startup

Fast startup is great. As you can imagine it greatly reduces the startup time for Windows. However, it also prevents me from mounting the drive from Mint.

![no-mount](https://github.com/Infinite-Actuary/Windows10-on-Linux/blob/master/images/windows10-mount.png)

I found several ways to (possibly) remedy this:

* Mounting in read only mode
* Mount with the `remove_hiberfile` option
* Using `ntfsfix` on the Windows partition to terminate the hibernated session

Ultimately I decided to simply [turn off](https://in.answers.acer.com/app/answers/detail/a_id/37059/~/windows-10%3A-enable-or-disable-fast-startup) the fast startup feature in Windows. You can also disable hibernation from the command prompt (`cmd`) with `powercfg /h off`.
