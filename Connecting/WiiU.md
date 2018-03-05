# Connecting with the WiiU

### NOTE: PRETENDO IS CURRENTLY NOT READY FOR CASUAL USERS. ONLY DEVELOPERS AND OFFICIAL TESTERS SHOULD BE CONNECTING AT THIS STAGE OF DEVELOPMENT

## Pretendo is currently working on a custom proxy called [Maryo](https://github.com/PretendoNetwork/maryo). Once it is complete, this guide will be deprecated

### Prerequisites:
- [Fiddler](https://www.telerik.com/fiddler) (or any other proxy software, though this guide is specific to Fiddler)
- Homebrew on either your [WiiU](https://wiiu.hacks.guide)
- Ensure both your console and PC are connected to the same internet connection

### Setup (Computer):
1. In Fiddler, open `Tools > Options`
2. In the `HTTPS` tab turn on HTTPS Connects
3. Enable HTTPS decrypting
4. Ignore server certificate errors
5. In the `Connections` tab tick `Allow remote computers to connect`
6. Turn off `Act as system proxy on startup`
7. Back in the `HTTPS` tab click `Actions > Export Root Certificate to Desktop`
8. Rename `FiddlerRoot.cer` to `CACERT_NINTENDO_CA_G3.der` wherever you exported it to. NOTE: MAKE SURE TO CHECK THE FILE EXTENSION. IT SHOULD BE `.der` NOT `.cer`
9. Go complete on of the console sections
10. Open the `FiddlerScript` section of the Fiddler UI and find the `OnBeforeRequest` method
11. At the end of the method, add:
```
// Change "account.nintendo.net" to whatever official Nintendo server you are replacing
if (oSession.HostnameIs("account.nintendo.net"))
{
    if (oSession.HTTPMethodIs("CONNECT"))
    {
        oSession["x-replywithtunnel"] = "PretendoTunnel";
        return;
    }

    oSession.fullUrl = "https://account.pretendo.cc" + oSession.PathAndQuery;
}
```
12. Restart Fiddler to apply changes

### Setup (WiiU):
Quick note, you will need Mocha installed on your WiiU. If you followed the guide all the way through, you will have it already.
1. Install FTPiiU_Everywhere (NOT THE REGULAR FTPiiU!)
2. Launch in to Mocha (redNAND or sysNAND, doesn't matter. Do which ever you want)
3. Launch the Homebrew Launcher and startup FTPiiU_Everywhere
4. Connect to the FTP server on your computer using any FTP client (FileZilla/WinSCP on Windows or Finder on MacOS for example)
5. Navigate to `/vol/storage_mlc01/sys/title/0005001b/10054000/content`
6. Enter `scerts` and replace the `CACERT_NINTENDO_CA_G3.der` file there with the `CACERT_NINTENDO_CA_G3.der` file we just made earlier from the FiddlerRoot.cer. AGAIN MAKE SURE TO CHECK THE FILE EXTENSION, IT MUST BE `.der`
7. Close the FTP connection on your computer and exit FTPiiU_Everywhere (press Home button)
8. Reboot your console (DO NOT FORCE REBOOT (holding power button for 4 seconds)! REBOOT NORMALLY (only hold for 2 seconds)! FORCE REBOOTING CAN SOMETIMES ERASE CHANGES!)
9. Go to the connection settings for your WiFi connection on your WiiU and turn on proxy connections
10. Set the proxy server to your PC's IP, and the port to `8888` (unless you changed the Fiddler port)
11. Congratulations! You have now connected to Pretendo