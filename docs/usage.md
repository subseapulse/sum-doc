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
|Modulation technique			|modulation				|flex,janus,flexofdm				|
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

##Communication
Once the `startmodem.sh` script is up and running, the modem opens a TCP socket on port `55555` to communicate with the user.
You may use *netcat* to connect to the socket: open another SSH terminal and connect to the modem (as in [quickstart](quickstart.md)) then type

	nc localhost 55555
	
Afterwards data can be sent and received through the terminal.

##Toggling autostart
If you wish the modem to start listening and transmitting to the socket port 55555 automatically after startup you may enable the autostart service by typing

	sudo systemctl enable sumd.service
	
In order to disable the autostart feature instead type

	sudo systemctl disable sumd.service


	

