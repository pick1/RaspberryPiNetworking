# RaspberryPiNetworking
This is a repo of procedures for setting up 'bramble' of RaspberryPi's.

The goal is to ultimately get Kubernetes running on the cluster for home server support.

Jumping ahead a view steps...

After flashing a fresh image to the SD card.

 - Boot up the Pi.
 - Run `sudo raspi-config` to configure the Pi
   - enable SSH
   - Rename the Pi
   - Localize
   - etc...
 - Finish and reboot
 - Perform updates
   - `sudo apt-get update`
   - `sudo apt-get upgrade`
 - Enable wifi
   - open wpa_supplicant.conf:
   - `sudo nano /etc/wpa_supplicant/wpa_supplicant.conf`
   - add `network={`
             `ssid="replaceWithNetworkId"`
             `psk="replaceWithWifiPass"`
         ` }`
   - restart networking service:
     `sudo systemctl restart networking.service`

 - Repeat above steps 'n' times for Pi's in bunch.

 - Networking the Pi's together. This example uses wifi to connect the Pis. As the goal is to create cluster it will be important to have static ip addresss for the Pis. Following steps are to setup static ip.

   - Set static IP addresses
   - Run `ifconig`
   - Open /etc/dhcpcd.conf
   - At the end of the file insert:
     `interface wlan0
     `static ip_address=192.168.1.numberGoesHere/24`
     `static routers=192.168.0.1`
     `static domain_name_servers=192.168.0.1`
   - Run `sudo reboot`
