# Forward windows vpn connection to wsl2

If your VPN is connected on Windows but applications inside WSL2 (curl, wget, reverse shells, browsers, package managers, etc.) cannot access resources through the VPN, this configuration may help.

## .wslconfig

Create or edit:

```
%UserProfile%\.wslconfig
```

Add:

```text
[wsl2]
# Enable mirrored networking mode
networkingMode=nat
dnsTunneling=true
autoProxy=true

# Allow Windows Firewall to manage WSL ports
# firewall=true

# Limit system resources to prevent freezing (Example: 8GB RAM machine)
memory=4GB
processors=4

[experimental]
# Improve IP mapping and loopback stability
 hostAddressLoopback=true

# Automatically reclaim unused RAM from Linux
autoMemoryReclaim=gradual

# Automatically shrink virtual disk size (VHDX)
sparseVhd=true
```

Restart WSL:

```powershell
wsl --shutdown
```

Then start WSL again.

## Result

After restarting:

- WSL can use the Windows VPN connection.
- DNS resolution works through the VPN.
- `curl`, `wget`, `git`, package managers, and other network tools can reach VPN-only resources.
- Applications running inside WSL can communicate using the same VPN path as the Windows host.

## Notes

- Requires a recent version of WSL.
- Behavior may vary depending on the VPN client (OpenVpnClient tested).
