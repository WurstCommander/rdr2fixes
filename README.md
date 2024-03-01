**Red Dead Redemption 2 - Startup fix**

I suddenly had the error that my Red Dead Redemption 2 install didn't work anymore. Right after clicking "PLAY" on the launcher, the rdr2.exe is open for a second and then closes right away, no error message shown, and none in the error log files (very helpful, thanks Rockstar).

I tried deleting settings or profile, uninstalling the launcher, removing all files, moving the installation, and even a complete reinstallation.

The error is that the VULKAN API doesn't work anymore, and if you delete the profile/reinstall, it defaults to the Vulkan API.

I tried nearly everything, I could not get RDR2 to start with Vulkan again, after a certain NVidia Driver version it didn't work anymore. Several solutions, several hours of rebooting, reinstalling. 
If you are intestred in the finding of the solution(s), here are some reddit threads of mine:

[Reddit Red Dead Redemption 2 won't start Fix, deleting Files and reinstalling Vulkan](https://old.reddit.com/r/PCRedDead/comments/ykenv9/psa_if_red_dead_redemption_2_crashes_instantly/ixv8dv4/)

[Reddit Red Dead Redemption 2 won't start #2](https://old.reddit.com/r/PCRedDead/comments/ykenv9/psa_if_red_dead_redemption_2_crashes_instantly/)

This fix worked, but isn't really the right solution, after getting tips from /u/diceman2037 the cause of the problem are old / deprecated Vulkan Layers.

----

** In the end, you have to remove old Vulkan Layers with regedit.exe**

For me, it was an old Twitch Layer which I installed years ago and the uninstallation routine of Twitch didn't remove it.

Even if you believe you don't have such layers, check them out :-) It's not only Twitch, it can be any software which uses Vulkan Layers, like EOS/Epic Store Software, Bandicam, Reshade, OBSS, RTSS and so on.

**Instructions:**
--------------------
You can see your Vulkan Layers with GPU Caps Viewer ([https://www.techspot.com/downloads/4618-gpu-caps-viewer.html](https://www.techspot.com/downloads/4618-gpu-caps-viewer.html)) 

Screenshot:

![image info](./pics/gpucapsview.png)


My old Vulkan layer was:

    5/ VK_LAYER_Twitch_Overlay (spec:1.1.0, impl:1)

As you can see, it's version 1.1.0 which doesn't seem to work with new Vulkan drivers which are installed with relatively new Nvidia drivers.
That's the reason a downgrade to older versions of Vulkan / Nvidia worked. 

---

**Ok, long story short, you have to remove the old Vulkan Layers with Regedit.exe**

You can find the keys with these names/paths:

    HKEY_LOCAL_MACHINE\SOFTWARE\Khronos\Vulkan\ImplicitLayers
    HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Khronos\Vulkan\ImplicitLayers
    HKEY_CURRENT_USER\SOFTWARE\Khronos\Vulkan\ImplicitLayers

These were my Twitch Layers:

    C:\Program Files\Common Files\Twitch\Studio\Versions\0.90.7641.33738\TwitchOverlayVulkanConfig64.json
    C:\Program Files\Common Files\Twitch\Studio\Versions\0.90.7641.33738\TwitchOverlayVulkanConfig32.json

I removed them, the GPU Caps Viewer showed this:

    Instance layers: 9
     1/ VK_LAYER_NV_optimus (spec:1.3.224, impl:1)
     2/ VK_LAYER_Galaxy_Overlay (spec:1.1.73, impl:1)
     3/ VK_LAYER_Galaxy_Overlay_VERBOSE (spec:1.1.73, impl:1)
     4/ VK_LAYER_Galaxy_Overlay_DEBUG (spec:1.1.73, impl:1)
     5/ VK_LAYER_VALVE_steam_overlay (spec:1.3.207, impl:1)
     6/ VK_LAYER_VALVE_steam_fossilize (spec:1.3.207, impl:1)
     7/ VK_LAYER_EOS_Overlay (spec:1.2.136, impl:1)
     8/ VK_LAYER_EOS_Overlay (spec:1.2.136, impl:1)
     9/ VK_LAYER_ROCKSTAR_GAMES_social_club (spec:1.0.70, impl:1)

**So Twitch's Layer is gone and Red Dead Redemption 2 starts.**

If you want a deeper understanding of this solution / how I found out what the reason for the error was, read this:
[https://old.reddit.com/r/PCRedDead/comments/ykenv9/psa_if_red_dead_redemption_2_crashes_instantly/j0yml6k/](https://old.reddit.com/r/PCRedDead/comments/ykenv9/psa_if_red_dead_redemption_2_crashes_instantly/j0yml6k/)

**Alternative fix for the lazy / Quick fix using DX12**
--------------------
You can just use DX12 which had worse performance for me.
Just change this setting in this file:

```
YOUR_DOCUMENTS_FOLDER_NOT_THE_GAME_INSTALL\Rockstar Games\Red Dead Redemption 2\Settings\system.xml
```

Change

```xml
<API>kSettingAPI_Vulkan</API>
```

to

```xml
<API>kSettingAPI_DX12</API>
```

And save the file.

RDR2 will now use DX12 instead of Vulkan, so there are no Layer problems anymore and it should start.

I hope this helps, some people couldn't even launch RDR2 the first time, so there were no XML files in the first place to change the API from Vulkan to DX12.

I think you could also remove the Layers with the Vulkan SDK if you aren't comfortable using regedit.exe


