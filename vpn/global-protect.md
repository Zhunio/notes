# Global Protect

To fix GlobalProtect not working on iOS hotspot.

1. Add `vpn-hotspot` script to `$HOME/bin`

```bash
#!/bin/bash

# Fix VPN not working on iOS hotspot
networksetup -setmanual Wi-Fi 172.20.10.3 255.255.255.240 172.20.10.1
```

2. Add `vpn-wifi` script to `$HOME/bin`

```bash
#!/bin/bash

# Switch VPN back to Wi-Fi mode
networksetup -setdhcp Wi-Fi
```

3. Switch between the two mode(s):

when using hotspot:

```bash
vpn-hotspot
```

when using wifi:

```bash
vpn-wifi
```

