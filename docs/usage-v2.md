#Usage and settings guide (v2.x)
**__For older versions (v1.x, shipped before 04-2026) see the [dedicated Usage page](usage-v1.md).__**

Once the [set-up](quickstart.md) is complete you may start communicating with the modem. Ensure you have already connected the transducer then start the modem by executing the `sum_modem` file.
The full path to run the main file from the user directory will be

	/home/sum/sum/sum_modem
	
This command will run the `sum_modem` process with the last valid used settings.
Upon startup, the modem will open another TCP socket at port 33333 to receive ASCII-encoded configuration messages. This is the recommended and the only ufficially supported method for changing (and reading) the modem's working settings. You may use *netcat* to connect to the socket, typing

	nc <SuM IP address> 33333
	
Alternatively, the socket can be opened locally: open another SSH terminal and connect to the modem (as in [quickstart](quickstart.md)) then type

	nc localhost 33333
	
Afterwards, configuration messages/commands (and their return data) can be sent/received through the terminal.

When new settings are sent, the modem may behave in two different ways depening on which settings was sent:

- Most settings require the modem’s software to restart (due to significant software-level change).
Therefore sending configuration messages affecting these parameters does not produce an immediate effect on the modem’s running configuration. These parameters instead form a “candidate” configuration which is validated and applied only after the modem receives an “apply” command from the user, which restarts the modem’s software with the new settings. The software restart will disable acoustic communication and IO socket (55555) functionality for a couple of seconds.
- Some settings, below referred as *live*, can be changed during the modem’s execution: as soon as a valid “set” configuration message is received, the parameter is changed with immediate effect. This is limited to a small settings subset due to technical limitations.

##Configuration messages syntax

Configuration messages will generally follow the syntax below. Messages are LF-terminated, case-insensitive and are composed by space-separated tokens.

    <operation> [<key>] [<value>] LF

where:

- *Operation* indicates what action the modem must perform/has performed.
- *Key*, where applicable, identifies a parameter.
- *Value*, where applicable, is used in configuration messages to specifify a new parameter value.

Below are all the possible token values (hence all possible settings and actions).

###Operation tokens

|Operation token    |Key required?  |Value required?    |Description                                                |
|-------------------|---------------|-------------------|-----------------------------------------------------------|
|get                |yes            |no                 |Gets value of parameter (currently in use)                 |
|set                |yes            |yes                |Sets value of candidate/live parameter.                    |
|apply              |no             |no                 |Evaluates (and apply) candidate configuration.             |
|clearconf          |no             |no                 |Resets the candidate configuration (to running)            |
|reset              |no             |no                 |Restarts the modem software (discarding candidate config)  |
|help               |Optional       |no                 |If no key is provided, a help message is printed listing available operation and key tokens. If a key is provided, key-specific help is printed. |

###Key tokens
|Key token          |Value type/range                       |Description                    |Mod-specific?  |Live?  |
|-------------------|---------------------------------------|-------------------------------|---------------|-------|
|carrier            |int[1;95000]                           |Carrier frequency [Hz]         |               |       |
|modulation         |See "value tokens" table #1 below      |Modulation scheme family       |               |       |
|id                 |int[1;15]                              |Sender ID value                |               |       |
|gain               |float[0;1]                             |Transmission gain %            |               |yes    |
|samplfreq          |{48000,96000,192000}                   |Signal sampling frequency [Hz] |               |       |
|premodulation      |bool                                   |Toggle premodulation features  |               |       |
|flex_mac           |See "value tokens" table #2 below      |MAC protocol                   |Flexframe      |       |
|flex_modscheme     |See [Overview](overview.md) table #2   |Specific modulation scheme     |Flexframe      |       |
|flexfec_inner      |See [Overview](overview.md) table #3   |Inner FEC scheme               |Flexframe      |       |
|flexfec_outer      |See [Overview](overview.md) table #3   |Outer FEC scheme               |Flexframe      |       |
|interpflex         |int[2;80000]                           |Sample interpolation value     |Flexframe      |       |
|janus_bw           |int > 0                                |Bandwidth [Hz]                 |JANUS          |       |
|janus_hd           |bool                                   |Toggle half-duplex HW mode     |JANUS          |       |
|janus_mac_en       |bool                                   |Toggle additional MAC header   |JANUS          |       |
|vad_level          |{-1,0,1,2,3}                           |VAD toggle and filter level    |Analog         |       |

###Value tokens (enum)
|modulation - value |Description                                                        |
|-------------------|-------------------------------------------------------------------|
|flex               |Flexframe modulation scheme family                                 |
|janus              |JANUS modulations                                                  |
|ofdm               |OFDM configuration for Flexframe modscheme family. **WIP!!!**      |
|lsb                |Analog lower side band (LSB) modulation for voice communication    |
|usb                |Analog upper side band (USB) modulation for voice communication    |
|dsb                |Analog double side band (DSB) modulation for voice communication   |

|flex_mac - value   |Description                                                        |
|-------------------|-------------------------------------------------------------------|
|csma               |Carrier-Sensing Multiple Access (CSMA) MAC policy                  |
|dummy              |Simple channel access, no flow control MAC policy                  |

###Modem-to-Client messages
The modem will transmit data over the configuration socket (33333) only when replying to incoming configuration/command messages. Modem-to-client messages have a similar syntax to client-to-modem messages (presented above) but make use of different operation tokens:

- **ACK** to indicate successful operation, echoing the accepted operation/params (if any)
- **SEND** to send information to the client (upon request)
- **DENY** to indicate operation rejection/failure, including a synthetic error code.

The table below synthetizes the possible response messages:

| Operation       | Outcome                        | Response                       |
|-----------------|--------------------------------|--------------------------------|
| get             | Successful operation           | send <key\> <value\>           |
| get             | Error (invalid key provided)   | deny invalid_key               |
| set             | Successful operation           | ack <key\> <value\>            |
| set             | Error (invalid key provided)   | deny invalid_key               |
| set             | Error (invalid value provided) | deny invalid_value             |
| apply           | Successful operation           | ack apply                      |
| apply           | Error (invalid configuration)  | deny invalid_config            |
| clearconf       | Success (no error possible)    | ack clearconf                  |
| reset           | Success (no error possible)    | ack reset                      |
| help            | Success (no error possible)    | **Human-readable text output** |
| Any other input | Error (unknown operation)      | deny invalid_op                |

<br>

##Communication
###Data communication
Once the modem software is up and running, the modem opens a TCP socket on port `55555` to communicate with the user.
You may use *netcat* to connect to the socket, typing

	nc <SuM IP address> 55555
	
Alternatively, the socket can be opened locally: open another SSH terminal and connect to the modem (as in [quickstart](quickstart.md)) then type

	nc localhost 55555
	
Afterwards data can be sent and received through the terminal.

Having problems transmitting/receiving data? Take a look at the [troubleshooting](help.md) section.

###Audio communication (modulation=analog)
**Analog audio trasmission is not natively available on some old SuM SW versions and requires specific hardware, as listed in [quickstart](quickstart.md)**

Once the modem software is up and running, the SuM immediately starts receiving and demodulating the analog audio signal, sending it to the USB soundcard's line-out interface. In order to transmit, push the Push-To-Talk button and release it once transmission is over (as in standard intercom behaviour): the modem will transmit audio provided through the USB soundcard's line-in.

##Toggling autostart
If you wish the modem to begin operation automatically after startup you may enable the autostart service by typing

	sudo systemctl enable sumd.service
	
In order to disable the autostart feature instead type

	sudo systemctl disable sumd.service

On startup the modem will load the last valid used configuration.

