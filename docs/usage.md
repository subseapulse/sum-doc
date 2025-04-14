#Usage
Once the [set-up](quickstart.md) is complete you may start communicating with the modem. Ensure you have already connected the transducer then start the modem by executing the `startmodem.sh` script.
The full path to run the main file from the user directory will be

	/home/modem/moda/dev-toolbox/scripts4trials/startmodem.sh
	
This command will run the `test-modem` process with the default settings.
Most parameters can be conveniently changed by editing the `startmodem.sh` file. The table below lists them.

##Modulation-independent parameters

|Parameter						|`startmodem.sh` label	|Possible values					|
|-------------------------------|-----------------------|-----------------------------------|
|ID (source address)			|id						|`int` [1;15]						|
|Carrier frequency				|carrier				|`int` (Hz)							|
|Modulation technique			|modulation				|flex,janus,flexofdm,analog				|
|Output amplifier gain			|gain					|`float` [0.01;0.99]				|

<br>

##Flexframe-specific parameters
**(only evaluated when modulation=flex or modulation=ofdmflex)**

|Parameter						|`startmodem.sh` label	|Possible values					|
|-------------------------------|-----------------------|-----------------------------------|
|Modulation						|flex_psk				|Listed in [Overview](overview.md)	|
|Interpolation					|interpflex				|`int` [5;20]						|
|Inner FEC scheme				|intFEC					|Listed in [Overview](overview.md)	|
|Outer FEC scheme				|outFEC					|Listed in [Overview](overview.md)	|

<br>

##JANUS-specific parameters
**(only evaluated when modulation=janus)**

|Parameter						|`startmodem.sh` label	|Possible values					|
|-------------------------------|-----------------------|-----------------------------------|
|Bandwidth						|bandwidth				|`int` (Hz)							|

<br>

##Analog audio trasmission-specific parameters
**(only evaluated when modulation=analog)**

|Parameter						|`startmodem.sh` label	|Possible values					|
|-------------------------------|-----------------------|-----------------------------------|
|Sampling frequency						|samplingfreq				|`int` (Hz)							|
|Analog modulation mode					|analog_mod				|usb, lsb, dsb							|

<br>

##Communication
###Data communication
Once the `startmodem.sh` script is up and running, the modem opens a TCP socket on port `55555` to communicate with the user.
You may use *netcat* to connect to the socket, typing

	nc <SuM IP address> 55555
	
Alternatively, the socket can be opened locally: open another SSH terminal and connect to the modem (as in [quickstart](quickstart.md)) then type

	nc localhost 55555
	
Afterwards data can be sent and received through the terminal.

Having problems transmitting/receiving data? Take a look at the [troubleshooting](help.md) section.

###Audio communication (modulation=analog)
**Analog audio trasmission is not natively available on old SuM versions and requires specific hardware, as listed in [quickstart](quickstart.md)**

Once the `startmodem.sh` script is up and running, the modem immediately starts receiving and demodulating the analog audio signal, sending it to the USB soundcard's line-out interface. In order to transmit, push the Push-To-Talk button and release it once transmission is over (as in standard intercom behaviour): the modem will transmit audio provided through the USB soundcard's line-in.

##Toggling autostart
If you wish the modem to begin operation automatically after startup you may enable the autostart service by typing

	sudo systemctl enable sumd.service
	
In order to disable the autostart feature instead type

	sudo systemctl disable sumd.service
	


	

