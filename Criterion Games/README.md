# Criterion Games HostFS

After learning about [HostFS](https://github.com/Nahelam/PCSX2-HostFS-Patches/tree/main) I wanted to make the magic happen on Burnout 3 and it turned out that Criterion Games left us a really cool [IOP module](https://github.com/RetroReversing/retroReversing/blob/master/pages/consoles/ps2/irxFiles.md) in their game files which does almost all the job for us, and it works on all their PlayStation 2 games!

By default, the games are loading an IOP module called `GTFSCDVD.IRX` during the boot sequence which is responsible of almost all disc file system operations.

I noticed that in some of their games, a module called `GTFSHOST.IRX` was left, I tried to make them load it instead of `GTFSCDVD.IRX` and guess what? They were now trying to read files from the host device! Isn't that nice?

However to be able to load the full game extracted in a folder I still had to patch the hardcoded paths that are used in the boot sequence to load IOP modules as these operations don't go through `GTFSHOST.IRX`.

To do so, I wrote a tiny stub that replaces modules paths (cdrom -> host) and the file system module name (GTFSCDVD.IRX -> GTFSHOST.IRX), then I made the module loading functions call this stub, that's it!

## Setting up PCSX2
1. Create a new folder and move your game disc image (.iso) into it
2. Extract your game disc image files in the folder you just moved it to (<ins>don't remove it once extracted!</ins>) using the extractor of your choice (I personnally use 7-Zip)
3. Add the folder you just created in the PCSX2 game scanning list, deny recursive scan
4. Download the HostFS patch corresponding to your game region from this repository and place it into the PCSX2 `cheats` folder
5. On PCSX2 right click on your freshly scanned game, select `Properties` then:
   - In the emulation settings make sure `Host Filesystem` is checked
   - In the cheats settings, make sure `Enable Cheats` and `HostFS` are checked
6. Start the game, if you did everything correctly it should start normally

At this point PCSX2 will only read `SYSTEM.CNF` and the game executable from your disc image, everything else will be read directly from the folder, follow the additional steps if you want to shrink your disc image to a "minimal" one so it will only contain these two files.

Making PCSX2 load the game ELF executable directly from the folder was considered but there was some issues on the UI side, the detection of the game and the toggleable cheats list weren't working properly so I decided to stick with the shrinked disc image method for this tutorial.

### Additional steps (optional)
Now that you can start your game through HostFS, you may want to shrink the disc image that still have to reside in your folder.

To do so, create a new disc image which only contains the game binary and the `SYSTEM.CNF` file using an external tool of your choice.

Here is an example using `mkisofs` from [cdrtools](https://sourceforge.net/projects/cdrtools/files/alpha/win32/cdrtools-1.11a12-win32-bin.zip/download):

https://github.com/Nahelam/PCSX2-HostFS-Patches/assets/128867759/f8748a8d-a063-4f84-a695-8985f503fd41
    
## Games with missing HostFS module
There are actually three games that are missing the `GTFSHOST.IRX` module in their IOP folder: AirBlade, Burnout Dominator and Black, fortunately you can copy the module from the other games having it, I can confirm the following compatiblity:
- Burnout Dominator and Black are working fine with Burnout Revenge `GTFSHOST.IRX`
- AirBlade is working fine with Burnout 1 `GTFSHOST.IRX`

## Caveats
- In Burnout 3 you won't be able to boot the NFS demo (lol)
- In Burnout 3 and Burnout Revenge you won't be able to boot into the network configuration utility, make sure you already have one saved in your memory card if you want to play online
- In Burnout 3, loading a different IOP module than usual will invalidate the patches relying on IOP addresses, if you are using the [old network play patches](https://github.com/Nahelam/PCSX2-Burnout-Mods/tree/main/Burnout%203%20Takedown/Network%20Play/Old%20Patches), please use the [new ones](https://github.com/Nahelam/PCSX2-Burnout-Mods/tree/main/Burnout%203%20Takedown/Network%20Play) instead.
