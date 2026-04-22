# Terminal Access

Short note for connecting to Sovol SV08 by terminal.

## SSH connection

- Use: `ssh sovol@PRINTER_IP`
- Example: `ssh sovol@192.168.1.45`
- Username: `sovol`
- Password: `sovol`
- Expected: terminal opens a shell on the printer

## First commands after login

- `hostname`
- `ip a`
- `pwd`
- `ls`

Expected:

- You can see the printer hostname, network interfaces, and home directory contents

## If SSH does not connect

- Check that the printer web UI opens by IP in the browser
- Check that the PC and printer are on the same local network
- Check that you are using the correct IP address
- Try Ethernet if Wi-Fi is unstable
- Test with `ping PRINTER_IP`

## Useful Wi-Fi commands from SSH

- `nmcli r wifi on`
- `nmcli d wifi list`
- `nmcli d wifi connect "YOUR_WIFI_NAME" password "YOUR_PASSWORD"`
- `ip a`

Expected:

- Printer connects to Wi-Fi and shows a valid local IP address

## Note

- SSH access gives full terminal access to the printer
- Be careful with software updates on stock SV08 firmware