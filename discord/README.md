I chose the `.deb` file for installation instead of Flatpak because, running Wayland, I needed a more up-to-date version of Discord that would support audio and screen sharing. 

But it appears to quickly become a pain in the ass because when starting the app, every time discord would get an new update, I was forced to download it and manually installed it or the program wouldn't start.

There were two options if I wanted to continue using Discord : deactivate the auto-update or create a script to automate the process. I choose the latter but since it's quite easy, I will explain here the first : 

## Deactivate auto-update 

Discord don't provide built-in function to do so.
Therefore to achieve that, we need to edit `nano ~/.config/discord/settings.json` and add the following line before the last closing bracket : 
```json
"SKIP_HOST_UPDATE": true
```

And that would dothe trick. 

## Automation 

### Script

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

The script tries to launch Discord, waits for a brief period, and then checks if it's running. If Discord fails to start (likely because an update is required), the `install_discord` function is triggered to download and install the latest version.

### Systemd

To ensure the script runs automatically and restarts if necessary, I used systemd which, in addition, allows to get real-time logging of the service to track any issues with Discord or the updater.
Therefore, i created a `~/.config/systemd/user/discord-launcher.service` file: 
```
[Unit]
Description=Discord Auto-Updater and Launcher
After=network.target

[Service]
# Ensure the script doesn't run if Discord is already running
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

## Future improvement

- Currently, the script assumes that if Discord is not starting, it is due to an update. That might not always be the case, and further consideration is needed to handle other potential reasons for Discord failing to start. At least, a feedback mechanism could be added to notify the user when the app doesn't start for other reasons.
