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
# specify the classroom.example.com server as the NTP time source. add in /etc/chrony.conf
server classroom.example.com iburst

sudo systemctl enable --now chronyd

#check last step (The * in the S (Source state)  indicates  chronyd service uses the classroom.example.com  as a time source and is the NTP server that the machine is currently synchronized to.)
chronyc sources -v

#Enable time synchronization on the server
sudo timedatectl set-ntp true

#check last step (System clock synchronized: yes)
timedactl


````

# Rsyslog

## Configure the rsyslog service to write the Logging test authpriv.alert message to the /var/log/auth-errors file. Use the authpriv facility and the alert priority.
````
#create & add in /etc/rsyslog.d/auth-errors.conf
authpriv.alert  /var/log/auth-errors

systemctl restart rsyslog.service

logger -p authpriv.alert "Login test authpriv.alert"

tail -f /var/log/auth-errors
````

