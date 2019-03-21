# RaspberryPiNetworking
This is a repo of procedures for setting up 'bramble' of RaspberryPi's.

The goal is to ultimately get Kubernetes running on the cluster for home server support.

Jumping ahead a view steps...

After flashing a fresh image to the SD card (For Kubernetes to install correctly the image should be at least Stretch (Debian ( or higher)).

 - Boot up the Pi.
 - Run `sudo raspi-config` to configure the Pi
   - Rename the Pi
   - Localize
   - enable wifi
   - expand partition
 - Perform updates
   - `sudo apt-get update`
   - `sudo apt-get upgrade`
 - Repeat above steps 'n' times for Pi's in bunch.

 - Networking the Pi's together. This example uses wifi to connect the Pis. As the goal is to create cluster it will be important to have static ip addresss for the Pis. Following steps are to setup static ip.

   - Use `ssh-keygen` to create id_rsa and id_rsa.pub tokens
   - Make copies of the .pub tokens relevant to source e.g. `cp id_rsa.pub node1`
   - Use secure copy to place tokens in receiving Pi's. For example if on node2 use: `scp pi@node1:~/.ssh/node1 .` to get a copy of the node1 .pub key.
   - Add the key to the authorized_keys file `cat node1>>authorized_keys`
   - Check now that `ssh pi@node2` no longer creates a password prompt.
   - If password prompt persists try: `ssh-copy-id -i pi@ip_address`
 
 - Install NFS server on one of the Pi's to create a mountable sharable directory
   - Install with: `sudo apt-get install nfs-kernel-server`
   - make a shared directory - `mkdir cloud`
   - add a line in `/etc/exports` - `/home/pi/cloud *(rw,sync,no_root_squash,no_subtree_check)`
   - update `cloud` privileges: `sudo chmod -R 777 /home/pi/cloud`
   - bind and restart nfs server `sudo update-rc.d rpcbind enable && \`
                                 `sudo service rpcbind restart && \`
                                 `sudo exportfs -a && \`
                                 `sudo service nfs-kernel-server restart`
   - mkdir new cloud directories and mount with: `sudo mount -t nfs node1:/home/pi/cloud ~/cloud`
   - check mount status with `df -h`
       
