## FIX for the NVMe Corruption  
This fix is making it possible to use the native NVMe driver from apple on many machines. There is no drawback, because all modern OS (like Windows 7 and newer) support 4k sector sizes. After this step your SSD will be unreadable and must be reformatted. You'll loose all your data.   

## Do first
First check your firmware upgrades for your SSD. Especially if you use Toshiba drives, these have a critical "drive disappearing" bug which can happen at any time. See http://www.dell.com/support/home/en/en/debsdt1/Drivers/DriversDetails?driverId=2N42W.  
Some people reported problems with identification of a 4Ke formatted drive in its Dell Notebooks (looks like the disappearing bug). Recovery is sometimes possible by restarting multiple times until the disk is visible again and switching back to the 512b mode.  


## Ok lets start
Boot from Ubuntu 16.10 Live USB  
Enable Universe repository and reload repo database  
check the device for your ssd (can be /dev/nvme0, /dev/sda0 or something completly different.  
open the terminal  
```
sudo apt-get install smartmontools  
sudo apt-get install nvme-cli  
sudo smartctl -a /dev/nvme0  
```  
Check the output. If you have an entry with 4096, then your SSD is 4k compatible and you can use the native SSD configuration  
```
Supported LBA Sizes (NSID 0x1)  
Id Fmt  Data  Metadt  Rel_Perf  
 0 +     512       0         2  
 1 -    4096       0         1  
```

the setting with the + in front is the active one  
You can switch the settings by reformating it:  
`nvme format -l 1 /dev/nvme0`
  
Now remove SSDT-Hackr.aml from EFI/ACPI/patched, the hackrnvmefamily kext or the clover storage hotpatches from your EFI bootloader and reinstall OSX
