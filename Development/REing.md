# Setup guide for basic REing, for development


### Prerequisites:
- [Fiddler](https://www.telerik.com/fiddler) (or any other proxy software, though this guide is specific to Fiddler)
- [WireShark](https://www.wireshark.org/)
- [VirtualRouter](https://archive.codeplex.com/?p=virtualrouter) (used to easily turn your Windows PC into a hotspot. This can be done without this software, however I find that it makes it easier)
- Homebrew on your [WiiU](https://wiiu.hacks.guide) or [3DS](https://3ds.hacks.guide) (depending on which console you want to RE for)
- [NodeJS](https://nodejs.org/en/) (our servers are written in Node)

### Setup (Computer):
1. In Fiddler, open `Tools > Options`
2. In the `HTTPS` tab turn on HTTPS Connects
3. Enable HTTPS decrypting
4. Ignore server certificate errors
5. In the `Connections` tab tick `Allow remote computers to connect`
6. Turn off `Act as system proxy on startup`
7. Back in the `HTTPS` tab click `Actions > Export Root Certificate to Desktop`
8. Rename `FiddlerRoot.cer` to `CACERT_NINTENDO_CA_G3.der` wherever you exported it to. NOTE: MAKE SURE TO CHECK THE FILE EXTENSION. IT SHOULD BE `.der` NOT `.cer`
9. Go complete one of the console sections

# Setup (Consoles)

### WiiU:
1. Install FTPiiU_Everywhere (NOT THE REGULAR FTPiiU!)

### 3DS:
1. Wait for whiskers to set this up

# Sniffing and REing requests

## WiiU
### HTTP
1. Launch in to Mocha (redNAND or sysNAND, doesn't matter. Do which ever you want)
2. Launch the Homebrew Launcher and startup FTPiiU_Everywhere
3. Connect to the FTP server on your computer using any FTP client (FileZilla/WinSCP on Windows or Finder on MacOS for example)
4. Navigate to `/vol/storage_mlc01/sys/title/0005001b/10054000/content`
5. Dump the `ccerts` folder to your PC (Will be used later. The file we will be using is common to ALL WiiU consoles, so you can download this file online if you do not have a WiiU)
6. Enter `scerts` and replace the `CACERT_NINTENDO_CA_G3.der` file there with the `CACERT_NINTENDO_CA_G3.der` file we just made earlier from the FiddlerRoot.cer (MAKE SURE TO BACK UP THE ORIGINAL. REPLACING THIS CERT MAKES YOU UNABLE TO CONNECT TO THE OFFICIAL SERVERS WITHOUT THE PROXY, YOU MUST CHANGE IT BACK IF YOU WANT TO CONNECT TO THE OFFICIAL SERVERS WITHOUT THE PROXY). AGAIN MAKE SURE TO CHECK THE FILE EXTENSION, IT MUST BE `.der`
7. Close the FTP connection on your computer and exit FTPiiU_Everywhere (press Home button)
8. Reboot your console (DO NOT FORCE REBOOT (holding power button for 4 seconds)! REBOOT NORMALLY (only hold for 2 seconds)! FORCE REBOOTING CAN SOMETIMES ERASE CHANGES!)
9. Go to the connection settings for your WiFi connection on your WiiU and turn on proxy connections
10. Set the proxy server to your PC's IP, and the port to `8888` (unless you changed the Fiddler port)
11. On your PC go to where you dumped `ccerts` and copy `WIIU_COMMON_1_CERT.der` (again, this file is common to all consoles. You can find it online if you don't have a WiiU)
12. Paste the cert in `%USERPROFILE%\My Documents\Fiddler2\` and rename it to `ClientCertificate.cer`

You should now see all HTTP(S) traffic from your console feeding into Fiddler. With this you can RE the HTTP(S) endpoints with ease

### PRUDP/UDP (THIS IS WHAT GAMES USE)
1. Start VirtualRouter (or turn your PC into a hotspot by any other means)
2. On your WiiU, connect to the hotspot
3. Start up WireShark
4. Select the VirtualRouter connection to sniff (for me this is called `Local Area Connection2`)
5. Click `Capture > Options`
6. Enter the `Options` tab
7. Enable all name resolution options
8. Apply the filter `ip.addr eq YOUR.WIIU.CONSOLE.IP && udp`

You should now see all UDP/PRUDP packets being sent to and from the WiiU. Unpack the packets and decrypt the payload, and you will be able to being REing the packet

## 3DS
### HTTP
Wait for whiskers

### PRUDP/UDP (THIS IS WHAT GAMES USE)
Wait for whiskers