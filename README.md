# dummy-systemd-service
Simple steps to follow to create a systemd service


# Step 1: Create the dummy.sh Script
Open a terminal and create the sample script file:
```
sudo nano /usr/local/bin/dummy.sh
```
ensure it is in the /usr/local/bin directory

Add the following content to the script:

```
#!/bin/bash
while true; do
  echo "Dummy service is running..." >> /var/log/dummy-service.log
  sleep 10
done
```

Grant the script executable permission:
```
sudo chmod +x /usr/local/bin/dummy.sh
```

# Step 2: Create the Systemd Service File
Create the systemd service file:
```
sudo nano /etc/systemd/system/dummy.service
```

Add the following content:

```
[Unit]
Description=Dummy Service to Simulate Background Process
After=network.target

[Service]
ExecStart=/usr/local/bin/dummy.sh
Restart=always
RestartSec=5
StandardOutput=append:/var/log/dummy-service.log
StandardError=append:/var/log/dummy-service.log
User=root

[Install]
WantedBy=multi-user.target
```


# Step 3: Reload Systemd and Start the Service
Reload the systemd daemon to recognize the new service:
```
sudo systemctl daemon-reload
```

Start the dummy service:
```
sudo systemctl start dummy.service
```

Enable the service to start automatically on boot:
```
sudo systemctl enable dummy.service
```


# Step 4: Verify Service Status
Check if the service is running:

```
sudo systemctl status dummy.service
```

Check the log output to confirm it is writing to log file
```
tail -f /var/log/dummy-service.log
```


# Step 5: Manage the Service
The service can be controlled using the following commands:

Stop the service:
```
sudo systemctl stop dummy.service
```
Restart the service:
```
sudo systemctl restart dummy.service
```

Disable the service (prevent it from starting on boot):
```
sudo systemctl disable dummy.service
```

Re-enable the service:
```
sudo systemctl enable dummy.service
```

View service logs via journalctl:
```
sudo journalctl -u dummy.service -f
```



Step 6: Test Service Failure Recovery
To verify that the service restarts if it fails, kill the process manually:
```
sudo pkill -f dummy.sh
```
Then check the status again:
```
sudo systemctl status dummy.service
```
The service should restart within 5 seconds.




# Step 7: Clean Up (Optional)
If you want to remove the service and stop it completely:

Stop and disable the service:

```
sudo systemctl stop dummy.service
sudo systemctl disable dummy.service
```

Remove the service file:
```
sudo rm /etc/systemd/system/dummy.service
```

Remove the script:
```
sudo rm /usr/local/bin/dummy.sh
```

Reload the systemd daemon:
```
sudo systemctl daemon-reload
```
