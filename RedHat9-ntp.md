# Time 

## Select the appropriate time zone for Haity.
````
#guess name for HAity TZ
tzselect

#Set TZ
sudo timedatectl set-timezone America/Port-au-Prince

#Verify that you correctly set the time zone 
timedatectl

````

## chronyd
````
# specify the classroom.example.com server as the NTP time source.
add in /etc/chrony.conf
server classroom.example.com iburst

sudo systemctl status/start chronyd

#check last step (The * in the S (Source state)  indicates  chronyd service uses the classroom.example.com  as a time source and is the NTP server that the machine is currently synchronized to.)
chronyc sources -v

#Enable time synchronization on the server
sudo timedatectl set-ntp true

#check last step (System clock synchronized: yes)
timedactl


````


