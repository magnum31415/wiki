# Manage Profiles
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





