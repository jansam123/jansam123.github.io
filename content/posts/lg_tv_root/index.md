---
title: "Install AD free Youtube on LG TV"
date: 2022-09-18T08:06:25+06:00
description: "Tips and tricks for installing AD free Youtube on LG TV using the webOS TV SDK and a custom web app."
theme: Toha
menu:
  sidebar:
    name: AD free Youtube on LG TV
    identifier: lg_tv_root
    weight: 994
hero: homebrew.png
---
# [Installing AD free Youtube on LG TV](https://github.com/webosbrew/webos-homebrew-channel)

1.  **Developer mode** 
    Put your TV into developer mode using the following guide: [https://www.webosbrew.org/devmode/](https://www.webosbrew.org/devmode/)
2. **Get the SSH key for your TV**
    Go to the Developer mode app on your TV. Turn on the "Dev Mode Status" and "Key Server". 
    Run the following commands to get the SSH key for your TV and add it to your SSH config:
    ```bash
    ares-setup-device -a mytv -i "username=prisoner" -i "host=192.168.1.50" -i "port=9922" -i "default=true"
    ares-novacom --device mytv --getkey
    ares-device --device mytv
    ```
    The key lives at `~/.ssh/mytv_webos`.
3.  **SSH into TV**
    Use the IP address of your TV displayed in the Developer mode app to SSH into your TV using the following command:
    ```bash
    ssh -o HostKeyAlgorithms=+ssh-rsa -o PubkeyAcceptedAlgorithms=+ssh-rsa -i ~/.ssh/mytv_webos -p 9922 prisoner@10.1.0.92
    ```
    Make sure to replace `~/.ssh/mytv_webos` with the path to your SSH private key.

4. **Run the install script**
    ```bash
    curl -L https://raw.githubusercontent.com/webosbrew/webos-homebrew-channel/main/tools/install.sh | sh -
    ```
5. **Install the web app**
    You should now see the "Homebrew Channel" app on your TV. Open it and scroll all the way down to the Youtube app. Click on it and install it. You should now have an AD free Youtube app on your LG TV!