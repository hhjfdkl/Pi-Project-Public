# Raspberry Pi Server





## Hardware Used
**Machine**: Raspberry Pi 5, 4GB RAM

**Storage**: WD MyPassport
- USB-powered

**Power Supply**: CanaKit 45W

**Chasis**: CanaKit Case for Raspberry Pi 5

**Cooling**: Raspberry Pi Active Cooler

**SD Card**: MicroSD Card 32GB
- Also, a USB adapter to flash the OS onto the card

**Network cable**: Cat5/Cat6 cable

## Software
**Operating System**: Raspberry Pi OS 64-bit Lite

**Raspberry Pi Imager**: To flash OS onto the microSD card

**Web Server**: _Nginx_ : Configured for reverse-proxy between services
- See [Reverse Proxy](/reverse-proxy-setup.md) section

**Hosted Applications**: 
- [OpenMediaVault](/OpenMediaVault.md)
- [Pi-Hole](/Pi-Hole.md) 

## Steps for Setup
Hardware Assembly
1. Assemble Pi in the case
    - Overall simple, but looking up a video can help
    - Case works well out of the box, which makes it a lot easier
2. Connect cables & plug power adapter into wall
    - Make sure you directly plug the Pi into the router for more consistent connection. Relying on wifi sometimes can have consistency issues

Put image onto MicroSD
1. Plug in the MicroSD with your USB adapter
2. Use Raspberry Pi Imager
3. Select Raspberry Pi 5 for Device
4. Select Raspberry Pi OS Lite for Operating System
	- This is the headless install
	- Use the regular Raspberry Pi OS if you need the GUI installed
5. Select the MicroSD for Storage 
	- Review the device and make sure it's the correct one

Put the imaged microSD card into the pi, power it on. You should now be able to ssh into it using the username and password you set up in the Raspberry Pi Imager.



## Change Notes
7/17/25 :
- Updated Unbound to have logging for troubleshooting purposes
- Fixed an error by disabling an Unbound-related service, though the error didn't affect any actual functionality of Pi-Hole or the rest of the machine.

7/16/25 :
- First error in Pi-Hole, but turned out to be a non-issue.

7/15/25 :
- Completed Pi-Hole setup

7/14/25 :
- nginx Reverse proxy setup **completed** : See additional document added.
	* First-time use of nginx and learned a lot about setting up this basic config in this.
	- Adjusted OMV port to work better with the nginx reverse proxy. (previous was causing conflicts)
	- Pi-Hole set up and nginx configured to route to Pi-Hole properly
- Pi-Hole setup **completed**
- Broke up this README into different spaces - other .md documents for each configuration and software setup

7/13/25 : 
- Add in nginx reverse proxy to handle routing of traffic between Pi-Hole and OMV

7/12/25 : Only conducted the basic set up. I'll document any future changes or tinkering on this Pi here. 
- OMV setup **completed**
- Enabled monitoring, set up the dashboard.
- Clock was out of sync. Corrected on Pi itself and also had to adjust in OMV. (OMV adjusts its own time through NTP, not the base machine)
- Pi-Hole installation next? Will need to use a different port than OMV to avoid conflicts




