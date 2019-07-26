# iptables-docker-systemd
Restart docker service if iptables has been restarted

```bash
[Unit]
Description=IPv4 firewall with iptables
After=syslog.target docker.service
AssertPathExists=/etc/sysconfig/iptables
PartOf=docker.service
Requiers=docker.service

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/usr/libexec/iptables/iptables.init start
ExecReload=/usr/libexec/iptables/iptables.init reload && /usr/bin/systemctl restart docker.service
ExecStop=/usr/libexec/iptables/iptables.init stop
ExecStartPost=/usr/bin/systemctl restart docker.service
Environment=BOOTUP=serial
Environment=CONSOLETYPE=serial
StandardOutput=syslog
StandardError=syslog

[Install]
WantedBy=basic.target
```
