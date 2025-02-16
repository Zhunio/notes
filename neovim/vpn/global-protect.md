# Global Protect

Fix VPN not working on iOS hotspot

```bash
networksetup -setmanual Wi-Fi 172.20.10.3 255.255.255.240 172.20.10.1
```

Switch VPN back to WiFi mode

```bash
networksetup -setdhcp Wi-Fi
```
