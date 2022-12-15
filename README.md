# WebStorm with Windows Subsystem for Linux on Windows 10

This setup is a short summary of the steps needed to install WSLg on Windows 10 and then installing WebStorm in it and linking it to Windows.

## Updating Windows for WSLg

Windows needs to be rather up to date to support the WSLg backport from Windows 11.
If you got the 21H2 Update then you are good to go.

## Installing the Windows Store WSL app

There are now three editions of WSL: WSL 1, WSL 2 and WSL Store App (see: https://devblogs.microsoft.com/commandline/the-windows-subsystem-for-linux-in-the-microsoft-store-is-now-generally-available-on-windows-10-and-11/)
This store app is the new intended way of using WSL for the future and is the most up to date and does not require enabling of system components.

It also contains the backport of the wslg feature, which was initially only released for Windows 11.
I would reccommend that you delete (`wsl --unregister <distro>`)  your other WSL distros before installing the new app.

Then install the "Windows Subsystem for Linux" app (https://apps.microsoft.com/store/detail/windows-subsystem-for-linux/9P9TQF7MRM4R) from the Microsoft store and start it.
If you receive an error concerning the supported components, then you need to update your system.
If you don't receive the update automaticly then you can update your system manually by installing the required update from https://www.catalog.update.microsoft.com/Search.aspx?q=KB5020030

The app should then be able to start and install a Ubuntu 22.04 automaticly.
You can install another distro by using the `wsl --list --online` and then using `wsl --install <distro>`

## Updating and installing linux system components

When you setup your Ubuntu you should proceed by updating the system `sudo apt update` and `sudo apt upgrade`.
After that you can install and run gui apps.
For example you can try gedit `sudo apt install gedit` and then running `gedit`.

This should open up a new window which looks just like gedit would on Ubuntu.

## Installing WebStorm

Installing WebStorm is a bit more tricky.
Be aware that this does not install required dependencies to display the window.
To quickly install the required dependencies you can just install gedit, so Webstorm can use them as well.

```bash
# Downloading a specific version
curl https://download-cdn.jetbrains.com/webstorm/WebStorm-2022.3.tar.gz -o WebStorm-2022.3.tar.gz

# Unpacking
tar -xzf WebStorm-2022.3.tar.gz

# Moving to a permanent place
mv WebStorm-223.7571.168 /opt/

# Linking the executable
ln -s /opt/WebStorm-223.7571.168/bin/webstorm.sh /usr/local/bin/webstorm

# Copying the icons
# see: https://hjoelr.medium.com/wsl2-gui-app-shortcuts-in-windows-with-wslg-fcc66d3134e7
# DO THIS BEFORE CREATING THE DESKTOP FILE!
cp /opt/WebStorm-223.7571.168/bin/webstorm.png /usr/share/icons/hicolor/128x128/apps/webstorm.png
cp /opt/WebStorm-223.7571.168/bin/webstorm.svg /usr/share/icons/hicolor/scaleable/apps/webstorm.svg

# Creating a Linux / Windows startmenu entry
sudo tee /usr/share/applications/webstorm.desktop <<EOF
[Desktop Entry]
Name=WebStorm 2022.3
Comment=WebStorm IDE by Jetbrains
Exec=webstorm
Terminal=false
Type=Application
StartupNotify=true
MimeType=text/plain;
Icon=org.gnome.gedit
Categories=TextEditor;IDE;
EOF
```

After that, WebStorm should be present in your windows start menu.
