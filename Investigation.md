# FIESTA
## Can be started via both:
- SetupScene
- ConnectingScene

## Scene flow (Two valid ways)
1) SetupScene -> ConnectingScene -> MainScene
2) ConnectingScene -> MainScene

## Works with:
- Index = Yes
- Quest 2 = No (Headset works, but not controllers)
- VRTK's VR Simulator = No

## Chart
- "Acts as a wrapper for IATK's visualisation script"
- Single class, 2033 lines long!
- TODO: Find out if I should use Chart or just go direct to IATK visualisation script?

## Chart Manager
- Connects IATK data sources and visualisations to Proton multiplayer
- Obj containing "Chart Manager" (FIESTA) and "CSV Data Source" (IATK)
- Had CreatePanel

## CreatePanel
- 



# IATK
Used two main types of prefabs:

**CSV Data Source**

    Used to set the data to be used by one or more visualisation


**Visualisation**

    A single visualisation, can change settings to create different visualisation types


  