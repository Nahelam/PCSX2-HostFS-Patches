# Criterion Games HostFS

After learning about [HostFS](https://github.com/Nahelam/PCSX2-HostFS-Patches/tree/main) I wanted to make the magic happen on Burnout 3 and it turned out that Criterion Games left us a really cool [IOP module](https://github.com/RetroReversing/retroReversing/blob/master/pages/consoles/ps2/irxFiles.md) in their game files which does almost all the job for us, and it works on all their PlayStation 2 games!

By default, the games are loading an IOP module called `GTFSCDVD.IRX` during the boot sequence which is responsible of almost all disc file system operations.

I noticed that in some of their games, a module called `GTFSHOST.IRX` was left, I tried to make them load it instead of `GTFSCDVD.IRX` and guess what? They were now trying to read files from the host device! Isn't that nice?

However to be able to load the full game extracted in a folder I still had to patch the hardcoded paths that are used in the boot sequence to load IOP modules as these operations don't go through `GTFSHOST.IRX`.

To do so, I wrote a tiny stub that replaces modules paths (cdrom -> host) and the file system module name (GTFSCDVD.IRX -> GTFSHOST.IRX), then I made the module loading functions call this stub, that's it!

## Setting up PCSX2
1. Create a new folder and move your game disc image into it
2. Extract the game disc image in the folder you just moved it to using the extractor of your choice (I personnally use 7-Zip)
3. Add the folder you just created in the PCSX2 game scanning list, deny recursive scan
4. Download the HostFS patch corresponding to your game region from this repository and place it into the PCSX2 `cheats` folder
5. On PCSX2 right click on your freshly scanned game, select `Properties` then:
   - In the emulation settings make sure `Host Filesystem` is checked
   - In the cheats settings, make sure `Enable Cheats` and `HostFS` are checked
6. Start the game, if you did everything correctly it should start normally

### Additional steps (optional)
Now that you can start your game through HostFS, you may want to get rid of the disc image you copied and extracted earlier.\
On the other hand PCSX2 requires a disc image to be present in the folder to be able to detect a game and its cheat list correctly.\
One way that I found to work around this issue is to generate a "minimal" disc image only containing the necessary data for PCSX2.

Here are the additional steps to do so:

7. On PCSX2 top menu, select `Tools` then `Save CDVD Block Dump`
8. You will be prompted to select a folder, select the one that we are currently working in
9. Right click on the game then select `Boot and Debug`, you will get a black screen, this is intended
10. Wait a few seconds then shut down the game and close the debugger if it opened
11. A file with the `.dump` extension should have been created in our folder, this will be our new "minimal" disc image
12. On PCSX2 top menu, select `Tools` then uncheck `Save CDVD Block Dump` as we don't need it anymore
13. Rename the `.dump` file to whatever you want but make sure to change its extension to `.iso`
14. On PCSX2 top menu, select `Settings` then `Rescan All Games`, if everything has been done correctly you should see your game twice
15. Now move your full game disc image somewhere else and only leave the "minimal" one
16. `Rescan All Games` again then try to start the game, it should start normally
17. If the game started normally and you were planning to remove the full game disc image, it's time!
    
## Games with missing HostFS module
There are actually three games that are missing the `GTFSHOST.IRX` module in their IOP folder: AirBlade, Burnout Dominator and Black, fortunately you can copy the module from the other games having it, I can confirm the following compatiblity:
- Burnout Dominator and Black are working fine with Burnout Revenge `GTFSHOST.IRX`
- AirBlade is working fine with Burnout 1 `GTFSHOST.IRX`

## Caveats
- In Burnout 3 you won't be able to boot the NFS demo (lol)
- In Burnout 3 and Burnout Revenge you won't be able to boot into the network configuration utility, make sure you already have one saved in your memory card if you want to play online
- In Burnout 3, loading a different IOP module than usual will invalidate the IOP address if you are using the [Play Online](https://github.com/Nahelam/PCSX2-Burnout-Mods/tree/main/Burnout%203%20Takedown/Play%20Online) patches, you can either manually patch `DRTYSCK.IRX` located in `IOP/NETWORK/` or get it from an online pre-patched disc image, actually, if you got an online pre-patched Burnout 3 disc image, do all the steps with it and you won't face this issue.
