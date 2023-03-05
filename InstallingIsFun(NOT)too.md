## Οδηγός εγκατάστασης Funtoo Linux σε VirtualMachine

> _*Author: [aggelos2000430](https://github.com/aggelos2000430)
Ο συγγραφέας των οδηγιών δεν φέρει καμία ευθύνη σε περίπτωση που κάτι πάει στραβά στην προσπάθεια εγκατάστασης του λειτουργικού στο σύστημα σας*_

[Κατεβάζουμε το LiveCD από εδώ](https://www.funtoo.org/Funtoo:New_Install_Experience/LiveCD/Releases/20221229-0411)

έπειτα επιλέγουμε >Machine >New >Name: Funtoo, επιλέγουμε για ISO Image το LiveCD που κατεβάσαμε και επιλέγουμε >Type: Linux, >Version: Gentoo (64-bit) 
έπειτα >Next, >Base Memory: 4096MB >Processors: 4CPUs και >Disk Size: 32GB

### Boot-άρουμε το Virual-Machine
`fdisk /dev/sda` και  `o` για διαγραφή του partition

### Δημιουργία disk partition
![boot](https://user-images.githubusercontent.com/73399706/220938466-085afbde-001a-4ed1-b14f-531ae599630a.png)</br>

![root](https://user-images.githubusercontent.com/73399706/220939508-77b397c8-e737-4398-8618-2fe2c67f5758.png)

και τώρα w και >_*enter*_< για να γράψουμε τους δίσκους

`mkfs.ext2 /dev/sda1`

`mkfs.ext4 /dev/sda2`

`mkdir -p /mnt/funtoo`

`mount /dev/sda2 /mnt/funtoo`

`mkdir /mnt/funtoo/boot`

`mount /dev/sda1 /mnt/funtoo/boot`

`cd /mnt/funtoo`

`links http://build.funtoo.org`

[ΟΚ] >_*enter*_

`next/`

`x86-64bit/`

`generic_64/`

`2023-01-31`

επιλέγουμε `kde-stage3-generic_64-next-2023-01-31.tar.xz` 
και >_*enter*_ [Save] >_*enter*_ για να κατεβάσουμε το αρχείο που είναι μεγέθους 2.0 GB

`q` για να βγούμε απ το `links`

### extract του αρχείου που κατεβάσαμε
`tar --numeric-owner --xattrs --xattrs-include='*' -xpf kde-stage3-generic_64-next-2023-01-31.tar.xz` 

### Chroot
`fchroot /mnt/funtoo /bin/bash --login`

### Download Portage Tree
`ego sync`

### Config Files
`nano -w /etc/fstab` το αρχείο απο την πρώτη εικόνα πρέπει να το επεξεργαστείτε έτσι ώστε να γίνει όπως στην δεύτερη εικόνα 
![nano](https://user-images.githubusercontent.com/92797427/220631516-af10a8cb-74e7-4649-a08b-3b302df7e787.png)
για να βγούμε από το nano: ctrl+x, Y, >_*enter*_<
`rm -f /etc/localtime`
`ln -sf /usr/share/zoneinfo/Europe/Athens /etc/localtime`


### System Update
`emerge -q -auDN @world`

### Bootloader
`emerge -av grub`
`grub-install --target=i386-pc --no-floppy /dev/sda`
`ego boot update`

### Network Set Up 
Ethernet
1. `rc-update add dhcpcd default`
2. `nano /etc/conf.d/hostname` και αλλάζουμε το hostname

### Setting Users
1. `passwd` κωδικός για τον root user
2. `useradd -m βάλτεΤονΑΜσαςΕδώ`
4. `usermod -G wheel,audio,video,plugdev,portage myuser`
5. `passwd myuser`

### Exit Environment and Reboot
`exit`
`cd /mnt`
`umount -lR funtoo`
`shutdown` για να επιστρέψουμε στη αρχική του livecd
Τερματίζουμε το vm μας
Από τις ρυθμίσεις του VM στο storage αφαιρούμε το livecd απο το "IDE Secondary Device" 

![storage](https://user-images.githubusercontent.com/77148351/220625668-b513b877-5100-4a39-9ea5-91a11079226d.png)

### Post Installation Configuration
Σε αυτό το σημείο καλό είναι να σετάρουμε τα sudo privilege του user μας.
Για να το καταφέρουμε αυτό θα χρειαστεί να κατεβάσουμε το sudo package.
`su` και βάζουμε τον κωδικό του root user
`emerge --ask app-admin/sudo`

Φτάσατε στο Τέλος...
Εαν δεν δουλέψει με την πρώτη φορά προσπαθήστε ΞΑΝΑ! 
