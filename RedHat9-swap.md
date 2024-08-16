# SWAP

## Create swap partition

1. Identify Available Disk Space
   ````
   lsblk
   ````
3. Create a New Partition for Swap
   ````
   sudo fdisk /dev/sdX
   ````
5. Format the Partition as Swap
   ````
   sudo mkswap /dev/sdXn
   ````
7. Enable the Swap Partition
   ````
   sudo swapon /dev/sdXn
   ````
9. Verify the Swap
   ````
   sudo swapon --show 
   ````
11. Make the Swap Permanent
   ````
   echo '/dev/sdXn swap swap defaults 0 0' | sudo tee -a /etc/fstab
    ````
13. Verify Swap Space
   ````
   free -h
   ````
