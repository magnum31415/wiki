# Tuned
The tuned application stores the tuning profiles under the /usr/lib/tuned and /etc/tuned directories. Every profile has a separate directory, and inside the directory the tuned.conf main configuration file and, optionally, other files.

## Configure Static Tuning
The tuned daemon applies system settings when a service starts or on selecting a new tuning profile. Static tuning configures predefined kernel parameters in profiles that the tuned daemon applies at runtime. With static tuning, the tuned daemon sets kernel parameters for overall performance expectations, without adjusting these parameters as activity levels change.

## Configure Dynamic Tuning
By default, dynamic tuning is disabled. You can enable it by setting the dynamic_tuning variable to 1 in the /etc/tuned/tuned-main.conf configuration file. If you enable dynamic tuning, then the tuned daemon periodically monitors the system and adjusts the tuning settings to runtime behavior changes. 
With dynamic tuning, the tuned daemon monitors system activity and adjusts settings according to runtime behavior changes. Dynamic tuning continuously adjusts tuning to fit the current workload, starting with the initial declared settings in your selected tuning profile.

Monitor plug-ins analyze the system and obtain information from it, so the tuning plug-ins use this information for dynamic tuning. At this moment, the tuned daemon ships with three monitor plug-ins:
disk: Monitors the disk load based on the number of I/O operations for every disk device.
net: Monitors the network load based on the number of transferred packets per network card.
load: Monitors the CPU load for every CPU.


## Manage Profiles
Use the tuned-adm command to change the settings of the tuned daemon. The tuned-adm command queries current settings, lists available profiles, recommends a tuning profile for the system, changes profiles directly, or turns off tuning.

1. Check tuned is installed ````dnf list tuned````
2. Check tuned is enabled ````systemctl is-enabled tuned````
3. Check tuned is active ````systemctl is-active tuned````
4. List available tuning profiles ````sudo tuned-adm list````
5. Review the configuration file, ie. virtual-guest ````cat /usr/lib/tuned/virtual-guest/tuned.conf ````
6. Verify tuning profiles applies these values ````sysctl vm.dirty_ratio; sysctl vm.swappiness````
7. Change current active profile ````sudo tuned-adm profile throughput-performance ````
8. Confirm that throughput-performance is the active tuning profile. ````sudo tuned-adm active````
9. List all available profiles ````tuned-adm list````
10. Get recommended profile: ````tuned-adm recommend ````
11. Set default profile ````tuned-adm profile <name_profile_recommended> ````
12. Turn off the tuned application tuning activity ````tuned-adm off; tuned-adm active````






