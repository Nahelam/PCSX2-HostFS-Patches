# PCSX2 HostFS Patches

## HostFS?
HostFS stands for "Host File System", a protocol allowing game files to be loaded directly from the "host" (development) machine file system.

This feature was initially available on PlayStation 2 development kit consoles.

## Advantages
From the PlayStation 2 modder point of view this feature is a powerful way to perform game files modifications as it completely eliminates the hassle of disc image repacking.

Depending on how the game loads its files, it may also be possible to edit them while it is running and make the modifications effective without having to reboot.

And of course, loading times are reduced.

## PCSX2 support
[PCSX2](https://github.com/PCSX2/pcsx2) has a built-in HostFS support but keep in mind that you will have to make the magic happen on the game side if you want to be able to run it through this protocol, if it isn't supported already.

Here are the existing HostFS patches that I know of:
- [Gran Turismo 4](https://nenkai.github.io/gt-modding-hub/ps2/gt4/volume/#setting-up-hostfs-recommended-method) by [Nenkai](https://github.com/Nenkai)
- [Persona 3 FES, Persona 4 and SMT III](https://shrinefox.com/guides/2020/04/10/modding-using-hostfs-on-pcsx2-p3-p4-smt3/) by [ShrineFox](https://github.com/ShrineFox)


## My contribution to the HostFS world
- [Criterion Games HostFS](https://github.com/Nahelam/PCSX2-HostFS-Patches/tree/main/Criterion%20Games)
