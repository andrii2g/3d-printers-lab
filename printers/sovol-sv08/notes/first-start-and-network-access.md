# First Start and Network Access

Short first-start checklist for getting the Sovol SV08 on the network and opening the web UI.

## 1. Basic first power-on check

- Do: power on the printer and wait until the screen finishes loading
- Check: screen responds, no immediate error is shown, fans and motion system sound normal
- If OK: continue to network setup

## 2. Choose connection type

- Option A: connect by Ethernet for the simplest first setup
- Option B: connect by Wi-Fi if the printer is close enough to your router
- Check: use the same local network as the PC or phone you will use for the web UI
- If OK: continue to network connection

## 3. Connect to Wi-Fi

- Do: open the printer network or Wi-Fi settings and select your home network
- Enter: Wi-Fi password carefully
- Check: use `2.4 GHz` Wi-Fi with `WPA2`; SV08 does not support WPA3-only networks
- Expected: printer shows as connected to the network
- If not OK: try Ethernet first, or check that your router exposes a 2.4 GHz network the printer can join

## 4. Or connect by Ethernet

- Do: plug an Ethernet cable from the printer to your router or switch
- Expected: printer receives a local IP address automatically from DHCP
- If OK: continue to IP discovery

## 5. Find printer IP address

- Do: look in your router admin page or DHCP client list for the printer
- Check: find the IP assigned to the SV08 on your local network
- Expected: IP looks like a local address such as `192.168.x.x` or `10.x.x.x`
- Do not use: `127.0.0.1`, because that is only the printer's local loopback address

## 6. Open the web UI

- Do: on your PC or phone, open a browser and enter `http://PRINTER_IP`
- Example: `http://192.168.1.45`
- Expected: the SV08 web UI opens, usually Mainsail or Fluidd
- If OK: continue with homing, gantry leveling, and calibration

## 7. If the UI does not open

- Check: PC and printer are on the same local network
- Check: printer still has the same IP in the router
- Check: browser is using `http://`, not a search query
- Try: ping the printer IP from your PC
- Fallback: use Ethernet first, then return to Wi-Fi later

## 8. Good next step after first connection

- Do: save the printer IP in bookmarks
- Optional: reserve the printer IP in the router DHCP settings so it stays the same
- Then: continue with the calibration checklist