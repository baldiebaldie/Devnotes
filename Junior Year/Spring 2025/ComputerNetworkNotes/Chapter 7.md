Computer networks
04-10-2025

(Link layer through wireless connections) 
#### Wireless networks
Several things that make wireless networks different than wired networks:
- Decreased signal strength: Radio signal attenuates as it propagates through matter.
- Interferences from other sources: Wireless frequencies can be shared between many devices. 
- Radio signals can also bounce off of other materials weakening or completely eliminating the original signal
There is a property of wireless signals called SNR.
>SNR (signal to noise ratio) : is the ration of signal to noise.
- The larger the SNR - the easier to extract the given signal from noise
There are different types of Wireless network taxonomy
- Single hop: With infrastructure, host connects to base station (wifi or cellular) which then connects to larger network. Without infrastructure, it has no base station of its own, no connection to the larger internet (bluetooth). 
- Multiple hops: With infrastructure hosts may relay through several wireless nodes to connect to larger internet (mesh net). Without infrastructure there is again, no base station, no connection to larger internet. May have to relay over a given wireless node (MANET, VANET)  
#### How WIFI works
Spectrum of signals are divided into channels at different frequencies.  (AP stands for *Access Point*)
- The AP admin chooses the frequency for AP
- channels mights be the same other APs which interfere with one another.
When a new host arrives it MUST associate with an AP.
- It scans channels and listens for beacon frames containing the AP's name and MAC address.
- It then picks an AP
- then performs authentication
#### Bluetooth
- Less than 10m diameter range
- a common replacement for cables (mouse, keyboards, headphones)
- has no infrastructure


