# SWAP

## Create swap partition

1. Identify Available Disk Space
   ````
   sudo lsblk
   ````
2. Create a New Partition for Swap
   ````
   sudo fdisk /dev/sdX
   ````
   O con parted
   ````
   sudo parted /dev/sdb
   (parted) mklabel gpt
   (parted) mkpart primary linux-swap 1MiB 4GiB
   (parted) print
   (parted) quit


    sudo parted /dev/vdb mkpart <paritionName> linux-swap 1001MB 1101MB
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
   lsblk -o UUID /dev/sdXn
   echo "UUID=cc18...1b69 swap swap defaults 0 0" >> /etc/fstab
   
   echo "/dev/sdXn swap swap defaults 0 0" | sudo tee -a /etc/fstab

   swapon -a
   ````
7. Verify Swap Space
   ````
   free -h
   ````
