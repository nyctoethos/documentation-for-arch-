first thing to edit plymouth and grub

PLEASE DO NOT FUCK THIS UP!!!
YOU WILL BREAK YOUR GOD DAMN SYSTEM REMEMBER THAT
IF YOU BREAK IT I KNOW YOU GOD DAMN MIGHT 
YOU WILL HAVE TO REINSTALL GOD DAMN ARCH!!!
REMEMBER THIS WHILE DOING PLYMOUTH AND GRUB DO NOT MESS UP! TAKE EXTRA PRECAUTION IF YOU FUCK UP YOU WILL HAVE TO REDO THE WHOLE INSTALLATION OF ARCH!!!!!!!!

IF YOU ARE HAVING ISSUES AND MY DOCUMENTATION IS SHIT PLEASE REFER TO 
```https://wiki.archlinux.org/title/Plymouth```
```https://www.youtube.com/watch?v=eTk2yG1JFsE```
```https://wiki.archlinux.org/title/GRUB```



```Sudo pacman -S plymouth``` or for the development version ```plymouth-git```

by deafult plymouth logs the bootmessages in ~/var/log/boot.log and will not show the splash screen
we are going to make it so we see the splash screen so we need to append splash to the kernel parameters

when booting grub press "e" and add this " quiet splash " to 
```linux /boot/vmlinuz-linux root=UUID=0a3407de-014b-458b-b5c1-848e92a327a3 rw```
it should look like this
```linux /boot/vmlinuz-linux root=UUID=0a3407de-014b-458b-b5c1-848e92a327a3 rw quiet splash```
and now press  ```ctrl+X``` to boot with these paramaters 

To make it persisent after reboot which you will need to do edit ```sudo nvim ~/boot/grub/grub.cfg```
with this line just copy and paste it 
```GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"``` 

and now reboot the grub config with the command 
```sudo grub-mkconfig -o /boot/grub/grub.cfg```

Now after doing that 
Add plymouth to the HOOKs array in mkiniticpi.conf
```/etc/mkinitcpio.conf``` it should look like this
```HOOKS=(... plymouth ...)```

if you are using systemd hook it MUST be before mplymouth
after that regenerate it with ```mkinitcpio -p linux```

After installation of pymouth dracut will automatically detect it and add it to initframfs images if it fails you can force it to include plymouth with the line in ```sudo nano /etc/dracut.conf.d/myflags.conf```
```add_dracutmodules+=" plymouth "```



Now configuration we are going to use a custom version of plymouth that hasnt been made yet ill explain here but not in detail you will have to look this up yourself.

the configuration file is in ```/etc/plymouth/plymouthd.conf```

and the default values are in   ```/usr/share/plymouth/plymouthd.default```

a list of themes are ```    BGRT: A variation of Spinner that keeps the OEM logo if available (BGRT stands for Boot Graphics Resource Table)
    Fade-in: "Simple theme that fades in and out with shimmering stars"
    Glow: "Corporate theme with pie chart boot progress followed by a glowing emerging logo"
    Script: "Script example plugin" (Despite the description seems to be a quite nice Arch logo theme)
    Solar: "Space theme with violent flaring blue star"
    Spinner: "Simple theme with a loading spinner"
    Spinfinity: "Simple theme that shows a rotating infinity sign in the center of the screen"
    Tribar: "Text mode theme with tricolor progress bar"
    Text: "Text mode theme with tricolor progress bar"
    Details: "Verbose fallback theme" ``` 

to confirm theme run ```plymouth-set-default-theme``` 
to change the theme either
1. change it in ``` sudo nano /etc/plymouth/plymouthd.conf```
2. OR by running plymouth-set-default-theme -R theme

installing new themes 
install from AUR plymouth-kcm
but you will be uploading your own theme

```plymouth-set-default-theme -l```


the show the delay edit ```/etc/plymouth/plymouthd.conf```
to 
``` [Daemon] ShowDelay=5 ```
