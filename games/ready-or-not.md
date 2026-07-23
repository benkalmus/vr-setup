# VR Mod 

https://www.nexusmods.com/readyornot/mods/6914?tab=description


Once you've downloaded the mod from Nexus, place the PAK file in the following directory;
\SteamLibrary\steamapps\common\Ready Or Not\ReadyOrNot\Content\Paks

This can easily be found by right clicking on Ready Or Not in your Steam library. Click on manage and browse local files. Then navigate through folders ReadyOrNot, Content and Paks.

Place the PAK file in the Paks folder.


Step 2 - Launch Options

Right click on Ready Or Not in your Steam Library -> Properties -> General. Set "selected launch Option to : "DirectX 11".
Then in the field below add the following to the launch Options (copy-paste them): 
-usehmd -VRTweaks -VRMappings

If you want the game to automatically boot into VR 3 seconds after a mission is loaded, you can add -autoVR to the launch options so you wont have to use the U key.


Step 3 - Adjust In Game Settings

Launch the game in flat mode. Change your in game graphics preset to match your PC performance. The game is very performance intensive in VR.

Important - Navigate to the Controller menu and turn aim assist intensity off!

Click Apply Settings

We need more user data with various PC configurations to be able to offer recommended specs and settings so please provide any feedback on good settings in our Discord.

Now you’re ready!


Step 4 - Launch VR Software,  Launch the Game.
(Order MATTERS)

Launch Meta Link / Steam VR (Depending on your headset). You should see the Meta or Steam holding area in your headset.

Launch Ready Or Not from Steam in flat mode. (You won't see anything in VR just yet. That is intentional)

Select your loadout and mission in flat mode.

Once the mission has loaded, press the ‘U’ key on your keyboard to move into VR mode and put your headset on. The game window needs to be selected for the game to detect your controllers.

If you complete or fail a mission, you will automatically be moved back to flat mode. If you need to exit VR manually, you can press the 'home' key on your keyboard. After using the 'home' key you have to restart the mission.

We find this method is useful as it means you can play the game in both VR and flat mode on the fly without having to remove or tweak files.


DLSS (Highly Recommended)

If you have an nVidia graphics card, we highly recommend using the latest version of DLSS for the best performance and graphical fidelity. Download DLSS swapper here; https://github.com/beeradmoore/dlss-swapper/releases 

Select v310.4 or newer for DLSS and DLSS Frame Generation. For preset select J.

ostty 1.3.1-arch2.   /home/benkalmus/projects/vr-setup
Important - Enable DLSS in game settings by switching it to Balanced.


Performance Tweaks

There’s a few things you can do to improve performance;
Lower in game graphical preset to Medium / High
Keep the Rendering Resolution to 1.0x in the Meta Quest Link PC Software
Lower the headset refresh rate to 72hz in the Meta Quest Link PC Software
Use DLSS if you’re using an nvidia graphics card


Loadout

Main weapon is over the right shoulder
Long Tactical (Optiwand, Ram, Shield, GLauncher) is over the left shoulder
Pistol is on right hip/thigh
Tablet is on left hip. Contains objectives and VR settings.
Taser is under left arm
Taser cartridge is under right arm
Accessory pouch is on your lower back. Press alt fire mode to switch between lock pick and wire cutters when holding a mutlitool. Wire cutters allow disarming door-wired traps manually by snipping the wire. Putting a lockpick into a lock starts lockpicking sequence.


Hand Gestures For Squad Commands

We’ve implemented some hand gestures to squad commands (you need to move your hand quite quickly for them to be detected;

Left hand make a fist and hold = hold position
Left hand up with index finger pointed making a circle = On me
Left hand down with index finger pointed making a circle = search the area
Left hand up flat making a chopping motion = default command


Known Issues & Solutions

1P - Black screen, Missing textures
1S - Set game to DX11. Try removing -AAMode1 from params. Restart PC (yes, seriously). Try adding -d3d11 param

2P - Game crashes upon launch
2S - Conflicting with other mods, try removing some. Check safety notifications and add an exclusion to Windows defender for the Ready Or not folder. Confirmed, for some people it is silently blocking it. Try removing -AAMode1 from params. OpenXR-Companion app may cause a crash, disable it if you have it. If you used UEVR before it replaced some files so a full game reinstall is required (important to delete the game folder after uninstalling the game!)

3P -Cant download it from Mod.io - says queued/downloading, not getting applied.
3S - Mod.io shat itself apparently. For some it works, for some it doesnt, out of our control. Use Nexusmods link

4P - I see my hands but buttons/grabbing/walking does not work.
4S - Probably the game is out of focus. Happens sometimes. Once spawned - click with mouse so the game window is in focus. Or you missed the -VRMappings param.

5P - It won't shoot where I aim the gun
5S - Disable aim assist and restart the game/pc. If it is already off (or if it didn't help) turn it on and off again.

6P - Performance issues. Camera jitter
6S - Disable discord overlay and any other overlays. Drop settings. Disable OpenXR toolkit if you have it on. Install DLSS with a DLSS Swapper like it was described in the instructions. Try without DLSS if it causes problems. Try adding -AAMode0 this will drop visuals but should increase performance. Make sure your default OpenXR runtime is set correctly.

7P - Spinning like crazy.
7S - You pressed U multiple times. Dont do that.

8P - I cant use left hand as dominant. Kinda works but jank.
8S - Comes with a future update. Just not there yet.

9P - When launch the mission and press U - my view changes on flat display but nothing in my headset. Not detecting headset.
1) Check your launch params. Could've added an extra space between dash and the word. Better copypaste it, not type it:
-usehmd -VRTweaks -VRMappings
2) Order matters. Enable SteamVR/ Meta Link BEFORE you launch the game so it connects to the VR software properly. Then start the game from STEAM LIBRARY, not the desktop shortcut, not the in-VR dashboard, not the DLSS-swapper menu.3) If you used UEVR before - it replaced VR related files. Do a full game reinstall. (important to delete the game folder after uninstalling the game!)
4) Make sure your default OpenXR runtime is set correctly - if using Meta link - set Meta link as default openXR runtime in Meta App settings. Same for steamVR
5) Steam sometimes does not pass launch parameters to the executable. If that's the case you need to make a shortcut to the game executable (steamapps\common\ReadyOrNot\ReadyOrNot.exe) and in TARGET put the launch parameters like this:
... Or Not\ReadyOrNot.exe" -usehmd -VRMappings -VRTweaks -d3d11
Then launch the game using that shortcut.

10P - When using Virtual Desktop there is a black bar between the eyes
10S - There is currently no good solution for that. We reaching out to the VD dev to see what can be done. For now make sure Horizontal and Vertical FOV in advanced streamer settings (the app you use before launching into VR) is set to between 95 and 100, not lower

11P - I am in VR but there are no hands!
11S - Met frequently in Virtual desktop setups. Try clicking Menu/Meta button a couple times. Try suspending and activating your headset. (Clicking power button on the headset or taking it off your head) Basically anything that would kick the system to update controller info. And make sure your default OpenXR runtime is set correctly

12P - In Multiplayer with other VR clients going VR causes a crash/checksum error
12S - Different runtimes cause a mismatch. Primarily VD + Meta in one world cause this. Use same runtime.


Other Known Issues

    Gun and hands disappear when cuffing suspect / civilian
    Gun and hands disappear when healing
    False positive detection for melee, sometimes you hit people while just passing them
    We dont talk multiplayer. Just accept it as it is for now, how it looks and works.
    A little jitter or stiffness when moving IRL laterally
    You can fall through the map in some places. Rarely happens but still happens.
    Hands (and guns) can go through walls and doors. Physical interaction really complicates things. I have a sketch for a system that would prevent cheesing it but so far if you play by the rules of common sense - it works. If you ask me - it's better than walking through the doorframe too close, hitting it with a gun and making it CLUNKCLUNK and spazz out or stopping you.
    Starting loading 6-shot launcher from the bottom round (leftmost if chamber is open) may block other shots from being inserted. Should be a one-off issue when doing it the first time on the level. Cycle the chamber (open-close-open) to reset it.

    Doorwedge, C2 are not implemented yet.
    Laser pointer moves away from the gun when aiming down sights with magnified scopes.
    In some rare cases left arm gets stuck in the air.
    Some places like stairs or steps you cant walk up or down. Temporary workaround - crouch-uncrouch
