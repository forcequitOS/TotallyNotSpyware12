# TotallyNotSpyware12
<p align="center">
<img src="https://github.com/forcequitOS/TotallyNotSpyware12/blob/main/showcase.png?raw=true" width="60%">
</p>

## An improved remix of totallynotspyware-v2 for iOS 12
**You can try it now at https://tns12.forcequit.cc!**

**Alternatively,** if you so please, you can also re-jailbreak your device with a [Shortcut.](https://raw.githubusercontent.com/forcequitOS/TotallyNotSpyware12/main/Jailbreak%20Me.shortcut) This is way less tested and supported, but it does work.

This modification makes re-jailbreaking more frictionless by skipping all popups, gives additional configuration options, and can be **added to your Home Screen to be run offline.**

While this is an unofficial modification of totallynotspyware-v2, I can say rather confidently that reliability and safety should be the same as the original, and you can personally check my source code to verify this (if you so desire.)

## How is this better?

- Add to your Home Screen to run like a native app, including an app icon and complete offline support
- A Safe Mode to disable tweak injection to recover from buggy tweaks, without the Chimera app installed, in case a bad tweak screws you up
- Complete user interface overhaul with animations and a somewhat native style and tons of little touches that come with it
- Userspace reboot (or just respring, if that's your thing) immediately after re-jailbreaking, without a single popup

## Troubleshooting / Info

**Manually reloading the web app** - While in the web app, press and hold the Top/Side button until you reach the "slide to power off" screen, then press and hold the Home button until you reach the Home Screen.

**"A problem repeatedly occurred..."** - If you're in Safari and this message appears while you're trying to rejailbreak, reload the page. If you've added the page to your Home Screen, and solely wish to jailbreak from there, see above to reload the web app, or restart your device.

**Device reboots straight to Apple logo after "Jailbreaking..." for a few seconds** - Leave your device powered on and unlocked for ~30-60 seconds before retrying again, it'll work eventually.

**Failed, retry?** - The re-jailbreak is failing for some reason. One possible cause for this can be that your device is already jailbroken. If retrying doesn't help, refresh the website, reload the web app, or restart your device.

**Device doesn't reflect updates to site** - If you're using the web app, see earlier instructions to manually reload the web app, and/or reboot your device. If you're not, I probably didn't update the AppCache manifest. Go to Settings > Safari > Clear History and Website Data and it'll get new data from the server.

**Unable to remove problematic tweaks** - If you can't remove any tweaks due to your device crashing when trying to reboot to a jailbroken state, you can disable tweak injection temporarily by setting the NVRAM `boot-args` variable to `-safe-mode` using either an SSH RAM disk, or the `irecovery` command line tool on a computer, with your device connected in Recovery mode. You can look below for some more in-depth instructions for doing this.

**If you're having an issue and it's not mentioned here, and you're sure that you've done everything right, file a GitHub Issue and I'll try to get back to you.**

## Configuring TotallyNotSpyware12

TNS12 reads the NVRAM `boot-args` variable post-exploitation, and so I've implemented a couple of options you can choose between.

`-safe-mode` - Disables tweak injection entirely when re-jailbreaking, you'll be given an option to disable Safe Mode when your device re-jailbreaks. This replaces the "stay" option on the popup in totallynotspyware-v2 as your device will not respring or userspace reboot when using this option if you leave Safe Mode enabled.

`-respring` - Respring your device after re-jailbreaking, rather than performing a userspace reboot. This is quite a bit faster to just get back to the Home Screen, and tweaks will be injected into both apps and SpringBoard, but some stuff can be broken, so it's not really recommended. But, this is really fast.

`-userspace-reboot` - Userspace reboots your device after re-jailbreaking, as most traditional semi-untethered jailbreaks (including regular Chimera) do. A userspace reboot is also the default option if no boot arguments are specified. 

**If you're able to get to a jailbroken state,** you can use the command `nvram boot-args="[your_choice_here]"`, or leave the space in between the quotes blank to choose no argument (Which will userspace reboot your device by default). You can use the `nvram` command from a terminal app on your device (like NewTerm), or over SSH with a computer. Setting NVRAM variables is *also* possible using the `nvram` command via an SSH RAM disk if you're **unable to boot to jailbroken iOS**, or if you don't want to use the following option.

**If you're unable to get to a jailbroken state,** by using the `irecovery` command line tool on a computer, you can set your device's NVRAM `boot-args` variable from standard Recovery mode, usage goes as follows:

```
irecovery -s

setenv boot-args [your_choice_here]

saveenv

reboot
```

I may, in the future, work on some simpler alternative tools you can use to configure these options. 

## But as always, there is ~~one~~ a few more thing(s).

I have a couple of things in mind for how I can improve this in the future, this is my current list of ideas, some of these are far more likely to happen than others:

- An iPad-optimized layout, for, you guessed it, iPads. I'm almost definitely going to work on this myself at some point whenever I get my hands on an iOS 12 iPad.
- Improving the background gradient two, electric boogaloo (I probably could have done this before throwing the 1.2.0 release at the world, but oh well), I basically just want better colors. The old background gradient I was utilizing before had some bugs on iOS 12 with animation (which you barely would see, anyways).
- An "Options" application to use while in a jailbroken state to configure the NVRAM boot arguments used by TNS12. Believe it or not, I did make an attempt to do this, but never was actually able to write to NVRAM successfully (only read), since I could never gain root privileges. Not to mention that UIKit, nor jailbroken iOS app development, are things I'm very skilled with.
- A shell script for macOS / Linux to just choose one of the three boot argument options from recovery / DFU mode without manually using `irecovery`. This is easy. I just don't think I care enough to do it. 
- Making the site not bounce around and be scrollable when it doesn't need to be (Fixing this, specifically targeting iOS 12, seems legitimately impossible. Good luck if you try to fix this.)
- Reducing lag on animations (This is not happening.)
- Restoring RootFS (Likely via an NVRAM boot argument like `-restore-rootfs`) would be a nice addition, but not really needed, as you can use [SuccessionRestore](https://github.com/Samgisaninja/SuccessionRestore) to clean the filesystem on-device (assuming that you can still jailbreak enough to at least get to safe mode), or just temporarily sideload Chimera to restore RootFS. It doesn't seem horribly difficult to do within the post-exploit stage3 environment, I just can't personally be bothered. 
- Bootstrapping and installing the jailbreak to make this a fully independent jailbreak really would be quite nice. However, I can't really be bothered to do that at the moment, and I don't know if I ever will be, seeing as it works really well as-is and it's not much effort to just install Chimera, jailbreak with it once, and then delete it. If someone else would like to contribute this change, I'd appreciate it, or if someone else makes a fork that can bootstrap the jailbreak, I will gladly incorporate my UI and tweaks on top of that.

If anyone would like to help contribute to these, I'd appreciate it, but if not, I'll probably get around to doing at least some of them myself at some point when time allows and I get bored on a Saturday night again.

Also, during this project, I had quite a few thoughts about a potential full open-source iOS 12 jailbreak (or even just Chimera going open-source). I don't think I quite have the skills to pull this one off, but it would be cool to see it happen. 

All of the modified post-exploitation payload's source code can be found [right here.](https://github.com/forcequitOS/TotallyNotSpyware12-Payload)

<sub>I've probably used the actual absolute messiest CSS I've ever written in my life to make this, oh well.</sub>