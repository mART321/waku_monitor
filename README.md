# Waku Node Monitoring Script

This script was created to monitor the state of the Waku node and send notifications to Telegram in case of errors or node shutdown. The script is developed for the community by ITRocket, based on the existing Waku node script to improve functionality and usability.

## Description

The script regularly checks the status of the Waku node using HTTP requests to the `/health` endpoint. Depending on the node's status, the script outputs appropriate messages to the console and sends notifications to Telegram.

### Key Features:
- Checks the node status every 5 minutes.
- Sends notifications to Telegram in case of node shutdown, errors, or unknown status.

## Installation and Usage

### Step 1: Preparation

1. Download the script
~~~
cd $HOME
wget -O monitoring-weku.sh https://raw.githubusercontent.com/mART321/waku_monitor/main/monitoring-waku.sh
~~~

2. Configure Telegram alerting:
Open Telegram and find `@BotFather`
- Here are the [instructions](https://sematext.com/docs/integration/alerts-telegram-integration/)
- How to get [chat id](https://stackoverflow.com/questions/32423837/telegram-bot-how-to-get-a-group-chat-id)

After creating telegram bot and group, specify the variables in the autonity-monitoring.sh:
- set values for `TELEGRAM_BOT_TOKEN=`, `TELEGRAM_CHAT_ID=""`
~~~
nano ~/monitoring-waku.sh
~~~

3. Make the script executable:

```bash
chmod +x ~/monitoring-waku.sh
```

### Step 2: Create Service File

Create a system service file with the command:

```bash
sudo tee /etc/systemd/system/monitoring-waku.service > /dev/null <<EOF
[Unit]
Description=Waku Node Health Service
After=network-online.target

[Service]
User=User
WorkingDirectory=$HOME
ExecStart=/bin/bash $HOME/monitoring-waku.sh
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```

### Step 3: Enable and start service

Enable and start service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable monitoring-waku
sudo systemctl restart monitoring-waku && sudo journalctl -u monitoring-waku -f
```

Voilà! Enjoy the script ;)

## Delete
~~~
sudo systemctl stop monitoring-waku
sudo systemctl disable monitoring-waku
sudo rm -rf /etc/systemd/system/monitoring-waku.service
rm ~/monitoring-waku.sh
sudo systemctl daemon-reload
~~~
