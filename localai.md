# LocalAI

Quick start https://localai.io/basics/getting_started/

Install as systemd service
```bash
curl https://localai.io/install.sh | sh
```

Edit the systemd service to change the location for models to directory with ssd mounted
```bash
sudo vim /etc/systemd/system/local-ai.service
```

Reload and restart local-ai service
```bash
sudo systemctl daemon-reload
sudo systemctl status local-ai
sudo systemctl stop local-ai
sudo systemctl start local-ai
```

Access WebUI at http:<pi_ip>:8080
