I used the .deb file to install Discord. I made that choice rather than using flatpak because, because running Wayland, I needed a more up to date version that would support audio and screen sharing. 

But it appears to quickly become a pain in the ass because every time discord would get an new update, when starting the app, I was forced to download it and manually installed it or the program wouldn't run.

There was two option if I wanted to continue using Discord : deactivate the auto-update or create a script to automate the process. I choose the latter but since it's quite easy, I will explain here the first : 

## Deactivate auto-update 

For that, we just need to edit `nano ~/.config/discord/settings.json` and add the following line before the last closing bracket : 
```json
"SKIP_HOST_UPDATE": true
```

And that would make the trick. 

## Automation 

### Script

I made the following script : 
```bash
#!/bin/bash

# Function to check if Discord is installed
function install_discord {
    echo "🔄 Downloading the latest Discord version..."
    URL=$(curl -s https://discord.com/api/download?platform=linux&format=deb | grep -oP '(?<=href=")[^"]+')
    FILENAME="/tmp/discord-latest.deb"

    wget -O "$FILENAME" "$URL"
    sudo dpkg -i "$FILENAME"
    rm "$FILENAME"
    echo "✅ Discord updated successfully!"
}

# Try to launch Discord
discord &

# Wait briefly to see if it fails
sleep 10

# Check if Discord is still running
if ! pgrep -x "Discord" > /dev/null; then
    echo "⚠️  Discord failed to start, likely due to an update requirement."
    install_discord
    discord &
fi
```

The idea there was when an update was available, the app won't actually start. Therefore the sleep time to wait and check again if Discord successfully start and launch the install_discord function if it didn't run. 

### Logging 

I wanted to be able to check if the script was running well, i created a systemd unit to be able to implement it in journalctl. 
Therefore, i created a `~/.config/systemd/user/discord-launcher.service` : 
```
[Unit]
Description=Discord Auto-Updater and Launcher
After=network.target

[Service]
# Check if Discord is already running. If yes, exit the script
ExecStartPre=/bin/sh -c '! pgrep -x discord || exit 1'
ExecStart=%h/bin/discord-launcher.sh
StandardOutput=journal
StandardError=journal
# Automatically run the script on process failure.
Restart=on-failure
RestartSec=10s
Type=simple

[Install]
WantedBy=default.target
```

Then i could enable the service and launch it : 

```bash
systemctl --user enable discord-launcher
systemctl --user start discord-launcher
```

Now we can check logs using : 

```bash 
journalctl --user -u discord-launcher -n 50 --no-pager
```

