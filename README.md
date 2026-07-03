# WSL2 + Windows VPN Configuration

If your VPN is connected on Windows but applications inside WSL2 (curl, wget, reverse shells, browsers, package managers, etc.) cannot access resources through the VPN, this configuration may help.

## .wslconfig

Create or edit:

```
%UserProfile%\.wslconfig
```

Add:

```text
[wsl2]
networkingMode=nat
dnsTunneling=true
autoProxy=true

memory=4GB
processors=4

[experimental]
hostAddressLoopback=true
autoMemoryReclaim=gradual
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
- Behavior may vary depending on the VPN client (OpenVPNclient tested).
- If you are a cyber budy enjoy playing labs on thm and htb without a vpm issues..
