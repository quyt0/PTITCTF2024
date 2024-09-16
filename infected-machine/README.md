# Write-up: InfectedMachine in Forensics

##  1.  Analyzing the Challenge

The challenge provides a very heavy compressed file containing `chall-disk1.vmdk`. For the first time tried to open it by using VirtualBox. I got the Blue Screen of Death
![enter image description here](https://raw.githubusercontent.com/quyt0/PTITCTF2024/main/infected-machine/vm-bsod.png)

I read the hint, it told me to check the shortcuts of Desktop. So that I think I have to access the Desktop folder through VirtualBox. However, I felt really confused because of the BSOD 😒

## 2. Trying to access 

After that, I realized that the system is Windows XP. However, I was currently running the config of Windows 10 when install.

Then I change to Windows XP and successfully accessed to the VM.
![enter image description here](https://raw.githubusercontent.com/quyt0/PTITCTF2024/main/infected-machine/winxp-hint.png)

But look, the system required password. I saw the password hint said that "find another way" :(

After a while searching on Google, I realized that the file `chall-disk1.vmdk` can be opened using `7-Zip`. LOL

Finally, I extracted it and check all of the shortcuts in Desktop at `\chall-disk1\Documents and Settings\Administrator\Desktop`. At the Notepad shortcut, I saw the flag in Target box:  ` PTITCTF{t4k3_4_5h0tcut_0f_c0ff33}`
![enter image description here](https://raw.githubusercontent.com/quyt0/PTITCTF2024/main/infected-machine/flag.png)
