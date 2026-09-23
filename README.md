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

Run cmd as admin:

```powershell
:: Forward port 1337 from Windows into WSL
netsh interface portproxy add v4tov4 listenport=1337 listenaddress=0.0.0.0 connectaddress=WSL_IP connectport=1337

:: Open firewall for it
netsh advfirewall firewall add rule name="WSL Shell 1337" dir=in action=allow protocol=TCP localport=1337
```

Restart WSL:

```powershell
wsl --shutdown
```

Then start WSL again.

## Result

After restarting:

- Now you can use `nc -nlvp 1337` inside wsl2 and receive reverse shell.
- WSL can use the Winwodws VPN connection.. DNS resolution works through the VPN.
- Applications running inside WSL can communicate using the same VPN path as the Windows host.

## Notes

- Requires a recent version of WSL.
- Behavior may vary depending on the VPN client (OpenVpnClient tested).
