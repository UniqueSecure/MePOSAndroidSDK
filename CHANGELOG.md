MePOS Android SDK release notes
================================

### MePOS 1.25 SDK (November 20, 2019)
- Added the method printRAW(byte\[\]\[\] data, MePOSPrinterCallback callback).

### MePOS 1.24 SDK (October 14, 2019)
- Fixed printRAW(String data) method, will work regarding of the size of the string.

### MePOS 1.23 SDK (September 21, 2018)
- Fixed loadImage(Bitmap image) method.

### MePOS 1.22.2.2 SDK (April 16, 2018)
- Fixed issues with enableUSB(), disableUSB(), enableWifi() and disableWifi() methods.

### MePOS 1.22.2.1 SDK (April 10, 2018)
- Fixed issues with WiFi configuration timing.

### MePOS 1.22.2 SDK (April 4, 2018)
- Fixed several issues with MePOS WiFi configuration and instance creation.

### MePOS 1.22.1 SDK (April 1, 2018)
- MePOSGetAssignedIP returns "0.0.0.0" when the WiFi client configuration failed.

### MePOS 1.22 SDK (March 10, 2018)
- Added full duplex communication with MePOS.

### MePOS 1.21.1 SDK (November 11, 2017)
- Added logging info levels.

### MePOS 1.20.1 SDK (November 6, 2017)
- Added methods to enable/disable the cosmetic LED rotating button.

### MePOS 1.19.2 SDK (October 30, 2017)
- Improved stability when printing and doing cash drawer changes.

### MePOS 1.19.1 SDK (October 13, 2017)
- Improved stability when printing and doing I/O changes such as cosmetic, diagnostic or cash drawer changes while printing a receipt.
- Added: setFeedAfterCut() in the MePOSReceipt class to handle feed lines after a paper cut.

### MePOS 1.18.4 SDK (October 12, 2017)
- Fix of feed line of receipts with partial cut.

### MePOS 1.18.3 SDK (October 6, 2017)
- Added a feed line at the end when printing receipts with a partial cut.

### MePOS 1.18.2 SDK (August 30, 2017)
- (HOTFIX) Fixed an issue related to wifi communication of complex receipts (bmp images printing slow or malfunctioning)

### MePOS 1.18 SDK (July 04, 2017)
- Fixed an issue related to hanging the communications via TCP if there were no MePOS on that IP Address

### MePOS 1.17 SDK (April 25, 2017)
- Added support to cash drawer open on generic esc/pos systems

### MePOS 1.16 SDK (March 30, 2017)
- Added support for generic printers

### MePOS 1.15 SDK (February 22, 2017)
- Improved printing speed on Wifi/Ethernet connectivity

### MePOS 1.14 SDK (February 8, 2017)
- Stability improvements on MePOS USB Hub

### MePOS 1.13 SDK (January 16, 2017)
- Enhanced control of USB hub while interacting with third party accessories

### MePOS 1.12 SDK (January 11, 2017)
- Fixed an issue of losing data while printing several receipts over USB.

### MePOS 1.11 SDK (January 6, 2017)
- Improved speed on USB instances
- Decreased value of timeout in the printer queue to trigger the error on the callback

### MePOS 1.10 SDK (December 15, 2016)
- Gradle integration! Now the sdk supports gradle to integrate easily to your project.
- Added a new method to open cash drawer with a boolean argument to enable/disable validation of cash drawer status before opening.
- Stability improvements on cash drawer methods.

### MePOS 1.9 SDK (November 21, 2016)
- Added a new constructor on MePOSReceiptBarcodeLine: Now it supports personalisation of human-interface readables and height of the barcode.
- Added a new method to control all the diagnostic LEDs.
- Improvements on printing queue (removed delay of the first element of the queue).
- Stability improvements on USB instances.

### MePOS 1.8 SDK (October 17th, 2016)
- Improvements on Network configuration methods to configure mepos as Default, AP, Ethernet Client or WiFi Client.
- Changed implementation method: openCashDrawer(). Now it supports a boolean parameter to return true if is opened or false if the cash drawer was already opened.

### MePOS 1.7 SDK (September 26, 2016)
- Added a cash drawer status method.
- General fixes and stability on USB attach/detach.

### MePOS 1.6 (September 12, 2016)
- Added a printer queue
- Added a print method with an interface parameter (see print(MePOSReceipt receipt, MePOSPrinterCallback callback)). Now MePOS can notify in real-time if a receipt starts printing, completed or handling errors.
- General fixes and stability improvements.

### MePOS 1.5 (September 1, 2016)
- Added a new constructor to allow users multiple MePOS instances.
- Added methods to configure MePOS as Ethernet Client and Default modes.
- Stability improvements.
