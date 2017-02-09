# MePOS Connect Android SDK

The MePOS connect SDK is designed to allow communication from a tablet to the MePOS host unit. SDK libraries are
currently available for Android and Windows.

This document is a reference on how to integrate to the MePOS unit into your own tablet application. This document
does not include information on how to set up the MePOS unit, for this please refer to the documentation that came with
your MePOS unit.

## Contents
- [Supported tablet devices](#supported-tablet-devices)
- [Supported MePOS firmware](#supported-mepos-firmware)
- [Use of the MePOS connect SDK on Android](#use-of-the-mepos-connect-sdk-on-android)
  - [Libraries](#libraries)
  - [Add the SDK to your project](#add-the-sdk-to-your-project)
  - [Creating a new MePOS object](#creating-a-new-mepos-object)
  - [General recommendations on Android](#general-recommendations-on-android)
- [MePOS SDK Methods](#mepos-sdk-methods)
  - [int setDiagnosticLed(int position, int colour)](#int-setdiagnosticledint-position-int-colour)
  - [int setLedOneCol(Integer colour), int setLedTwoCol(Integer colour), int setLedThreeCol(Integer colour)](#int-setledonecolinteger-colour-int-setledtwocolinteger-colour-int-setledthreecolinteger-colour)
  - [int setCosmeticLedCol(Integer colour)](#int-setcosmeticledcolinteger-colour)
  - [boolean printerBusy()](#boolean-printerbusy)
  - [int print(MePOSReceipt receipt)](#int-printmeposreceipt-receipt)
  - [int print(MePOSReceipt receipt, MePOSPrinterCallback callback)]()
  - [int printRAW(String command)](#meposreceipt-r--new-meposreceipt)
  - [int serialRAW(String command)](#int-printrawstring-command)
  - [int cashDrawerStatus() throws MePOSException](#int-cashdrawerstatus-throws-meposexception)
  - [(NEW) boolean openCashDrawer(boolean validateCashDrawerStatus) throws MePOSException](#boolean-opencashdrawer-throws-meposexception)
  - [boolean openCashDrawer() throws MePOSException](#boolean-opencashdrawer-throws-meposexception)
  - [int enableUSB() throws MePOSException](#int-enableusb-throws-meposexception)
  - [int disableUSB() throws MePOSException](#int-disableusb-throws-meposexception)
  - [int enableWifi() throws MePOSException](#int-enablewifi-throws-meposexception)
  - [int disableWifi() throws MePOSException](#int-disablewifi-throws-meposexception)
  - [String getFWVersion()](#string-getfwversion)
  - [String getSerialNumber()](#string-getserialnumber)
  - [MePOSConnectionManager getConnectionManager()](#meposconnectionmanager)
- [MePOSConnectionManager](#meposconnectionmanager)
  - [int getConnectionStatus()](#int-getconnectionstatus)
  - [setConnectionIPAddress(string IPAddress)](#setconnectionipaddressstring-ipaddress)
  - [String getConnectionIPAddress()](#string-meposgetassignedip)
  - [String MePOSGetAssignedIP()](#string-meposgetassignedip)
  - [String getMACAddress()](#string-getmacaddress)
  - [String getSSID()](#string-getssid)
  - [String getRouterFirmware()](#string-getrouterfirmware)
  - [boolean MePOSConnectDefault()](#boolean-meposconnectdefault)
  - [boolean MePOSConnectEthernet(String ipAddress, String netMask)](#boolean-meposconnectethernetstring-ipaddress-string-netmask)
  - [boolean MePOSConnectEthernet()](#boolean-meposconnectethernet)
  - [boolean MePOSConnectWiFi(String SSID, String IPAddress, String netmask, String encryption, String password)](#boolean-meposconnectwifistring-ssid-string-ipaddress-string-netmask-string-encryption-string-password)
  - [boolean MePOSSetAccessPoint(String SSID, String encryption, String password)](#boolean-meposconnectwifistring-ssid-string-ipaddress-string-netmask-string-encryption-string-password)
- [MePOSReceipt](#meposreceipt)
  - [setCutType(int cutType)](#setcuttypeint-cuttype)
  - [MePOSReceiptBarcodeLine(int type, String data)](#meposreceiptbarcodelineint-type-string-data)
  - [MePOSReceiptFeedLine(int lines)](#meposreceiptfeedlineint-lines)
  - [MePOSReceiptImageLine(Bitmap image)](#meposreceiptimagelinebitmap-image)
  - [MePOSReceiptPriceLine(String leftText, int leftStyle, String rightText, int rightStyle)](#meposreceiptpricelinestring-lefttext-int-leftstyle-string-righttext-int-rightstyle)
  - [MePOSReceiptSingleCharLine(char chr)](#meposreceiptsinglecharlinechar-chr)
  - [MePOSReceiptSingleCharLine(char chr)](#meposreceipttextlinestring-text-int-style-int-size-int-position)
  - [MePOSReceiptTextLine(String text, int style, int size, int position)](#meposreceipttextlinestring-text-int-style-int-size-int-position)
  - [Text style constants](#text-style-constants)
  - [Text size constants](#text-style-constants)
  - [Text position constants](#text-style-constants)

## Supported tablet devices

The MePOS connect library supports Android tablets and Windows PC’s via a USB and Wi-Fi connection. Due to the
restrictions of the iOS platform it is not possible to connect to the MePOS unit via USB from an Apple device.
Later releases of the MePOS connect library will introduce libraries for the iOS platform.

## Supported MePOS firmware

The MePOS connect SDK has been tested with the latest MePOS 3.1 firmware.

## Use of the MePOS connect SDK on Android

### Libraries
The MePOS connect SDK for Android currently includes one library.

* meposconnect.aar

  The MePOS connect library is provided for Android as an aar file for use with Android Studio.

### Add the SDK to your project

There are two options:

***1 Manual***

- Download the .aar file from [here](https://github.com/UniqueSecure/MePOSAndroidSDK/tree/master/aars)
- On your project create a new module (Import AAR Package)
- Go to project structure
- Add an app module dependencies on the library
    - On scope select compile

Continue [here](#creating-a-new-mepos-object)


***2 Gradle Integration***

  You can integrate the MePOS connect library using gradle, adding the following configuration to your build.gradle file:

```
repositories {
 maven { url "http://connect.mepos.io/artifactory/libs-release-local" }
}
```

```
dependencies {
 compile 'com.uniquesecure:meposconnect:1.14:@aar'
}
```

#### References

  If the library has been referenced correctly in Android studio, you should be able to add the following reference to your project without receiving errors:

**import com.uniquesecure.meposconnect.*;**

### Creating a new MePOS object
  To instantiate a new class to communicate with the MePOS unit, you will need to add the following code to your application:


##### MePOS mePOS = new MePOS(context);


  The above code has now instantiated a MePOS object, the required context parameter is needed for the MePOS class to gain access to the USB device.
  It is also needed to dispose the instance when the MePOS is disconnected from USB. To achieve this, you will need to add an IntentFilter and a BroadcastReceiver:

```
IntentFilter intentFilter = new IntentFilter();
intentFilter.addAction("android.hardware.usb.action.USB_DEVICE_ATTACHED");
intentFilter.addAction("android.hardware.usb.action.USB_DEVICE_DETACHED");
BroadcastReceiver broadcastReceiver = new BroadcastReceiver() {

 @Override
 public void onReceive(Context context, Intent intent) {
 if (intent.getAction().contains("ATTACHED")) {
 myMePOSInstance = new MePOS(context, MePOSConnectionType.USB);
 Toast.makeText(MyActivity.this, "MePOS connected", Toast.LENGTH_SHORT).show();
 } else if (intent.getAction().contains("DETACHED")){
 myMePOSInstance.getConnectionManager().disconnect();
 Toast.makeText(MyActivity.this, "MePOS disconnected", Toast.LENGTH_SHORT).show();
 }
}
getApplicationContext().registerReceiver(broadcastReceiver, intentFilter);
```

If there are multiple devices to access, each one will be controlled with an Instance of MePOS, but it is needed
to use the following constructor:

##### MePOS mePOS = new MePOS(context, MePOSConnectionType.USB);
##### MePOS mePOS = new MePOS(context, MePOSConnectionType.WIFI);

### General recommendations on Android
- While using another devices connected in the MePOS USB HUB, make sure no other app has been set up as default while connecting the external usb devices in the android Launcher. See [this post for more info] (http://android.stackexchange.com/questions/148161/how-to-set-home-launcher-in-android-7-0-nougat).
- Please be noticed that the acknowledgement of USB permissions are not available while the tablet is on sleep mode. Make sure the MePOS instances over USB are created when the tablet is running in foreground.


### MePOS SDK Methods
Once a MePOS object has been created there are several methods that can be executed that perform actions on
the MePOS unit. The method names, syntax and usage are identical across the Android platform. Note that
some of them are not recommended to execute on the main Thread.

### int setDiagnosticLed(int position, int colour)

  Will set the diagnostic LED indicated. The positions are defined in ***MePOSDiagnosticLEDS*** class, as the colors in ***MePOSColorCodes.***

### int setLedOneCol(Integer colour), int setLedTwoCol(Integer colour), int setLedThreeCol(Integer colour)

  (Deprecated: use setDianosticLed instead)
  Will set one of the three diagnostic LED's on the MePOS unit to one of the following colours:

  - MePOSColorCodes.LED_OFF (0)

  - MePOSColorCodes .LED_GREEN (1)

  - MePOSColorCodes.LED_RED (2)

  - MePOSColorCodes.LED_AMBER (3)

  ***Note:***  Between the development version and early versions of the MePOS unit the colours 1 and 2 (Green and
Red are swapped).

### int setCosmeticLedCol(Integer colour)
Will set the MePOS cosmetic LED to one of the following colours:
  - MePOSColorCodes.COSMETIC_OFF

  - MePOSColorCodes.COSMETIC_BLUE

  - MePOSColorCodes.COSMETIC_GREEN

  - MePOSColorCodes.COSMETIC_CYAN

  - MePOSColorCodes.COSMETIC_RED

  - MePOSColorCodes.COSMETIC_MAGENTA

  - MePOSColorCodes.COSMETIC_YELLOW

  - MePOSColorCodes.COSMETIC_WHITE

### boolean printerBusy()

  Print commands are sent to the printer asynchronously, print or printRAW will return immediately to prevent locking the UI thread on a tablet device. To control the tablet UI and prevent possible print buffer overflow it is possible to monitor the printer busy status. New receipts cannot be printed unless the printerBusy method returns false.

### int print(MePOSReceipt receipt)

  Prints a pre-defined MePOS receipt using the built in receipt printer. To print a receipt, you must first create a MePOS receipt and add lines to it using the add command. This method will return 0 if the receipt was queued, or 1 otherwise. This method integrates by default a printer queue.

  The below example prints a single line receipt:

### MePOSReceipt r = new MePOSReceipt();

  *** r.addLine(new MePOSReceiptTextLine(“Hello World”, MePOS.TEXT_STYLE_BOLD, MePOS.TEXT_SIZE_WIDE, MePOS.TEXT_POSITION_CENTER)); ***  

### int success = mePOS.print(r);


### int print(MePOSReceipt receipt, MePOSPrinterCallback callback);

  Prints a pre-defined MePOS receipt using the built in receipt printer. When MePOS starts or finishes a receipt, it will call onPrinterStarted(), onPrinterCompleted() or onPrinterError(). This method also integrates by default a printer queue.

### receipt.addLine(new MePOSReceiptImageLine(getBitmapFromAsset(context, "ic_launcher.bmp")));
 Prints a bmp image. The size should be 576 x 200 pixel

### int printRAW(String command)

  The raw print command allows the user to send ESC POS commands directly to the printer without using the receipt builder. This function is useful if your epos system already prints using the ESC POS command set. This method will return 0 for success, 1 if no MePOS is connected or 2 if the printer is busy.

### int serialRAW(String command)

  The serial raw command allows the user to send commands directly to the DE9 port. This method will return 0 for success or 1 if an  error happened.

### int cashDrawerStatus() throws MePOSException

  This command will return the status of the Cash Drawer. Possible values are:

  - MePOS.CASH_DRAWER_STATUS_OPENED (0)
  - MePOS.CASH_DRAWER_STATUS_CLOSED(1).

  *** (NEW) boolean openCashDrawer(boolean validateCashDrawerStatus) throws MePOSException ***

  If there is any cash drawer connected to the MePOS, this command will try to open it. The method returns true if opens or false if it was already opened. If you need to avoid validation of cash drawer status and send the command directly, use ** validateCashDrawerStatus ** as ** false. **

### boolean openCashDrawer() throws MePOSException

  - Same as openCashDrawer(true).

### int enableUSB() throws MePOSException

  - Enables the USB ports on the MePOS device.

### int disableUSB() throws MePOSException

  - Disables the USB ports on the MePOS device.

### int enableWifi() throws MePOSException

  - Enables the Wifi module on the MePOS device.

### int disableWifi() throws MePOSException

  - Disables the Wifi module on the MePOS device.

### String getFWVersion()
  - Gets the characteristics of the firmware version as Doc number, article code, revision and date of the firmware.

### String getSerialNumber()
  - Gets the serial number of the MePOS.

### MePOSConnectionManager getConnectionManager()
  - Gets the MePOSConnectionManager for the current MePOS instance. The connection manager can be used to set up the Wi-Fi network on the MePOS unit and query the connection state of the MePOS unit.

### MePOSConnectionManager

  The MePOSConnectionManager can be used to configure the connection settings for the MePOS unit and to configure the Wi-Fi module on a MePOS unit. The methods of this interface will not respond immediately, and is encouraged to the user to execute it asynchronously.

### int getConnectionStatus()
  Gets the current connection state of the MePOS unit:

  -1 = Not initialised

  0 = Not connected

  1 = Connected

### setConnectionIPAddress(string IPAddress)

  Sets the IP address on which the connection manager will look for a MePOS unit. The default IP address of the MePOS from the factory is 192.168.16.254, if the tablet is connecting to the MePOS as a client then you will not need to change this parameter unless the network settings on the MePOS unit have been changed.

### String getConnectionIPAddress()

  Gets the current IP address setting for the connection manager.

### String MePOSGetAssignedIP()

  Gets the current IP address from the connected MePOS unit. When connected to Wi-Fi this will be either the statically provided IP address or the DHCP network assigned IP address. In access point mode this will be the default IP address of 192.168.16.254.

### String getMACAddress()

  Gets the Mac Address of the MePOS unit.

### String getSSID()

  Gets the current SSID of the MePOS unit if is configured as WiFi Client or Access Point.

### String getRouterFirmware()
  Gets the actual Router Firmware of the MePOS unit.

### boolean MePOSConnectDefault()

  Sets the MePOS unit as factory settings.

### boolean MePOSConnectEthernet(String ipAddress, String netMask)

  Set the MePOS unit to a network as a client using an Ethernet connection.

  Valid input parameters are:

  - IPAddress – A valid static IP Address e.g. 192.168.0.100 or DHCP
  - ****(MePOSConnectionManager.IP_AS_DHCP)**** to request a DHCP address from the network.
  - netMask – A valid network mask e.g. 255.255.255.0, if the IPAddress parameter was DHCP this parameter will be ignored.

### boolean MePOSConnectEthernet()

  Set the MePOS unit to a network as a client using an Ethernet connection and as DHCP. Same as MePOSConnectEthernet(“DHCP”, null)

### boolean MePOSConnectWiFi(String SSID, String IPAddress, String netmask, String encryption, String password)

  Connects the MePOS unit to a WiFi network as a client. After performing a Wi-Fi connection, the setConnectionIPAddress method must be called with the provided static IP address or the assigned DHCP IP address using the MePOSGetAssignedIP() method. If the MePOS unit is being used as an access point, connecting to a Wi-Fi network will switch the MePOS to becoming a Wi-Fi client and the MePOS unit will no longer be a Wi-Fi access point. It is only possible to configure the WiFi module when the MePOS unit is plugged in via USB, a call to this method will return false if no USB connection is found.

  Valid input parameters are:

 ***SSID*** – The SSID of the Wi-Fi network you are connecting to.

 ***IPAddress*** – A valid static IP Address e.g. 192.168.0.100 or DHCP

 *** (MePOSConnectionManager.IP_AS_DHCP) *** to request a DHCP address from the network.

 Netmask – A valid network mask e.g. 255.255.255.0, if the IPAddress parameter was DHCP this parameter will be ignored.

 Encryption – The encryption type of the network you are connecting to, one of the following values NONE, WEP, WEP_OPEN, WPA_TKIP, WPA_AES, WPA2_TKIP, WPA2_AES, WPAWPA2_TKIP, WPAWPA2_AES. The Encryption modes are defined as constants in the enum: ***EncryptionMode.***

 Password - The password for the network you are connecting to. This can be left blank or will be ignored if the encryption type was set to NONE.

 ***boolean MePOSSetAccessPoint(String SSID, String encryption, String password)***

 Sets the MePOS unit in to access point mode. When entering access point mode, the MePOS unit will create its own Wi-Fi network with the SSID, encryption and password provided. In access point mode the MePOS unit will create the IP network 192.168.16.0 and will get the IP address 192.168.16.254. Clients connecting to the
MePOS unit will be automatically assigned IP addresses via DHCP. If the MePOS unit is connected to a Wi-Fi network as a client, setting the MePOS unit in access point mode will remove the MePOS unit from any Wi-Fi networks it is connected to in client mode. It is only possible to configure the WiFi module when the MePOS
unit is plugged in via USB, a call to this method will return false if no USB connection is found.

  SSID – The SSID of the Wi-Fi network you are creating.

  Encryption – The encryption type of the network you are creating, one of the following values NONE, WEP, WEP_OPEN, WPA_TKIP, WPA_AES, WPA2_TKIP, WPA2_AES, WPAWPA2_TKIP, WPAWPA2_AES.
  The Encryption modes are defined as constants in the enum: EncryptionMode.

  Password - The password for the network you are creating. This can be left blank or will be ignored if the encryption type was set to NONE

### MePOSReceipt

  The MePOS library also contains the classes to define and print a receipt using the receipt printer.

  To create a new receipt, use the following code:

  ***MePOSReceipt r = new MePOSReceipt();***

  After the receipt has been initialised you can add lines to the receipt for printing. There are currently six types of line available to print on a receipt, these are detailed below.

### setCutType(int cutType)

  Specifies whether to perform a full or partial cut at the end of the receipt where the cut type is:
  - MePOS.CUT_TYPE_FULL
  - MePOS.CUT_TYPE_PARTIAL

  Once the receipt has been created you can modify how the printer will cut the receipt after it has finished printing. As a default the printer is set to perform a full receipt cut.

### MePOSReceiptBarcodeLine(int type, String data)
  The barcode line can be used to add a barcode to a receipt. There are currently three supported barcode types, UPC-A, Code 39 and PDF417. They are specified using the following constants:

- MePOS.BARCODE_TYPE_UPCA
- MePOS.BARCODE_TYPE_CODE39
- MePOS.BARCODE_TYPE_PDF417

  The following example shows how to add a barcode to a receipt:
### MePOSReceipt r = new MePOSReceipt();
### r.AddLine(new MePOSReceiptBarcodeLine(MePOS.BARCODE_TYPE_PDF417, “Hello World!”);

  By default, the Barcode prints a human-interface readable string below, and a defined height of around 0.35 inches. If you want to customise the height or the hri, please use this constructor instead:
### new MePOSReceiptBarcodeLine(MePOS.BARCODE_TYPE_CODE39, MePOS.BARCODE_HRI_NONE, 0.50, “Hello World!”);
  Please note that the height is *** not customisable *** on *** PDF 417 *** barcodes.

### MePOSReceiptFeedLine(int lines)

  The feed line can be used to add whitespace to a receipt. The parameter supplied is the number of lines to feed.

  The following example shows how to feed 10 lines on a receipt:
  ***MePOSReceipt r = new MePOSReceipt();***
  ***r.AddLine(new MePOSReceiptFeedLine(10);***

### MePOSReceiptImageLine(Bitmap image)
  The image line can be used to print black and white raster graphics to the printer. The bitmap provided must be a valid Android.graphics.Bitmap for Anroid or System.Drawing.Bitmap for Windows.

  The following example shows how to add an image to a receipt:
  ***MePOSReceipt r = new MePOSReceipt();***
  ***r.AddLine(new MePOSReceiptImageLine(bitmap);***

### MePOSReceiptPriceLine(String leftText, int leftStyle, String rightText, int rightStyle)

  The price line can be used to add a line to a receipt with text on the left and right simulating the common price item layout of many receipts. The price line takes parameters defining the text on the left and right and also the style of the text. Text styles are discussed later in this document.

  The following example shows how to add a price line to a receipt:
  ***MePOSReceipt r = new MePOSReceipt();***
  ***r.AddLine(new MePOSReceiptPriceLine(“Some Item”, MePOS.TEXT_STYLE_NONE, “Some Price”, MePOS.TEXT_STYLE_NONE);***

### MePOSReceiptSingleCharLine(char chr)
  The single character line can be used to fill a single line with the same character. The parameter is the character to repeat across the whole line.

  The following example shows how to add a single character line to a receipt:

  ***MePOSReceipt r = new MePOSReceipt();***
  ***.AddLine(new MePOSReceiptSingleCharLine(‘.’);***

### MePOSReceiptTextLine(String text, int style, int size, int position)

  The text line can be used to put text on a receipt on the left centre or right in different sizes or styles. The first parameter is the text to print, the size and position constants are discussed later in this document.

  The following example shows how to add a text line to a receipt:
  ***MePOSReceipt r = new MePOSReceipt();***
  ***r.AddLine(new MePOSReceiptTextLine(“Hello World!”, MePOS.TEXT_STYLE_NONE, MePOS.TEXT_SIZE_NORMAL, MePOS.TEXT_POSITION_CENTER);***

### Text style constants
  Some of the printer line commands accept a style parameter. This parameter can be any one of the following constants:
- MePOS.TEXT_STYLE_NONE
- MePOS.TEXT_STYLE_BOLD
- MePOS.TEXT_STYLE_ITALIC
- MePOS.TEXT_STYLE_UNDERLINED
- MePOS.TEXT_STYLE_INVERSE

  Text styles can also be combined using the "or" operator to achieve a mix of styles, for example to print bold italic text you would use the following:
- MePOS.TEXT_STYLE_BOLD | MePOS.TEXT_STYLE_ITALIC

### Text size constants

  Some of the printer line commands accept a size constant. This can be one of the following but not both:
- MePOS.TEXT_SIZE_NORMAL
- MePOS.TEXT_SIZE_WIDE

### Text position constants

  Some of the printer line commands accept a position constant. This can be any of the following:
- MePOS.TEXT_POSITION_LEFT
- MePOS.TEXT_POSITION_CENTER
- MePOS.TEXT_POSITION_RIGHT

## Sample Codes
- [MePOS print WIFI/USB](https://github.com/UniqueSecure/MePOS-Print-button-WIFI-USB)
- [MePOS print WIFI/USB With decorative LEDs](https://github.com/UniqueSecure/MePOS-Print-button-WIFI-USB-Decorative-LED)
- [MePOS WIFI modes](https://github.com/UniqueSecure/WifiModesExample)
