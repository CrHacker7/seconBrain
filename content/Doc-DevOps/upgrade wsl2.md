To check the WSL mode, run:
 wsl.exe -l -v

To upgrade the Linux distro to v2, run:
 wsl.exe --set-version (distro name) 2 // (distroName = Ubuntu-22.04)

To set v2 as the default version for future installations, run:
wsl.exe --set-default-version 2 // wsl --set-default Ubuntu-22.04 2
