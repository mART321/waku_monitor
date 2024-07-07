# waku_monitor
---

# Waku Node Monitoring Script

This script was created to monitor the state of the Waku node and send notifications to Telegram in case of errors or node shutdown. The script is developed for the community by ITRocket, based on the existing Waku node script to improve functionality and usability.

## Description

The script regularly checks the status of the Waku node using HTTP requests to the `/health` endpoint. Depending on the node's status, the script outputs appropriate messages to the console and sends notifications to Telegram.

### Key Features:
- Checks the node status every 5 minutes.
- Sends notifications to Telegram in case of node shutdown, errors, or unknown status.

## Installation and Usage

### Step 1: Preparation

1. Download the script and place it at `~/waku_node/nodehealth.sh`.
2. Replace the following variables with your values:
    - `TOKEN`: Your Telegram bot token.
    - `CHAT_ID`: The ID of your chat or group in Telegram.

```bash
TOKEN="your_bot_token"
CHAT_ID="your_chat_id"
```

3. Make the script executable:

```bash
chmod +x ~/waku_node/nodehealth.sh
```

### Step 2: Create System Service File

Create a system service file with the command:

```bash
sudo tee /etc/systemd/system/nodehealth.service > /dev/null <<EOF
[Unit]
Description=Node Health Service
After=network-online.target

[Service]
User=waku
WorkingDirectory=/home/waku/nwaku-compose
ExecStart=/bin/bash /home/waku/waku_node/nodehealth.sh
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```

### Step 3: Enable Service on Reboot

Enable the service to start on server reboot:

```bash
sudo systemctl enable nodehealth.service
```

### Step 4: Restart Daemon and Start Service

Restart the daemon and start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl restart nodehealth && sudo journalctl -u nodehealth -f
```

---
