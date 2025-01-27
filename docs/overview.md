#Overview
## Introduction
The Subsea Modem (SuM) is a project by [SubSeaPulse Srl](https://www.subseapulse.com/products/#sum). It is a low cost, made in Italy, software-defined acoustic modem specifically tailored for underwater telecommunications.
The device provides a series of robust modulations and coding schemes to optimise usage for any application, both research and industrial. Nonetheless it is suitable for low-power applications, allowing for battery-powered deployments.

The SuM is composed of a Raspberry Pi board, an HiFi sound card and the SuM-HAT, the first Raspberry Pi HAT engineered to transform the Raspberry into an analog front-end to pilot underwater acoustic transducers with great performance.

## Operation principles
The SuM is a complete device, ready to be coupled with a transducer to stard communicating in the underwater realm. The modem's software is designed to send and receive data through a TCP socket, so that it can be easily interfaced with any user-provided application.

## Communicating
Upon receiving data from the user the modem will encode it into a modulated audio signal. This signal will be generated and amplified by the SuM's hardware in order to be then fed to the connected transducer. When not trasmitting, the modem will be listening for other signals captured by the transducer, ready to decode them and deliver their message to the user.

The device is built as a Software-Defined Modem (SDM), meaning that much freedom is left to the user in setting parameters and choosing modulation and coding schemes. In particular, the SuM supports out-of-the-box the modulation techniques below.

|Modulation technique					|Type										|
|---------------------------------------|-------------------------------------------|
|[Flexframe](https://liquidsdr.org/)	|Single-carrier/OFDM, various modulations	|
|[JANUS](https://www.januswiki.com/)	|Frequency-hopping, BFSK modulation			|

Choosing single-carrier Flexframe allows the user to better specify what modulation and forward error correction scheme to use: below are listed the supported options.

|Type								|Available parameters													|
|-----------------------------------|-------------------------------------------------------------------|
|Phase shift keying					|bpsk, qpsk, psk2, psk4, psk8, psk16, psk32, psk64, psk128, psk256	|
|Differential phase shift keying	|dpsk2, dpsk4, dpsk8, dpsk16, dpsk32, dpsk64, dpsk128, dpsk256		|
|Amplitude shift keying				|ask2, ask4, ask8, ask16, ask32, ask64, ask128, ask256				|
|Quadrature amplitude				|qam4, qam8, qam16, qam32, qam64, qam128, qam256					|
|Amplitude and phase shift keying	|apsk4, apsk8, apsk16, apsk32, apsk64, apsk128, apsk256				|
|On-off keying						|ook																|
	
[full list](https://github.com/jgaeddert/liquid-dsp/blob/master/src/modem/src/modem_utilities.c#L34)

|Type							|Parameter	|
|-------------------------------|-----------|
|Hamming (7,4)					|h74		|
|Hamming (7,4) plus parity bit	|h84		|
|Hamming (12,8)					|h128		|
|Repeat x3						|rep3		|
|Repeat x5						|rep5		|
|SEC-DED (72,64)				|secded		|
|Convolutional K=7, df=10		|cv27		|
|Convolutional K=9, df=12		|cv29		|
|Convolutional K=9, df=18		|cv39		|
|Convolutional K=15, df<=57		|cv615		|
|Reed-Solomon m=8				|rs			|

