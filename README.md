<h1 align="center"> Dying Light Linux Modding Guide </h1>

---

### Ever wanted to mod Dying Light on Linux and play with friends ? But you can't because modding Data3.pak & using it makes the online not working ? Well fear not ! This guide is for you.

---

## What you will need for it :
- A **Legit copy** of the game on Steam (c'mon, it's only 5 bucks and sometimes even cheaper)
- Steam Native Runtime (I am not using the Flatpak version and I will not make a tutorial for it)
- A computer running Linux (Debian, Arch, RedHat, Fedora, doesn't matter, as long as it runs Linux, I will be using Arch during this guide, even though it doesn't matter)
- nloginov's modpack (You'll need it for dide_mod & dsound.dll, you can find it [here !](https://nlog.us/downloads/load_online.zip))
- A mod (In this example, we're gonna use [CLM Weapons Mod](https://www.nexusmods.com/dyinglight/mods/202?tab=files))
- Some common sense :)

---

## Quick Rundown :

- Identify your steam install path, proton version/install path, Dying Light install path, and Dying Light "compatdata" path
- Download and extract nloginov's modpack & the mod you downloaded, and navigate with the terminal to the extracted directory
- Set terminal variables
```
STEAMINSTALLDIR="YOUR STEAM CLIENT INSTALL DIRECTORY"
PROTONDIR="YOUR PROTON DIRECTORY"
DLDIR="YOUR DYING LIGHT COMPATDATA DIRECTORY"
```
- Extract the mod you downloaded into a folder of your choice (For my mods, I've created a folder named DLMods in my Documents folder)
- Extract the "Online mod usage" file and put the dide_mod.ini & dsound.dll inside your Dying Light installed folder 
- Open wine configuration for the Dying Light prefix:
```
STEAM_COMPAT_CLIENT_INSTALL_PATH="${STEAMINSTALLDIR}" STEAM_COMPAT_DATA_PATH="${DLDIR}" WINEPREFIX="${DLDIR}/pfx" "${PROTONDIR}/proton" run winecfg
```
- Set library override for dsound to (builtin,override)
- Success

---

Guided tutorial for the less nerdy people : 

Some versions of Proton require an environment variable specifying the steam installation directory. When proton initializes a new game prefix, certain dlls are copied from this location (and they're confirmed and/or updated at every launch within the proton prefix). For most people, the directory we want is either ${HOME}/.steam/steam or ${HOME}/.local/share/Steam. My Steam install looks like this.
```
❯ ls ${HOME}/.local/share/Steam
 .                      friends        resource         userdata                        output_log.txt                                 ThirdPartyLegalNotices.doc
 ..                     graphics       servers          .crash                          standalone_installscript_progress_228980.vdf   ThirdPartyLegalNotices.html
 appcache               legacycompat   steam            bin_steamdeps.py                steam.sh                                       update_hosts_cached.vdf
 bin                    linux32        steamapps        bootstrap.tar.xz                steam_msg.sh
 clientui               linux64        steamui          d3ddriverquery64.dxvk-cache     steam_subscriber_agreement.txt
 compatibilitytools.d   logs           steamui-public   fossilize_engine_filters.json   steamclient.dll
 config                 music          tenfoot          GameOverlayRenderer64.dll       steamclient64.dll
 controller_base        package        ubuntu12_32      installscriptevalutor_log.txt   steamdeps.txt
 depotcache             public         ubuntu12_64      local.vdf                       ThirdPartyLegalNotices.css
```
If you get a directory not found error or empty listing, you probably have a non-standard install location and will have to track it down yourself. Something like ```locate steamclient.dll``` might help.

Next, identify the version of proton that you're using to launch the game. If you've set a specific override for Dying Light, it will be in the game's properties:
![image](https://github.com/AzhamProdLive/Dying-Light-Linux-Modding-Guide/assets/73955243/7986cb2d-e54a-4cd7-8034-df46255e0125)

Otherwise, look to Steam>Settings>Steam Play
![image](https://github.com/AzhamProdLive/Dying-Light-Linux-Modding-Guide/assets/73955243/39ae5a66-5c77-4e20-b607-76adb8d1182f)

We need to find the installed directory for the correct version of proton. For 8.0-5 shown in my screenshots, you'd look in your steam library for "Proton 8.0". Open it's properties, go to the local files tab, click browse, and a file browser should open to the correct path.
![image](https://github.com/AzhamProdLive/Dying-Light-Linux-Modding-Guide/assets/73955243/95c1767f-0896-488e-b25d-d07ec3bd18a7)

This is your "Proton directory"

Next, find your Dying Light installation directory. Go to Dying Light in your library, properties>local files>browse, and again, a file browser opens to the correct path.
![image](https://github.com/AzhamProdLive/Dying-Light-Linux-Modding-Guide/assets/73955243/94fc2257-be12-4d3e-873a-1ba065081e11)

This is your "Dying Light Install directory"

Finally, we need to find the compatdata directory for Dying Light. This is where the wine registry/libraries/etc live. If your Dying Light install directory was /foo/bar/steamapps/common/Dying Light, the compatdata directory should be /foo/bar/steamapps/compatdata/239140 (239140 is the game's App ID in Steam).
![image](https://github.com/AzhamProdLive/Dying-Light-Linux-Modding-Guide/assets/73955243/be80e710-542c-434e-8b92-8038f82f6a40)

This is your "Dying Light compatdata directory"

Let's set the paths that we've identified into variables for easier use. Open a terminal, and enter
```
STEAMINSTALLDIR="YOUR STEAM CLIENT INSTALL DIRECTORY"
PROTONDIR="YOUR PROTON DIRECTORY"
DLDIR="YOUR Dying Light COMPATDATA DIRECTORY"
```
so, for me, it's
```
STEAMINSTALLDIR="${HOME}/.local/share/Steam"
PROTONDIR="/home/azhmprd/.local/share/Steam/steamapps/common/Proton 8.0/"
DLDIR="/home/azhmprd/.local/share/Steam/steamapps/compatdata/239140/"
```
Put all paths in quotes. These variables are not directly accessed by Proton and don't need to be exported; they're only for our convenience to avoid retyping long paths (and allow you to copy/paste the commands below). They **ONLY APPLY TO THE TERMINAL WINDOW YOU TYPE THEM IN**, so we're going to use this window for everything. 

Now that we are done with this, let's extract dide_mod.ini & dsound.dll inside the Dying Light Install Directory. After it's done, go back to your terminal, and enter the following command :

```
STEAM_COMPAT_CLIENT_INSTALL_PATH="${STEAMINSTALLDIR}" STEAM_COMPAT_DATA_PATH="${DLDIR}" WINEPREFIX="${DLDIR}/pfx" "${PROTONDIR}/proton" run winecfg
```

In the "Libraries" tab, check your "Existing overrides" for dsound. If it isn't present, type dsound into the "New override for library" box and click "Add". In the end, you should have a dsound (native,builtin) line in your overrides
![image](https://github.com/AzhamProdLive/Dying-Light-Linux-Modding-Guide/assets/73955243/957ce880-b531-4986-95b9-168014195d36)

If it doesn't say (native,builtin), just highlight dsound, click "Edit", and select "Native then builtin"
![image](https://github.com/AzhamProdLive/Dying-Light-Linux-Modding-Guide/assets/73955243/d4cacca7-7f37-4717-bb6f-5113e01caa94)

Click apply, and OK, and the "Wine Configuration" window should go away.

Now that this is done, we have to add mods to the game, for this part, I'm gonna use a folder that I named DLMods inside my Documents directory, the path is :
```
${HOME}/Documents/DLMods
```
Inside this folder, you will put all your .pak files for the mods you want to use, 
![image](https://github.com/AzhamProdLive/Dying-Light-Linux-Modding-Guide/assets/73955243/eb1204a1-0eb9-44b7-bdc6-66a242748548)

For me, there is the Data3.pak file I've extracted from the CLM Weapon Pack mod (or any other mods) you should've downloaded before starting this guide (The file is renamed Data3CLM.pak to know which mod it is)

After this, head back into your Dying Light Install directory and open a terminal inside, then type ```nano dide_mod.ini```. At the end of the file, add the path of the mod you added inside your DLMods folder (or whatever you named it)
For me it's ```/home/azhmprd/Documents/DLMods/Data3CLM.pak```, and make sure to add ```=1``` in order for the mod to get loaded at start
![image](https://github.com/AzhamProdLive/Dying-Light-Linux-Modding-Guide/assets/73955243/07c8c5aa-cb6d-4779-b9c9-8face6629afd)

Now save and exit, then start the game

**If you used the mod I used in this guide**, then head into your inventory, if you see this : 
![image](https://github.com/AzhamProdLive/Dying-Light-Linux-Modding-Guide/assets/73955243/0df6749d-d0fd-4242-9776-1a2b94b60287)

Then congrats ! You managed to complete this guide !

If it doesn't, then you did something wrong, or you have a cracked version of the game, you little pirate *Yarr Arr Arr*

---

### If you need help for it, make sure to make an [issue](https://github.com/AzhamProdLive/Dying-Light-Linux-Modding-Guide/issues) on this Github page, where i'll make sure to help you around 4 at 6 working years :)

--- 

# Special thanks : 
- Buck2202 for the great guide on MSC that inspired a lot this guide.
- nloginov for the great full archive of mods and cheats (Don't cheat in PvP thought, cheating bad :( )
