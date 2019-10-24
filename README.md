# Moddable WebIDE Workshop

This document explains how to run JavaScript examples on the Moddable One and Moddable Two from a browser-based interface (referred to as the WebIDE in this document). The instructions here assume the Moddable Workshop host application is already installed on the development board.

## Table of Contents

* [About the WebIDE](#webide)
* [Host Application](#host-app)
* [Examples](#examples)
* [Running Examples](#running-examples)
* [Additional Resources](#resources)
* [Troubleshooting](#troubleshooting)

<a id="webide"></a>
## About the WebIDE

The WebIDE is a development tool to edit, build, install, run, and debug JavaScript code for microcontrollers using the Moddable SDK.  It allows you to install JavaScript applications built with the Moddable SDK on your device from the browser without having to install any additional software or drivers. It is an alternative to some of the command line build tools in the Moddable SDK.

The WebIDE is available at [ide.moddable.com](https://ide.moddable.com).

![](./images/webide.png)

The WebIDE connects can connect to devices using both USB and WebSockets. To connect over USB, it uses [WebUSB](https://developers.google.com/web/updates/2016/03/access-usb-devices-on-the-web). At this time, WebUSB is only implemented in Chrome. If you don't have Chrome already, you can download it [here](https://www.google.com/chrome/).

> The repository for the main project is [on GitHub](https://github.com/FWeinb/moddable-webide). Special thanks to [FWeinb](https://github.com/FWeinb) for developing it. The version used for this workshop is also [on GitHub](https://github.com/lprader/moddable-webide).
	
<a id="host-app"></a>
## Host Application

When you power on your device, the device will connect to the `moddable-guest` network. Make sure you are connected to the same network.

On your device's screen, you'll see a blue background and a random name like in the image below. **Remember this name.** You will need it if you want to install examples using WebSockets, and it will no longer appear on screen after you install an example.

![](./images/homescreen.png)

<a id="examples"></a>
## Examples

Several examples are provided to get you started. 

- **Hello, world**: draws the string "Hello, world" on screen
- **Drag**: draws four buttons that you can drag around the screen
- **Button**: Updates the screen when you press the on-board Flash button
- **Blink**: Blinks the on-board LED
- **Weather**: Traces weather data from a cloud service to the console

To open an example in the WebIDE, take the following steps.

1. Go to the Projects view in the WebIDE and click **Import GitHub Gist**.

	<img src="./images/projects.png" width=200>

3. Enter a name for the project.

	<img src="./images/input-name.png" width=450>
	
4. Enter the gist ID for the example you want to run.

	- Hello, world: `598fa08d89986872ae5bef048a18f0a4`
	- Drag: `49e5ea10301921f975e861779e6cbf30`
	- Button: `fffa26dfb77d4b2f5bc7fa246a00ac06`
	- Blink: `289ffe2a7b87201e4760a744e2921b11`
	- Weather: `13d3bf789866582640c4d937015e357a`
	
	<img src="./images/input-gist-id.png" width=450>

<a id="running-examples"></a>
## Running Examples

You can install examples using WebUSB or WebSockets. Instructions for both options are provided in the sections below.

### WebUSB Instructions

To install an example using WebUSB, take the following steps.

1. In the top right corner, select **USB** and the type of board you're using from the drop-down menus. If you're using a Moddable One, select **ESP8266**. If you're using a Moddable Two, select **ESP32**.

	<img src="./images/dropdown.png" width=350>
	
2. Click the **Flash** button to install the application.

	<img src="./images/flash.png" width=350>

	> If you see `NetworkError: Unable to claim interface` traced to the console, see the [troubleshooting section](#vcp-driver).
	
3. Your device will show up as a **CP2104 USB to UART Bridge Controller**. Select the device and click the **Connect** button to connect to it.

	<img src="./images/select-device.png" width=500>

	> If the popup says `No compatible devices found` see the [troubleshooting section](#vcp-driver).
	
4. If the installation is successful, you will see the following messages traced to the Log at the bottom of the WebIDE.

	![](./images/success.png)
	
	> If you see `File exists` traced to the console, see the [troubleshooting section](#file-exists).
	
### WebSockets Instructions

To install an example using WebSockets, take the following steps.

1. In the top right corner, select **Wi-Fi** from the drop-down menus.

	<img src="./images/dropdown-wifi.png" width=350>
	
2. Enter the hostname of your device. The hostname is the name you screen on the main screen of your device with `.local` added to the end, for example `thing2aa218.local`.

	<img src="./images/hostname.png" width=350>
	
3. Click the **Flash** button to install the application.

	<img src="./images/flash-websockets.png" width=350>
	
4. If the installation is successful, you will see the following messages traced to the Log at the bottom of the WebIDE.

	![](./images/success.png)
	
	> If you see `File exists` traced to the console, see the [troubleshooting section](#file-exists).
	
	> If you never see `Uploading...` traced to the console, see the [troubleshooting section](#no-upload).

<a id="resources"></a>
## Additional Resources
Now that you are able to build, install, and run the examples, you are ready to start writing your own code. You might choose to tweak the examples or write your own examples froms cratch.

The [Moddable SDK](https://github.com/Moddable-OpenSource/moddable) includes many developer resources. The readme at the root of the repository contains an overview of what's there.

The following resources are particularly useful for learning to edit the examples provided for this workshop:
 
- The [Piu user interface framework](https://github.com/Moddable-OpenSource/moddable/blob/public/documentation/piu/piu.md) is an object-based framework that makes it easier to create complex, responsive layouts. The examples from this workshop are all built using Piu.

- The [Pins documentation](https://github.com/Moddable-OpenSource/moddable/blob/public/documentation/pins/pins.md) describes the APIs used for various hardware protocols, including all of the ones used in examples from this workshop.

- There are [150+ example apps]((https://github.com/Moddable-OpenSource/moddable/tree/public/examples)) and [extensive documentation](https://github.com/Moddable-OpenSource/moddable/tree/public/documentation) of all the features of the Moddable SDK.

<a id="troubleshooting"></a>
## Troubleshooting

This section lists common issues and how to resolve them.

<a id="vcp-driver"></a>
### `NetworkError: Unable to claim interface` or `No compatible devices found`

If you have the [Silicon Labs VCP Driver](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers) installed, you will get the following error when you try to install an application on your device from the WebIDE:

```
NetworkError: Unable to claim interface.
** Looks like you need to uninstall the driver **
```

You can uninstall the driver by running the uninstall script provided by Silicon Labs. You may have to restart your computer after uninstalling for the change to take effect. Alternatively, you can disable it while you use the WebIDE, then re-enable it when you're done. The next sections explain how to do this on macOS, Windows, and Chrome OS.

#### macOS

To disable the driver:

```
sudo kextunload -b com.silabs.driver.CP210xVCPDriver
```

To bring the driver back:

```
sudo kextload -b com.silabs.driver.CP210xVCPDriver
```

#### Windows

1. Install and open [Zadig](https://zadig.akeo.ie/)

2. Plug in your device via USB

4. Select **Options->List All Devices**

	![](./assets/step1.png)

5. Select **CP2102 USB to UART Bridge Controller** from the drop down menu

	![](./assets/step2.png)

6. Select **WinUSB** from the Driver drop down menu and click the **Replace Driver** button

	![](./assets/step3.png)

7. ***IMPORTANT!*** Restart your computer after the driver is installed

#### Chrome OS

1. Put your Chromebook into developer mode 

	The steps for this may vary for each model. I had success following [these steps](https://www.howtogeek.com/210817/how-to-enable-developer-mode-on-your-chromebook/).

2. Open Crosh by pressing **Ctrl+Alt+T**

3. Open the developer shell by entering the `shell` command

4. Unload the CP210X module

	```
	sudo rmmod cp210x
	```

<a id="file-exists"></a>
### `File exists`

If you see the message `File exists` when you try to install an application, simply refresh the page and try again. You won't lose any saved changes to your example apps.

<a id="no-upload"></a>
### `Uploading...` message never shows in console

If you never see the `Uploading...` message traced to the console, open the JavaScript console in Chrome.

<img src="./images/chrome-console.png" width=500>

You may see a Mixed Content error like in the following image. 
<img src="./images/mixed-content.png">

If so, take the following steps:

1. Click the Insecure Content warning icon in the URL bar.

	<img src="./images/insecure-content-warning.png">

2. Click "Load Unsafe Scripts" in the popup to give the site permission to connect to the device.

	<img src="./images/load-unsafe-scripts.png">

You may see an ERR\_NAME\_NOT\_RESOLVED error like in the following image.
<img src="./images/name-not-resolved.png">

If so, make sure you entered the correct hostname in the WebIDE and your device is connected to the same Wi-Fi network as your laptop.