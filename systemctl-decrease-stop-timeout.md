# Decreasing systemctl's annoying default 90sec stop timeout


```
sed -i 's/#DefaultTimeoutStopSec=90s/DefaultTimeoutStopSec=5s/g' /etc/systemd/system.conf
systemctl daemon-reload
```
