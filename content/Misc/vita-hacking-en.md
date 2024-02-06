+++
title = 'Modding the PS Vita'
date = 2024-02-03T16:23:00Z
publishDate = 2024-02-07T16:23:00Z
draft = false
tags = ['Misc','PS Vita']
ShowToc = true
+++

I recently modded my PS Vita, using [HENkaku](http://henkaku.xyz/) and [Ensō](https://enso.henkaku.xyz/), so that I was able to install unsigned software, and begin exploring the Vita homebrew community, and potentially make my own games/programs for the system!

## Disclaimers

**I do not support pirating games, this project was focused on exploring the Vita system and the homebrew created for it!**

I do not claim responsibility for any consequences if you attempt this with your own device, you do so at your own risk and may void the manufacturer's warranty!

This post also does not serve as a comprehensive set of instructions for modding a PSV, please see https://vita.hacks.guide/ for guides to installing custom firmware!



## Prerequisites

For the method I chose, a combination of [HENkaku](http://henkaku.xyz/) and [Ensō](https://enso.henkaku.xyz/), the system needs to be using either firmware 3.60 or lower. Other methods are available for systems updated beyond this point, but they are not covered here. My PSV hadn't been used for many years at this point, so it wasn't updated past this point.



## HENkaku and Molecularshell

The first step I took was to run HENkaku on the PSV. HENkaku is a Japanese word meaning "to change radically, to make something new", which is definitely true, as this process revived the interest I had in what is admittedly a pretty forgotten console by now.

HENkaku is an exploit chain for the PSV which allows the installation of homebrew applications on the home screen, and is not persistent, meaning that it does not remain installed when rebooting the system. For this reason, Ensō must be installed later in order to retain access to these features once the device is rebooted.

### Instructions

In order to install HENkaku, launch the internet browser on the PSV and go to `http://henkaku.xyz/`. After clicking 'Install', the PSV will launch the exploit, and if the exploit is successfully completed, a new icon will appear on the home screen, called 'MolecularShell'. 

<img title="" src="https://imgur.com/0A6Kn7L" alt="">

<img title="" src="https://www.cfwaifu.com/wp-content/uploads/2019/07/molecularshell-ftp-e1562584017733.jpg" alt="">

MolecularShell, and a similar program VitaShell, are effectively file managers, used to view and transfer files on the device. MolecularShell can be used to host an FTP server for file transfers, or to transfer files via USB connection too. Another significant feature is that it allows us to install files such as `.vpk`,`.pkg` and many others, to install new unofficial programs created by the community.

Next, navigate to the normal Settings app on the PSV, and you will see a new 'HENkaku Settings' tab. Here, check 'Enable Unsafe Homebrew'. This is required for the system to allow the custom programs to be installed.

## Ensō

<img title="" src="https://modernzen.org/wp-content/uploads/2019/08/tumblr_lonpj9kqhp1qashouo1_1280.jpg" alt="">

Ensō is a sacred symbol in the Zen school of Buddhism, and can be loosely translated as "Mutual Circle". This is pretty appropriate, as this is the software which allows the previous modifications to persist through reboots, so that the PSV can be used as normal.

It runs an exploit at boot time to set up the HENkaku environment automatically, and is much more convenient than opening the internet browser each time. Additionally, it does not require an internet connection, as HENkaku's manual installation does.

### Instructions

First, we have to download both [Enso](https://github.com/henkaku/enso/releases/tag/v1.1) and [VitaShell](https://github.com/TheOfficialFloW/VitaShell/releases/tag/v2.02) to our PC - only the `.vpk` files are required here.

Inside either MolecularShell or VitaShell, press SELECT to host an FTP server on the device, and connect with a client such as WINSCP or Filezilla. Alternatively, a connection could be made through a micro USB cable.

To install a `.vpk` file, move it to `ux0:data/`, and run the `.vpk` file inside Molecular or Vita Shell.

Once these apps are both installed, find Enso on your home screen and run it. Once installation has completed, press X to reboot your device, and the exploit should now remain installed on your device permanently (or at least until you choose to uninstall it, more importantly, it will not dissapear after rebooting now).

Reccomended software and next steps can be found [here](https://vita.hacks.guide/finalizing-setup-(3.60).html), and will allow you to things such as connecting to PlayStation Network on a modded console, or provide some nice quality of life improvements to the PSV.

## Ripping Games from Cartridges

With MolecularShell or VitaShell installed, it is possible to rip games from their retail cartridges and install them on the internal storage, or your memory card, so that they can be played without inserting the card, like a downloaded game.

With the game card inserted into the PSV, a new drive will be visible inside VitaShell, `gro0`. `ro` means Read-Only, so don't worry about accidentally wiping your only copy of *Minecraft: PS Vita Edition* off the card :) . On this drive, you will see all the files that are on the game card. You will see a folder with a name something like `LLLLDDDDD/`, where L is a letter and D is a digit, for example, Minecraft is `PCSB00560/`. 

Hit Triangle, and in the menu that appears, select Copy, and then navigate to `ux0:app/`, and hit Triangle again and select Paste to copy the game to your internal storage. Once you've done this, you may need to navigate back to the root of VitaShell, where all the drives are visible, and select "Refresh LiveArea" in the Triangle menu in order to see the game on your home screen.

Now, the game should be playable without having the card installed! Bear in mind that if you insert the game card again, the system may ask you which version you want to use, and make sure to choose the right option here, as your save games on the copied version will not be available if you choose to run the game from the card! Though they will remain safe and usable once the card is removed again, and the newly copied version is the only version available to the PSV.

## SD2Vita

I'll keep this section brief, but a common problem you'll experience once a few games are installed is that the memory cards sold by Sony are ridiculously expensive compared to microSD cards, and are a proprietary format, so they are not able to be used with your PC unless the Vita is used as an adapter, through VitaShell's USB connectivity functionality.

A SD2VITA adapter can be bought online, which adapts a microSD card to fit into the game cardridge slot on the top of the device. With this, you can potentially have a Terabyte of storage, at a fraction of the cost of Sony's cards.

Check out [this guide](https://vita.hacks.guide/yamt.html) for more information!

## Future Work

- I'm exploring the [Vita SDK](https://vitasdk.org/), and plan on creating at least a couple simple programs on the device, potentially a small game, depending on how the process goes!

- I would like to create a program that allows you to use your PSV as a USB game controller with your PC
