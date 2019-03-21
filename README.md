# RaspberryPiNetworking
This is a repo of procedures for setting up 'bramble' of RaspberryPi's.

The goal is to ultimately get Kubernetes running on the cluster for home server support.

Jumping ahead a view steps...

After flashing a fresh image to the SD card.

 - Boot up the Pi.
   - use `sudo raspi-config` to configure the Pi
     - enable SSH
     - Rename the Pi
     - Localize
     - etc...
   - Finish and reboot
   - run `sudo apt-get update\
          sudo apt-get upgrade`
   - enable wifi
     - open wpa_supplicant.conf
     - `sudo nano /etc/wpa_supplicant/wpa_supplicant.conf`
     - add `network={
                ssid="replaceWithNetworkId"
                psk="replaceWithWifiPass"
            }`
     - restart networking service:
       `sudo systemctl restart networking.service`y
`
