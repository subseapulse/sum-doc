#Troubleshooting
Below are reported some common problems and some diagnostic techniques that may help you get your modems back up and running. This page will be updated as relevant usage difficulties arise among users.

##Common problems
* **The modems are correctly set up and powered on. Transmission happens (green lights turns red on the SuM board) but no data is received on the other device.**

A common cause for this problem is misconfiguration of the `id` parameter inside the modem's `startmodem.sh` script. The value assigned to `id` is included in all sent messages: the default behavior for the modems is to reject incoming messages showing an `id` equal to their own. This behavior is needed to avoid message loopback.
Consequently, all modems must have a distinct `id` value for correct operation.

Another common cause that may hinder communication in underwater test setups is echo: the problem is particularly relevant when the hydrophones are placed close to each other and/or confined in a small water volume. Try changing the `gain` value inside `startmodem.sh` trying also lower values down to 0.01.


##Troubleshooting utilities
The modem's software is designed to log a number of operations that happen both during modulation and demodulation. That information is saved inside the system journal: a real-time view of the incoming journal messages is given by `journalctl -f`. Make sure to have `startmodem.sh` also running, either in background or in another shell.
