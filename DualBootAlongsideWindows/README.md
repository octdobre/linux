
# Dual Boot alongside Windows Legacy BIOS Only

Easy no-hassle dual boot **Ubuntu** alongside **Windows**. ONLY LEGACY **BIOS** :older_man:, NOT **UEFI**!!!

## Prerequisites 
   * USB Stick 8GB
   * Computer with Pre-installed **Windows 10,11** operating system.
   * **Rufus**
   * **EasyBCD**

   
## Steps:
  
   :floppy_disk: Create a small 500MB `partition`. Use the *Disk Management* tool on Windows to shrink the `C:` `drive` by 500MB. 
   
   Create a *bootable* **Ubuntu** medium :cd: using **Rufus**.

   Restart :computer:`PC` and start the **Ubuntu** `installer`.

   Select  :point_right: "Something Else" in the "Installation Type" menu.

   Install `/boot` in the 500MB `partition` we just created (will  install :point_right: **grub** `bootloader` here).

   Install `/` (the OS itself) on whatever `drive` or `partition` you wish.

   :walking: Proceed normally with installation.

   :exclamation: After installation is completed, remember that we still cannot yet load **Ubuntu** because we have not installed an **EFI** `partition`. We do not need one in this case because there is already one for **Windows**.

   :arrows_counterclockwise: Reboot computer and *boot* into **Windows**.

   Start **EasyBCD**. Add another `boot` :cd: entry to the **Windows BootManager**
   Select the 500MB `partition` where we installled `/boot` as the *boot* `partition` for **Ubuntu**.

   Now you will see 2 `OS` options in **Windows BootManager**. If you select **Ubuntu**, **grub** will load with the classic `boot` menu.

  
## Optional:
* You can use **EasyBCD** to change :arrows_counterclockwise: the boot order in case you want **Ubuntu** as the first option.
* :hourglass_flowing_sand: You can edit the **grub** settings in **Ubuntu** to show the **grub** boot menu for `0` seconds if you wish to	skip:leftwards_arrow_with_hook: it.

:balloon: Have fun with your new dual `boot`. :smile:





## :books: Documentation

🔗 [Dual Booting Ubuntu](https://www.youtube.com/watch?v=-iSAyiicyQY&t=564s 'Dual Booting Ubuntu')
