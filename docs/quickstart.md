#Quick start
## 1. Required items
In order to have a working setup you will need:

1. The SuM device - included in your order.
2. A 12V power supply or battery - NOT included in your order.
3. A LAN cable (optional, but recommended) - NOT included in your order.
4. An hydrophone (essential if you plan underwater use) - NOT included in your order.
    <br>If you need one, you may consider SubSeaPulse transducers. Contact SubSeaPulse for further information.
    

Moreover, if you intend to use the modem's analog audio transceiver feature (you can skip this point if you use the modem for regular digital data transmissions) you will also need:

1. A USB soundcard with a line-in/mic interface
2. A button, to be employed as a Push-To-Talk button.
	<br>**If these components were not included in your order, additional set-up steps are required: please follow the [SuM HW upgrade guide](https://github.com/subseapulse/sum-doc/raw/master/sum-analog-upgrade-guide.pdf).**
<br>**SuM devices shipped before 2025-04-15 need to be upgraded in order to support this feature: for furter guidance, contact SubSeaPulse.**

Further information about the power supply requirements (connector size, power rating) are included in the shipped, physical documentation that must be read before use. If not available, equivalent information can be found in the [SuM Hardware Manual](https://github.com/subseapulse/sum-doc/raw/master/SuM_HW_manual.pdf). __Powering the system through the Raspberry Pi's USB interface is NOT supported, as it may cause the system to fail due to insufficient current draw.__

## 2. Power up
In order to carry out inital configuration, it is preferred to first turn on the modem connecting the power supply only, without peripherals. A green LED on the base board will signal power while a red, blinking LED signals I/O operativity.
After some minutes, the SuM's bootstrap will be complete: you may proceed with the initial configuration. 

## 3 Initial configuration
Initial configuration mainly involves setting up the appropriate interface the user intends to communicate with the modem through. All methods require logging into the modem using the default user account credentials, reported below.

	username:	sum
	password:	sum

After logging in, you may configure the interface of your choice. We recommend using `sudo raspi-config` for editing WiFi settings and `sudo nmtui` for editing ethernet settings.
### 3.1 Initial access via Ethernet (recommended)
The modem will be shipped with a static Ethernet IP address, fixed at `192.168.3.100`. Connect it to a LAN using an Ethernet cable then connect via SSH using the default user credentials.
### 3.2 Initial access via screen and keyboard
Connect a screen (using the HDMI port) and an USB keyboard to the modem. If nothing is shown on the screen, reboot the modem by unplugging and plugging back the power supply. Log in using default credentials.
### 3.3 Inital access via WiFi
By default the modem will attempt connection to the `sum_wlan` WiFi network, providing `sum_default` as password. Once a WiFi connection with the same parameters is created, the modem will connect to it (it may take a couple of minutes). The modem will acquire an IP address via DHCP: connect via SSH using the default credentials.

At this point, your device should be reachable through the desired interface.
Now refer to the [Usage](usage.md) section for normal operation.
