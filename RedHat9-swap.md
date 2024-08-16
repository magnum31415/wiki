# SWAP

## Create swap partition

1. Identify Available Disk Space
   ````
   lsblk
   ````
2. Create a New Partition for Swap
   ````
   sudo fdisk /dev/sdX
   ````
3. Format the Partition as Swap
   ````
   sudo mkswap /dev/sdXn
   ````
4. Enable the Swap Partition
   ````
   sudo swapon /dev/sdXn
   ````
5. Verify the Swap
   ````
   sudo swapon --show 
   ````
6. Make the Swap Permanent
   ````
   echo "/dev/sdXn swap swap defaults 0 0" | sudo tee -a /etc/fstab
   ````
7. Verify Swap Space
   ````
   free -h
   ````
