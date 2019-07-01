# FIESTA
*Free-range Immersive Environment to Support Team-based Analysis*

FIESTA is a multiplayer Unity program that allows users to collaboratively visually analyse data in a shared virtual/augmented reality environment. It can support an arbitrary number of users and works in both co-located and remote settings.

## Requirements
To properly run FIESTA, you will need to have installed:
- Unity 2018.3.6f1
- Oculus software
- ZED SDK (for AR passthrough)
- Photon Server (for self-hosted server)

This repository currently contains most necessary 3rd party plugins and files to run. These will most likely be removed later on:
- IATK
- VRTK v3.3.0
- SteamVR v1.2.3
- Oculus Integration
- PUN v2
- DOTween
- ZED Plugin for Unity (for AR passthrough)

## How to run
### Setting up networking
As FIESTA is a multiplayer program, clients must connect to a server. We use the [Photon Unity Networking engine](https://www.photonengine.com/en-US/Photon) for this. 
You can either use the cloud servers hosted by Exit Games themselves (free up to 20 CCUs) or host your own one locally. The former requires less setup time, but the latter is less prone to latency and bandwidth restrictions and therefore is recommended.

In either case, you will need an AppId (API key) to use Photon. You get one by signing up on their [website](https://www.photonengine.com/en-US/Photon), and creating a new Photon PUN or RealTime app. You can then enter this key into Unity via Window > Photon Unity Networking > PUN Wizard and choosing Setup Project.

If you want to host your own local server, refer to [this guide](https://doc.photonengine.com/en-us/server/current/getting-started/photon-server-in-5min) for instructions. You will need to enter the IP address of the server in the inspector via Window > Photon Unity Networking > Highlight Server Settings. Make sure "Use Name Server" is unchecked.

If you want to use a cloud server, check "Use Name Server" instead.

### Launching the game
FIESTA swaps between scenes in order to properly load players and their required objects. The order in which these scenes load are:
1. StartupScene
	* This assists with specifying the correct IP address to connect to, particularly for when FIESTA is being launched from a standalone build and not from the Editor
	* Clicking "Connect" will connect to the specified IP address. If left blank, it will default to localhost.
	* "Connect to Cloud" will connect to a cloud server, irrespective of what was entered in the IP address field
	* Checking "Connect as spectator (non VR)" will influence which scene is launched when connected
2. *(NonVR_)* ConnectingScene
	* This acts as the "lobby" in which players connect to before loading into the actual scene
	* The player's name and colour of their avatar can be customised with the keyboard and mouse
	* The default scene will enable an avatar controlled in VR, the NonVR scene will enable an avatar controlled using the keyboard and mouse
	* The game can then be started either by clicking on the "Start" button on-screen, or by grabbing onto the floating "Start" button
3. MainScene
	* This is where the actual data visualisation functionality of FIESTA exists

In short, launch StartupScene if running from a standalone build, launch ConnectingScene is running from the Editor (with IP addresses properly setup in Photon's server settings).

### Notes on running
There are some things to bear in mind about how FIESTA functions:
* It does not support users joining and leaving mid-game, therefore ensure that all users are connected before starting
* With enough users and/or a higher defined server tickrate, PUN may force disconnect a user if network traffic is too high. The easy solution is to use a private locally hosted server instead

## Setting up co-located environment
FIESTA can be used in both co-located and remote collaborative contexts. While remote collaboration is easier to setup, co-located collaboration requires that the VR tracking system is properly synchronised across all users to prevent accidental collisions.

There are different steps that need to be done depending on the setup you are using. This section assumes you are using VR backpacks with HTC Vives for tetherless VR.

##### VR backpacks
You will need to establish a remote desktop connection to each of the VR backpacks to properly start/stop/update FIESTA. You can use any software you prefer, such as TeamViewer, AnyDesk, or Window's Remote Desktop Connection.

You may notice certain programs, particularly those which rely on the GPU such as Unity, do not properly show up in the remote viewer. In this circumstance, you will need to connect a monitor to the backpack for Windows to begin running the GPU. To circumvent this, a HDMI/DisplayPort dummy plug (display emulator) can be connected to trick Windows into thinking there is a monitor plugged in, resolving the issue.

To minimise latency for both the remote desktop and FIESTA, it is highly recommended to connect all backpacks onto the same private network.

##### SteamVR room sync
It is preferable to use VR headsets using outside-in tracking as it inherently provides an accurate external frame of reference to any headset in the environment. The more tracking basestations that are being used the better, as it minimises accidental occlusion to the basestations due to multiple users. This essentially limits your options to using SteamVR 2.0 basestations with either the HTC Vive Pro or the Valve Index.

To synchronise positions of players and objects in FIESTA, we need to use the same room setup and configuration across all PCs. Begin by performing the SteamVR room setup process on one of the PCs. If any other PC hasn't run the first time setup yet, it is recommended that you do so as well. Then, copy and paste the following from the PC you ran the room setup with to all other PCs:

1. Navigate to \<steam install directory>\\config
2. Copy over chaperone_info.vrchap
3. Navigate to \<steam install directory>\config\lighthouse
4. Copy over lighthousedb.json