# ![Icon](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_AppIcon.png) LiveboxMonitor

[LiveboxMonitor](https://github.com/p-dor/LiveboxMonitor) is a dynamic graphical interface that lets you:
- Monitor devices connecting to the Livebox and quickly detect intrusions.
- Record an activity log and receive email notifications when devices connect or disconnect.
- Get detailed traffic statistics for individual devices or the network as a whole.
- View traffic statistics graphs covering multiple days, by device or interface.
- Get extensive information about the Livebox itself and monitor the quality of its fibre connection.
- View detailed information about connected devices, whether active or inactive.
- Read the event log for a specific device.
- Fine-tune the DHCP server, NAT/PAT rules, DynDNS, DMZ, and routing table.
- Configure the Wi-Fi network and monitor its overall status.
- Monitor telephone calls and the contact list.
- Monitor one or more connected Orange Wi-Fi repeaters.
- Monitor one or more connected Orange TV decoders.

**WARNING:** Because their architectures are completely different, **this software is not compatible with the "Livebox Pro 4"**.

The application is dynamic because it reacts to and interprets events sent by the Livebox.

![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_DeviceList.png)


## Documentation contents
1. [Installation](#install)
2. [Minimum requirements](#minimalconfig)
3. [About the author](#author)
4. [Discussion](#discussion)
5. [Support the project](#support)
6. [Getting started](#handling)
7. [Profiles](#profiles)
8. [Command-line options](#commandline)
9. [Remote access](#remoteaccess)
10. [Configuration](#configuration)
11. [Linux](#linux)
12. [Devices — Connected device list](#devicelist)
13. [Livebox stats/info — Traffic statistics and advanced Livebox information](#infos)
14. [Graphs — Traffic graphs by interface and device](#graphs)
15. [Device info — Detailed information about each known device](#deviceinfos)
16. [Events — Events received for each known device](#events)
17. [DHCP — Fine-grained DHCP server control](#dhcp)
18. [NAT/PAT — Port and protocol forwarding rules](#natpat)
19. [Phone — Call history and contacts](#phone)
20. [Actions — Action and control buttons](#actions)
21. [Wi-Fi repeater tabs](#repeaters)
22. [TV decoder tabs](#tvdecoders)
23. [Managing and customizing device icons](#icons)
24. [Using Livebox APIs in your own Python program](#codingguide)


## Installation <a id="install"></a>

The application is written in [Python 3.11](https://www.python.org/downloads/). It uses [PyQt6](https://pypi.org/project/PyQt6/) for its graphical interface and [PyQtGraph](https://www.pyqtgraph.org/) for statistical graphs.

The other dependencies are `requests`, `cryptography`, `wakepy`, `xmltodict`, and `python-dateutil`.

**Note:** The `LmSession` module adapts the [sysbus](https://github.com/rene-d/sysbus) package for Livebox 5, 6, and 7. Event support has also been added.

### Download — version 1.7 (26/04/2026)

Standalone programs built with [PyInstaller](https://pyinstaller.org) are available for Windows and macOS:
- Windows: [Download](https://github.com/p-dor/LiveboxMonitor/releases/download/1.7/LiveboxMonitor.exe)
- Windows with console: [Download](https://github.com/p-dor/LiveboxMonitor/releases/download/1.7/LiveboxMonitor_Console.exe)
- macOS (Intel): [Download](https://github.com/p-dor/LiveboxMonitor/releases/download/1.7/LiveboxMonitor.dmg)
- macOS (Intel) with console: [Download](https://github.com/p-dor/LiveboxMonitor/releases/download/1.7/LiveboxMonitor_Console.dmg)
- macOS (Apple silicon): [Download](https://github.com/p-dor/LiveboxMonitor/releases/download/1.7/LiveboxMonitor_Silicon.dmg)
- macOS (Apple silicon) with console: [Download](https://github.com/p-dor/LiveboxMonitor/releases/download/1.7/LiveboxMonitor_Silicon_Console.dmg)

What's new in this version and the release history: [here](https://github.com/p-dor/LiveboxMonitor/blob/main/docs/ReleaseHistory.md) or [here](https://github.com/p-dor/LiveboxMonitor/releases).


### PyPI — [here](https://pypi.org/project/LiveboxMonitor/)

Installation:
```
    pip install LiveboxMonitor
```

Upgrade:
```
    pip install --upgrade LiveboxMonitor
```

Launch:
```
    LiveboxMonitor
```


### Running directly from source

Installation:
```
    git clone https://github.com/p-dor/LiveboxMonitor.git
    cd LiveboxMonitor
    pip install -r requirements.txt
```

Launch:
```
    cd src\LiveboxMonitor
    python3 lbm.py
```

Or use a virtual environment.

Installation:
```
    git clone https://github.com/p-dor/LiveboxMonitor.git
    cd LiveboxMonitor
    virtualenv -p python3 .venv
    source .venv/bin/activate
    pip install -r requirements.txt
```

Launch:
```
    cd src\LiveboxMonitor
    source .venv/bin/activate
    python3 lbm.py
```


## Minimum requirements <a id="minimalconfig"></a>

Based on the documented requirements for [Python 3.11](https://www.python.org/downloads/release/python-3110/), [PyInstaller](https://pyinstaller.org/en/stable/requirements.html), and [PyQt6](https://www.qt.io/product/qt6/technical-specifications), the minimum requirements for running this application are:
- Windows: **Windows 10** or later.
- macOS: **macOS 13 (Ventura)** or later.


## About the author <a id="author"></a>

The author of this software is an IT professional with no connection to Orange. This software is therefore in no way an Orange product.
Because no official documentation for the Livebox APIs is available, the software was developed using conventional reverse-engineering techniques, and the author has worked on the entire project in their spare time.


## Discussion <a id="discussion"></a>

If you have a GitHub account, the [discussion forum](https://github.com/p-dor/LiveboxMonitor/discussions) is active.
A [discussion thread](https://lafibre.info/orange-les-news/controler-son-reseau-livebox-5-ou-6/) is also active on the [lafibre.info](https://lafibre.info) forum. You can post comments, test results, or feature requests there.


## Support the project <a id="support"></a>

You can [make a donation](https://www.paypal.com/donate/?hosted_button_id=U7CLQM3WGTUEJ&locale.x=fr_FR) to encourage the author to continue developing the project.
[![](https://www.paypalobjects.com/fr_FR/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/donate/?hosted_button_id=U7CLQM3WGTUEJ&locale.x=fr_FR)


## Getting started <a id="handling"></a>

The interface is designed to be intuitive, but it is worth consulting the documentation to understand some of its behaviours and displays.
Here are the important points to understand before getting started:
- Connect using the same credentials you use to access the Livebox web configuration interface. For the URL, try `http://livebox.home/`, `http://livebox/`, or `http://192.168.1.1/`. Leave the username at its default value, `admin`. For the password, use either the one you set yourself or the original password. More information is available [here for Livebox 5](https://assistance.orange.fr/livebox-modem/toutes-les-livebox-et-modems/installer-et-utiliser/piloter-et-parametrer-votre-materiel/l-interface-de-configuration/livebox-5-acceder-a-l-interface-de-configuration_292471-827404) and [here for Livebox 6](https://assistance.orange.fr/livebox-modem/toutes-les-livebox-et-modems/installer-et-utiliser/piloter-et-parametrer-votre-materiel/l-interface-de-configuration/livebox-6-acceder-a-l-interface-de-configuration_363963-897414).
- When you first launch the application, it is normal for all devices to be marked as unknown (**UNKNOWN**, in red). One purpose of the program is to quickly identify unknown devices connected to the network using a local name database (the `MacAddrTable.txt` file). Start by naming each device you consider legitimate using the `Assign Name...` button on the `Device Info` tab. This local database becomes the trusted reference for legitimate devices on your network. The `Assign Names...` button on the `Devices` tab can also assign to all your devices the same names that the Livebox assigned to them automatically.
- It is normal for network statistics to appear and disappear. By default, the program refreshes these statistics every 3 seconds (or every 30 seconds in some cases). If no transfer occurs between refreshes, the cell is left blank. This makes it easier to see non-empty cells, where activity is occurring.
- You can drag tabs with the mouse to arrange them in your preferred order. The order is restored the next time you launch the program.
- All columns in the program can be resized with the mouse, except for certain columns that expand dynamically according to the window size. Depending on the situation, you can resize either the column or the window to adjust the column width.
- You can copy the value of any cell in a list to the clipboard: click the cell and press Ctrl+C.
- All lists in the program, including lists in dialogs, can be exported to a CSV file that can be opened in any spreadsheet application (such as Excel). Right-click the list, select `Export...`, and choose the options and columns to export.
- A status bar appears at the bottom of the window. It shows ongoing tasks on the left and the current profile name on the right (see **Profiles** below). Clicking the profile name opens the dialog for changing it.
- **Tooltips** are available throughout the interface to help you use the application without consulting the documentation.

**Caution about traffic statistics:** Do not take the displayed network throughput figures (per device or interface) too literally. This is probably why Orange stopped exposing them. If you see inconsistencies, they are not necessarily a software bug: the program simply displays the data returned by the Livebox, and unfortunately there is nothing it can do to correct it. Differences from reality seem to depend on the manufacturer of the measured device. For example, throughput figures for iOS devices (iPhone, iPad, etc.) seem exaggerated, while for some other devices they appear to match reality quite well.


## Profiles <a id="profiles"></a>

The program supports managing several Liveboxes using separate profiles. Each profile must have a unique name. A main profile is created automatically by default, but you can create additional profiles either at startup in the profile-selection dialog or in the program preferences.
If multiple profiles are configured, the current profile name appears in square brackets in the main window's title bar.
Each profile stores information such as the Livebox URL and password. The program also automatically associates the physical (MAC) address of the Livebox used for the connection with that profile.

What happens when the program starts?
- If you hold down `Ctrl`, the program displays a dialog for selecting the profile to use.
- Otherwise, if a default profile is configured, it is selected.
- Otherwise, the program goes through the profiles and checks whether a Livebox with the same physical address as the one associated with a profile responds at that profile's URL. The first profile that meets these criteria is selected.
- If no profile is found, the program displays a dialog for selecting one.

The profile-selection dialog warns you if you try to use a profile with a different Livebox from the one it is normally associated with. If you confirm, the profile will be updated to associate it with the new Livebox. The dialog also lets you create a new profile if none of the existing profiles is suitable.


## Command-line options <a id="commandline"></a>

- `--redir` `-r`  
  Redirects a URL configured or used by the program to connect to the Livebox or a Wi-Fi repeater.
  The format is `url1=url2`.  
  Example: `LiveboxMonitor --redir http://livebox/=http://myproxy:2080`  
  With this option, the program uses `http://myproxy:2080/` to connect to `http://livebox/` instead.  
  This option can be used more than once on the same command line.


## Remote access <a id="remoteaccess"></a>

You can use the program remotely if remote access is enabled on the Livebox.
In the Livebox configuration interface, click the **My remote access** tile. Enable access, configure a username, password, and custom port number. You can also enable DynDNS to avoid having to change the URL whenever your public IP address changes.

Your connection URL will then look something like this: `https://myIP:myPort/`.
If you have configured DynDNS, it will look something like this: `https://myDomainName.com:myPort/`.
Make sure the connection username and password are configured accordingly.


## Configuration <a id="configuration"></a>

### Configuration directory
- If the program is launched from source code, the configuration directory is the same directory that contains the launch source file, `lbm.py`.
- If you use the PyPI package (installed with `pip`) or a program built with [PyInstaller](https://pyinstaller.org), the configuration directory is located in the system's standard application-data location:
    - Windows: `%APPDATA%\LiveboxMonitor`
    - macOS: `~/Library/Application Support/LiveboxMonitor`

The program automatically creates three files in its configuration directory:
- `Key.txt`: a unique encryption key generated to encrypt all passwords. This key is itself encrypted using a key calculated by the program from unique characteristics of your PC (including its name). If something significant about your PC changes (the processor, operating system, name, etc.), this key is regenerated automatically and all passwords must be entered again.
- `Config.txt`: contains all application settings (JSON format).
- `MacAddrTable.txt`: maps MAC addresses to device names (JSON format).

### The Config.txt file

This JSON file is managed automatically by the application and normally should not need to be edited. The main settings can be changed using the `Preferences...` button on the `Actions` tab.
Note the following:
- Passwords are stored encrypted using the encryption key in `Key.txt`.
- The `Repeaters` key is generated automatically if different passwords are used for one or more connected Orange Wi-Fi repeaters. This setting is also in JSON format. It uses repeater MAC addresses as keys and stores the `User` and `Password` values for each repeater.

### The MacAddrTable.txt file

This JSON file is managed automatically by the application and normally should not need to be edited.
The keys are device MAC addresses, and the values are the assigned names.
Any detected device whose MAC address is not listed will be displayed as **UNKNOWN** in red. This feature is particularly useful for detecting new devices or possible intrusion attempts.

Why use a local name database when the Livebox also stores device names?
- Because the Livebox "forgets" any device that has not connected for more than a month.
- Because the Livebox sometimes unexpectedly loses names for certain devices. Wi-Fi repeaters are one example.
A local name file helps you determine whether a device really is unknown.

### The SpamCalls.txt file

This JSON file is managed automatically by the application and normally should not need to be edited.
It contains all phone numbers identified as unwanted, stored in international format. Calls from these numbers are automatically marked as "spam" on the `Phone` tab.


## Linux <a id="linux"></a>

If you use a virtual environment and Qt plugin errors occur, add the following commands during installation:

```
    apt install qt6-base-dev
    apt install '^libxcb.*-dev' libx11-xcb-dev libglu1-mesa-dev libxrender-dev libxi-dev libxkbcommon-dev libxkbcommon-x11-dev
```

If you encounter Wayland errors, you can change Qt's rendering backend using the `QT_QPA_PLATFORM` environment variable.
For example, `QT_QPA_PLATFORM=xcb python3 lbm.py` uses X Window directly (which may be rendered through Xwayland, but this works).

Another approach is to unset the `WAYLAND_DISPLAY` environment variable only for the program's execution, rather than globally. To do so, run `env -u WAYLAND_DISPLAY python3 lbm.py`.


## Devices — Connected device list <a id="devicelist"></a>

### List
The device list displays the following columns:
- **T**: icon representing the device type. The type can be assigned or changed using the `Assign Type...` button on the `Device Info` tab.
- **Name**: the device's local name. It can be assigned, changed, or removed using `Assign Name...` on the `Device Info` tab.
- **Livebox Name**: the device name configured in the Livebox and shown in its web interface. It can be assigned, changed, or removed using `Assign Name...` on the `Device Info` tab.
- **MAC**: the MAC address, also known as the device's physical address.
- **IP**: the device's IPv4 address on the LAN. It appears in bold if it is reserved for this device in the Livebox DHCP configuration, and in red if it is unreachable on the network, typically when the device is inactive.
- **Access**: the device's network access point. First, this shows the device name (the Livebox itself or the name of a connected Orange Wi-Fi repeater), followed by the interface on that device. `Eth` means one of the Ethernet ports followed by its port number. `Wifi` means a Wi-Fi connection followed by the connection band.
- **A**: an icon indicating whether the device is active. By default, the list is sorted to show active devices first.
- **Wi-Fi**: Wi-Fi connection quality.
- **E**: an exclamation-mark icon ![Icon](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Icon_Exclamation.png) indicates that an event has been received for this device. The detailed event list and event contents can be viewed on the `Events` tab.
- **Rx**: number of bytes received by the device since the Livebox last started.
- **Tx**: number of bytes sent by the device since the Livebox last started.
- **RateRx**: bytes received per second by the device over the last 30 seconds when shown in black, or over the last 3 seconds when shown in blue (the frequency can be changed in preferences).
- **RateTx**: bytes sent per second by the device over the last 30 seconds when shown in black, or over the last 3 seconds when shown in blue (the frequency can be changed in preferences).

The bytes-per-second statistics for received and sent traffic are calculated from statistics the Livebox sends as an event every 30 seconds per device. Because this resolution is not very meaningful, the program uses another available interface for Wi-Fi devices only to obtain statistics every 3 seconds. The preferences let you enable or disable these statistics and adjust their frequency. These more frequent statistics are displayed in blue.
If a statistic is displayed in red, the Livebox has detected transfer errors.
The statistics may sometimes look surprising, but they are an unfiltered interpretation of the data returned by the Livebox; this is not a program defect.
**Note:** Standard TV traffic from the UHD TV decoder is not included in this device's statistics. However, traffic from VOD services such as Netflix is included.

### Buttons
The `Devices` tab provides the following buttons:
- **`Refresh`**: forces the device list to refresh, not only on this tab but also on the `Device Info` and `Events` tabs. This is useful, for example, if the program is running while the computer wakes from sleep: some events may have been missed, and refreshing restores an up-to-date view.
- **`Assign Names...`**: assigns the names automatically given by the Livebox to all unknown devices.
- **`Device Info`**: switches to the `Device Info` tab for the selected device and displays its information.
- **`Device Events`**: switches to the `Events` tab for the selected device and displays the events received for it.
- **`IPv6...`**: displays the IPv6 enablement status, the Livebox IPv6 address and prefix, and a list of connected or disconnected devices with one or more assigned IPv6 addresses. It also displays the connection mode and lets you control the CGNAT setting.
- **`DNS...`**: displays the DNS names assigned to devices. These names can be assigned, changed, or removed using `Assign Name...` on the `Device Info` tab.


## Livebox stats/info — Traffic statistics and advanced Livebox information <a id="infos"></a>

![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_LiveboxInfos.png)

Traffic statistics for each interface appear in a list at the top left.
All other information is available through buttons, each of which fills the attribute list on the right. You can also export all the information to a file.

### Statistics

This list lets you monitor traffic:
- **Name**: network interface name. `Fiber` covers all WAN traffic, meaning external traffic between the Livebox and the internet. `LAN` covers all internal traffic passing through the Livebox. Statistics are also available for individual interfaces. `Guest` interfaces show guest Wi-Fi traffic, if guest Wi-Fi is enabled.
- **Rx**: number of bytes received by the interface. The time window covered by this total is unknown. It appears in red if transmission errors are detected. Note that for some interfaces this is a circular counter and cannot exceed 4 GB. For other interfaces, a higher-resolution counter is read roughly every 30 seconds and replaces the basic counter, which is limited to 4 GB, when the program starts.
- **Tx**: number of bytes sent by the interface. The time window covered by this total is unknown. It appears in red if transmission errors are detected. Note that for some interfaces this is a circular counter and cannot exceed 4 GB. For other interfaces, a higher-resolution counter is read roughly every 30 seconds and replaces the basic counter, which is limited to 4 GB, when the program starts.
- **RateRx**: bytes received per second by the interface over the last 3 seconds (the frequency can be changed in preferences). It appears in red if transmission errors are detected.
- **RateTx**: bytes sent per second by the interface over the last 3 seconds (the frequency can be changed in preferences). It appears in red if transmission errors are detected.

If a statistic is displayed in red, the Livebox has detected transfer errors.
The statistics may sometimes look surprising, but they are an unfiltered interpretation of the data returned by the Livebox; this is not a program defect.
**Note:** Standard TV traffic from the UHD TV decoder is not included in these statistics. However, traffic from VOD services such as Netflix is included.

### Buttons
The `Livebox Stats/Info` tab provides the following buttons:
- **`Livebox Info`**: displays the main Livebox information, such as software versions, WAN IP address, active services, memory status, and so on.
- **`Internet Info`**: displays the internet access type, connection credentials, IPv4 and IPv6 addresses, the date and time of the last connection, connection bandwidth, MTU, and so on.
- **`Wi-Fi Info`**: displays general Wi-Fi connectivity information and the status of each access point, including guest access points. For each access point, detailed information is available, such as channel, standard, bandwidth, quality, band, and number of connected devices.
- **`LAN Info`**: displays general LAN connectivity information, including basic DHCP information and whether each Ethernet interface is active, its bandwidth, and so on.
- **`ONT Info`**: displays important information about the connection and the fibre module (ONT), such as bandwidth, signal quality, serial number, and software versions. The `Receive Signal Power`, `Transmit Signal Power`, `Temperature`, `Voltage`, and `BIAS` fields are green when within acceptable connection-quality standards, red when indicating a problem, and orange when close to the acceptable limits.

![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_LiveboxInfos_ONT.png)
- **`VoIP Info`**: displays general telephony information, such as the protocol, telephone number, and DECT interface software version.
- **`IPTV Info`**: displays general information about TV services.
- **`USB Info`**: displays information about the USB port or ports. If a USB drive is currently connected, or has been connected since the Livebox last started, its information is displayed.
- **`Export...`**: exports all information displayed by the buttons to a text file. This is useful for sharing the information or tracking changes over time.


## Graphs — Traffic graphs by interface and device <a id="graphs"></a>

![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Graph.png)

This tab displays graphs of received and transmitted data for each selected interface or device, using data stored by the Livebox itself. Volumes are shown in megabytes per second, usually sampled every 30 seconds (sampling and its frequency are controlled by the Livebox). Graphs update automatically when new samples arrive as events.
You can navigate and zoom in the graphs with the mouse, then return to the normal view by clicking in the lower-left corner. Right-clicking a graph opens additional features, such as exporting it in different formats. Full documentation is available [here](https://pyqtgraph.readthedocs.io/en/latest/user_guide/index.html).

### Selecting interfaces and devices
First, select the interfaces and/or devices you want to graph. Two buttons are available:
- **`Add...`**: opens a dialog where you can select an interface or a device.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Graph_Add.png)

    First select the type, interface or device. Then choose the relevant interface or device and the colour to use for that item on the graph. Only items for which measurements may be available are shown. For devices, local names are used, or the MAC address by default. Additional information is shown in the dialog: the Livebox's internal identifier for the selected item, the number of samples stored by the Livebox for that item (the maximum is set by the Livebox at 8,680), and an estimate of the corresponding time window. This estimate may be inaccurate: if an interface or device is disconnected for a period, no samples are emitted during that time. As a result, the elapsed time between the oldest and newest samples may be much longer than estimated.
- **`Remove`**: removes the selected interface or device.

### Graph options
The following options are available for configuring graphs:
- **`Window`**: time window in hours to display, counting back from the present. A value of `0` displays all statistics available in the Livebox for the selected items.
- **`Background colour`**: background colour for the receive and transmit graphs. Right-click to clear the colour and use the default instead.

### Generating graphs
The **`Apply`** button loads all information for the selected interfaces and devices, draws the corresponding graphs using the options above, and saves the configuration. The configuration is automatically reloaded the next time the program starts.

The **`Export...`** button exports to CSV (using a semicolon as the separator) the most recently loaded data from `Apply`, along with any data received through events in the meantime.


## Device info — Detailed information about each known device <a id="deviceinfos"></a>

![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_DeviceInfos.png)

The list of known devices on the left displays these columns:
- **Name**: the device's local name. It can be assigned, changed, or removed using `Assign Name...`.
- **MAC**: the MAC address, also known as the physical address.

When you select a device in this list, its detailed information appears in the list on the right. Notable attributes include:
- **Active**: indicates whether the device is active.
- **Authenticated**: indicates whether the device's Wi-Fi connection has been authenticated.
- **Blocked**: indicates whether you have blocked the device. Use `Block`/`Unblock` to control this. Blocking an unknown device may be useful if you suspect it is unauthorized.
- **Scheduler**: indicates whether a connection schedule is active for this device. The schedule can be managed using `Scheduler...`.
- **First Connection**: date and time of the first connection. Note that this may also correspond to the date and time of a previous Livebox restart.
- **Last Connection**: date and time of the most recent connection.
- **Last Change**: date and time when the last change to this device was detected.
- **Name**: the name known by the Livebox for this device, with the source of the name in parentheses. Multiple names may appear if they come from different sources.
- **Type**: the device type known by the Livebox, with the source of the type in parentheses. Multiple types may appear if they come from different sources.
- **IPvX Address**: the device's IP address (IPv4 or IPv6). Parentheses indicate whether the address is active and reachable on the network. If the address is reserved for this device in the Livebox DHCP configuration, a `Reserved` label is shown.
- **Manufacturer**: the device manufacturer inferred from its MAC address. The program uses the [macaddress.io](https://macaddress.io/) API to identify the manufacturer. The service is free, but you must create an account and enter the corresponding API key in preferences to use this feature.
- **Wi-Fi Signal Strength** and **Wi-Fi Signal Noise Ratio**: indicate the quality of the Wi-Fi connection for wireless devices.

### Buttons
The `Device Info` tab provides the following buttons:
- **`Refresh`**: refreshes the information displayed for the selected device.
- **`Assign Name...`**: assigns or clears the local name, Livebox name, and/or DNS name for the selected device.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_DeviceInfos_AssignName.png)

    Uncheck the box to clear a name. The three names can, of course, be different.
- **`Assign Type...`**: assigns or clears the type of the selected device.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_DeviceInfos_AssignType.png)

    You can select one of the standard types known to the Livebox from the menu; each type is displayed with its corresponding Livebox icon. When a standard type is selected, its name as known by the Livebox is filled in automatically in the text field, and you can confirm the dialog. You can also assign a type not known by the Livebox by typing it directly into the field. Note: although the type `Djingo Speaker` is listed as standard by Livebox 5, it does not appear to be supported (yet?) by the Livebox graphical interface. **Warning:** if you assign the type `Wi-Fi Repeater` (values `repeteurwifi`, `repeteurwifi6`, or `SAH AP`) to a device, the software will identify it as an Orange repeater and try to connect to it. If it is not actually an Orange repeater, a connection error will occur at startup.
- **`Forget...`**: asks the Livebox to permanently forget the device, which immediately disappears from all lists. If the device is active, its connection will not be suspended; however, all its activity will remain invisible until its next connection attempt.
- **`WakeOnLAN`**: sends a wake-up signal over the network to the selected device. The device must be configured to power on when it receives this signal (Wake-on-LAN, or WOL) for this to work.
- **`Scheduler...`**: configures the selected device's network-access schedule. The schedule status is shown in the device information.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_DeviceInfos_Scheduler.png)

    You can configure the schedule with the mouse (green = enabled / red = disabled), in 15-minute increments. The `Add Period` field also lets you configure enable/disable time ranges to the nearest minute. Note that clicking the table afterwards resets the resolution to 15-minute increments.
- **`Block`/`Unblock`**: blocks or unblocks the selected device's connection. Its blocked status is displayed in the device information under `Blocked`.


## Events — Events received for each known device <a id="events"></a>

![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Events.png)

The list of known devices on the left displays these columns:
- **Name**: the device's local name. It can be assigned, changed, or removed using `Assign Name...` on the `Device Info` tab.
- **MAC**: the MAC address, also known as the device's physical address.

When you select a device in this list, all events received for that device since the program started appear in the list on the right. The program interprets most of these events to update the interface dynamically, and this list provides a history. The history is limited to the latest 100 entries per device. You can easily change this limit by modifying the `MAX_EVENT_BUFFER_PER_DEVICE` variable in `LmEventsTab.py`.

The event list contains these columns:
- **Time**: the time the event was received.
- **Reason**: the type of event generated by the Livebox.
- **Attributes**: a preview of the event's raw data in JSON format.

Double-click an event or click **`Show Event`** to open a dialog containing the full details:
- **Raised**: the exact date and time the event was received.
- **Handler**: the event handler, usually containing the device key, which is its MAC address.
- **Reason**: the event type.
- **Attributes**: the complete raw event data in JSON format, as generated by the Livebox.

### Automatic notifications
- The **`Notifications...`** button opens the settings for automatic notifications to generate when certain events are detected (for example, email notifications).

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Events_Notifications.png)

    You can create as many rules as needed. Multiple rules may apply to the same devices, and the program does not detect duplicate rules. A notification is sent for a device as long as at least one matching rule requests it.
    The list displays the configured rules and the devices concerned. Blue dots indicate the events selected for notification; green dots indicate the selected notification types (email and/or daily CSV files). Use **`Add`** and **`Remove`** to create a rule or remove the selected rule.

    Rule settings:
    - **`Device`**: selects the device or devices for which to receive notifications. `All devices` applies the rule to every device. `All unknown devices` applies it to any unknown device.
    - **`MAC address`**: the physical address of the selected device.
    - **`Events`**: selects which events should trigger a notification, such as adding or removing devices, connecting, disconnecting, or changing access points (useful if you have one or more Orange Wi-Fi repeaters).
    - **`Actions`**: selects how notifications should be delivered. Events can be recorded in a daily CSV file or the details of each event can be sent immediately by email.

    Notification preferences:
    - **`Event Resolution Interval`**: events are detected immediately, but some events may cancel each other out when they occur within a short period. For example, a device disconnecting and reconnecting within 15 seconds is common. To avoid unnecessary notifications, a 30-second delay is strongly recommended so that the program can identify such situations. A delay shorter than 5 seconds is strongly discouraged to prevent excessive resource usage.
    - **`CSV directory`**: the directory in which to create daily CSV files. Select `Default` to use the [program configuration directory](#configuration), or click `Select` to choose a specific directory.


## DHCP — Fine-grained DHCP server control <a id="dhcp"></a>

![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_DHCP.png)

### Static leases
The configured static leases are listed on the left. Two network domains are supported:
- `Home`: your private network, whose IP address range you can configure.
- `Guest`: the guest Wi-Fi network, which has its own IP range that cannot be changed.

The list contains these columns:
- **Name**: the device's local name. It can be assigned, changed, or removed using `Assign Name...` on the `Device Info` tab.
- **Domain**: the assignment domain, either `Home` or `Guest`.
- **MAC**: the MAC address, also known as the physical address.
- **IP**: the IP address statically assigned to the device.

### Buttons
The following buttons are available to manage the lease list:
- **`Refresh`**: refreshes the list of static leases.
- **`Add...`**: adds a lease.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_DHCP_AddBinding.png)

    The suggested device list is sorted and combines connected devices with those listed in the local MAC-address file. The MAC address is filled in from the selected device, but you can also type one manually. Choose the network domain (`Home` or `Guest`). A free IP address is suggested automatically, but you can change it. Note that a device can be configured in only one domain. If it connects to a domain while a static lease is configured for the other domain, that lease is automatically removed.
- **`Remove`**: removes the selected lease.

### Detailed DHCP information
The list on the right displays extensive DHCP server information:
- Detailed configuration for both the `Home` and `Guest` domains.
- Information about DHCPv4, including all DHCP options sent and received.
- Information about DHCPv6, including all DHCP options sent and received.

### Buttons
The following buttons are available for the DHCP information:
- **`Refresh`**: refreshes the DHCP information.
- **`DHCP Settings...`**: configures the DHCP server.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_DHCP_Setup.png)

    You can enable or disable the server, change the Livebox IP address and DHCP server subnet mask, and change the IP address range for the `Home` domain.


## NAT/PAT — Port and protocol forwarding rules <a id="natpat"></a>

![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_NatPat.png)

This tab lets you finely manage the Livebox's IPv4, IPv6, and UPnP port- and protocol-forwarding rules. Because the program exposes more detailed control, some rules may not appear in the Livebox web interface even though they are stored and interpreted correctly by the Livebox. This is normal and is not necessarily a problem.

### Port forwarding
The list of port-forwarding rules at the top contains these columns:
- **A**: an icon indicating whether the rule is active.
- **Type**: the rule type, either IPv4, IPv6, or UPnP. UPnP rules are managed automatically by the Livebox and normally should not need to be modified.
- **Name**: the rule name.
- **Description**: the rule description.
- **Protocols**: the protocols covered by the rule.
- **Internal Port**: the internal port or port range to which traffic is forwarded.
- **External Port**: the external port or port range to forward.
- **Device**: the device (or its IP address) to which traffic is forwarded.
- **External IPs**: the external IP addresses covered by the rule.

You can easily edit a rule by **double-clicking** it.

### Buttons
The following buttons are available to manage the rules:
- **`Refresh`**: refreshes the list of port-forwarding rules.
- **`Enable/Disable`**: enables or disables the selected rule.
- **`Add...`**: adds a port-forwarding rule.
- **`Edit...`**: edits the selected port-forwarding rule.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_NatPat_PortForwarding.png)

- **`Remove`**: removes the selected port-forwarding rule.
- **`Remove All...`**: removes all port-forwarding rules. You can choose to remove only selected rule types. If one or more rules cause problems for the Livebox (for example, because a rule name contains special characters), this button may help restore a working state.
- **`Export...`**: exports port-forwarding rules of one or more selected types to a file.
- **`Import...`**: imports port-forwarding rules previously exported to a file. Existing rules with the same names are overwritten by the imported rules.

### Protocol forwarding
The list of protocol-forwarding rules at the bottom contains these columns:
- **A**: an icon indicating whether the rule is active.
- **Type**: the rule type, either IPv4 or IPv6.
- **Name**: the rule name.
- **Description**: the rule description.
- **Protocols**: the protocols covered by the rule.
- **Device**: the device (or its IP address) to which traffic is forwarded. For IPv6, you can specify a prefix instead of an address.
- **External IPs**: the external IP addresses covered by the rule.

You can easily edit a rule by **double-clicking** it.

### Buttons
The following buttons are available to manage the rules:
- **`Refresh`**: refreshes the list of protocol-forwarding rules.
- **`Enable/Disable`**: enables or disables the selected rule.
- **`Add...`**: adds a protocol-forwarding rule.
- **`Edit...`**: edits the selected protocol-forwarding rule.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_NatPat_ProtocolForwarding.png)

- **`Remove`**: removes the selected protocol-forwarding rule.
- **`Remove All...`**: removes all protocol-forwarding rules. You can choose to remove only selected rule types. If one or more rules cause problems for the Livebox (for example, because a rule name contains special characters), this button may help restore a working state.
- **`Export...`**: exports protocol-forwarding rules of one or more selected types to a file.
- **`Import...`**: imports protocol-forwarding rules previously exported to a file. Existing rules with the same names are overwritten by the imported rules.


## Phone — Call history and contacts <a id="phone"></a>

![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Phone.png)

### Phone calls
The call list on the left displays these columns:
- **T**: an icon indicating the call type.
    - ![Icon](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Icon_Call_In.png): incoming call.
    - ![Icon](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Icon_Call_In_Missed.png): missed call. The entire row is shown in red.
    - ![Icon](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Icon_Call_Out.png): outgoing call.
    - ![Icon](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Icon_Call_Out_Failed.png): outgoing call that did not connect.
- **Time**: the date and time of the call.
- **Number**: the relevant telephone number.
- **Contact**: the contact name identified by the Livebox using the contact list as it existed at the time of the call. If the Livebox did not store a name, the program tries to find one dynamically by matching the number against saved contacts. If an unwanted number has been stored (see `SpamCalls.txt`), it is automatically shown as `# SPAM #`.
- **Duration**: the call duration.

You can easily create or edit the corresponding contact by **double-clicking** a call.

### Buttons
The following buttons are available for managing calls:
- **`Refresh`**: refreshes the call list.
- **`Remove`**: removes the selected call.
- **`Remove All...`**: removes all calls.
- **`Scan for spam`**: checks all unidentified incoming calls using the [callfilter.app](https://callfilter.app/) API to determine whether a number is unwanted (telemarketing, fraud, etc.). Identified spam calls are marked as `# SPAM #` and stored in `SpamCalls.txt` in the [program configuration directory](#configuration). This JSON file can be edited manually, but do not edit it while the program is running. To use this feature, configure a Call Filter API key in preferences. To request an API key (the free service allows a maximum of 15 requests per minute), send an email in English to info@callfilter.app and specify:
    - The tasks for which you intend to use the API.
    - The number of requests you expect to make per day, minute, and hour.
- **`Spam websites`**: opens [numeroinconnu.fr](https://www.numeroinconnu.fr/) and [callfilter.app](https://callfilter.app/) in your browser so you can check the origin of the selected call.
- **`Mark/Unmark spam`**: manually marks or unmarks an incoming call as unwanted. This action is also saved in the local `SpamCalls.txt` file.

### Contacts
The contact list on the right displays these columns:
- **Name**: the contact's name, in surname + first-name format.
- **Mobile**: mobile phone number.
- **Home**: landline number.
- **Work**: work telephone number.
- **Ringtone**: ringtone type, selected from the seven supported by the Livebox.

You can easily edit a contact by **double-clicking** it.
**Warning:** the Livebox supports a maximum of 255 contacts.

### Buttons
The following buttons are available for managing contacts:
- **`Refresh`**: refreshes the contact list.
- **`Add...`**: adds a contact. Note that duplicate contacts are not checked.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Phone_Contact.png)

- **`Edit...`**: edits the selected contact.
- **`Remove`**: removes the selected contact.
- **`Remove All...`**: removes all contacts.
- **`Ringtone`**: tests the telephone. You can select one of the seven ringtones supported by the Livebox to the left of the button; otherwise, the default ringtone is used.
- **`Export...`**: exports all contacts to a [VCF (vCard) file](https://en.wikipedia.org/wiki/VCard), which is useful for backup.
- **`Import...`**: imports one or more [VCF (vCard) files](https://en.wikipedia.org/wiki/VCard). Duplicate contacts are not checked. If the maximum supported number of contacts (255) is reached, the import is stopped.


## Actions — Action and control buttons <a id="actions"></a>

![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions.png)

This tab displays a list of actions grouped by category.

Wi-Fi actions:
- **`Configuration...`**: configures all Wi-Fi radio bands. Note that changing the channel width is not guaranteed to work in every case. For example, changes to the 5 GHz band are not applied by Livebox 4 and 5.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_WifiConfig.png)

    The `ON` and `OFF` buttons quickly enable or disable the Livebox Wi-Fi interface.
- **`Guest...`**: configures all radio bands for the guest Wi-Fi network.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_WifiConfigGuest.png)

    The `ON` and `OFF` buttons quickly enable or disable the Livebox guest Wi-Fi interface.
- **`Scheduler...`**: configures the Livebox Wi-Fi schedule.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_Scheduler.png)

    You can configure the schedule with the mouse (green = enabled / red = disabled), in 15-minute increments. The `Add Period` field also lets you configure enable/disable time ranges to the nearest minute. Note that clicking the table afterwards resets the resolution to 15-minute increments.
    If one or more Orange Wi-Fi repeaters are connected to the Livebox, the `Apply to all repeaters` option applies exactly the same schedule to all of them.
    The `ON` and `OFF` buttons quickly enable or disable the scheduler.
- **`Global Wi-Fi Status...`**: displays the overall Wi-Fi status, including the Wi-Fi status of any connected Orange Wi-Fi repeaters.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_WifiGlobalStatus.png)

Miscellaneous actions:
- **`Backup and Restore...`**: configures automatic backups of the Livebox configuration, starts a backup, or requests restoration of a configuration.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_BackupRestore.png)
- **`LEDs and Screen...`**: adjusts LED brightness and whether the Wi-Fi password is displayed on the Livebox screen. This feature is available only on Livebox 6 and later.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_Screen.png)

Restart actions:
- **`Restart Livebox...`**: forces the Livebox to restart.
- **`Reset Livebox...`**: resets the Livebox to its factory settings. If automatic configuration restoration is enabled, a dialog lets you disable it.
- **`Restart History...`**: displays the history of recent restarts.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_RebootHistory.png)

    This history is especially useful for detecting restarts forced by Orange to update the Livebox software.
    The list displays these columns:
    - **Restart Date**: date and time the device started.
    - **Restart Reason**: reason for the startup. Typically, `NMC` indicates a software-forced startup, while `Unsupported chipset` indicates a restart caused by a power outage or by operating the Livebox's power switch.
    - **Shutdown Date**: date and time the device stopped.
    - **Shutdown Reason**: reason for the shutdown. This is typically blank after a power outage, `Upgrade` for a software update, and `GUI_Reboot` for a restart requested from the web interface or LiveboxMonitor.

Network actions:
- **`Firewall Levels...`**: sets the IPv4 and IPv6 firewall levels.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_FirewallLevels.png)
- **`Ping Responses...`**: configures responses to IPv4 and IPv6 ping requests.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_PingResponses.png)
- **`Speed Tests...`**: runs quick speed tests provided by Orange. These can be useful for quick checks, but websites such as nperf.com or speedtest.net provide much more reliable results.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_SpeedTests.png)

    Two tests are available: the Livebox's historical native test and the test used by the "mon réseau local" website. Results may take up to a minute to arrive and are stored in the configuration file so they remain available each time the dialog is opened.
- **`DynDNS...`**: configures DynDNS domains.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_DynDNS.png)
- **`DMZ...`**: configures DMZ rules. Unlike Orange's application, this allows you to add multiple devices to the DMZ and filter external IP addresses, as with NAT/PAT rules.

    The rule created by Orange's application has the ID `webui`. To manage a rule from both LiveboxMonitor and Orange's application, you must preserve this identifier.
    Since this has not been tested, correct operation with multiple rules is not guaranteed. **Feedback from anyone who tries it is welcome.**

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_DMZ.png)
- **`Routing Table...`**: configures the static routing table. This option is available only on Livebox Pro models.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_Routing.png)

Settings actions:
- **`Preferences...`**: opens the program preferences.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_Preferences.png)

    See **Profiles** above for more details on how profiles work.

    The following settings can be configured for each **profile**:
    - `Name`: profile name.
    - `Livebox URL`: Livebox address. The default is `http://livebox.home/`.
    - `User`: login username. The default is `admin`. If the password is missing or incorrect, the program asks for it when you use the profile.
    - `Device Filtering`: filters out certain "ghost" devices detected by the Livebox. When enabled, the program displays the same devices as the Livebox web interface. This option is enabled by default.
    - `MacAddr Table File`: filename used to store device names. The default is `MacAddrTable.txt`. See the section on `MacAddrTable.txt` above for more information.
    - `Default`: indicates that this is the default profile used when the program starts. Only one default profile can be configured.

    The **general preferences** let you configure:
    - `Language`: application language, either French (the default) or English.
    - `Tooltips`: enables or disables tooltips.
    - `Stats Frequency`: statistics refresh interval, in seconds. The default is 3 seconds.
    - `macaddress.io API Key`: the program uses the [macaddress.io](https://macaddress.io/) API to identify a device manufacturer from its MAC address (the **Manufacturer** field in detailed device information). The service is free, but you must create an account and enter your API key here to use it.
    - `CallFilter API Key`: the program uses the [callfilter.app](https://callfilter.app/) API to identify unwanted phone calls (on the `Phone` tab). To obtain an API key (the free service allows up to 15 requests per minute), email info@callfilter.app in English.
    - `Phone Country Code`: local telephone country code, used to match call numbers with contact numbers. The default is France's code, `33`.
    - `Header Height`: list-header height in pixels. The default is 25.
    - `Header Font Size`: list-header font size. A value of zero uses the system font size; zero is the default.
    - `Row Height`: list-row height in pixels. The default is 30.
    - `Row Font Size`: list-row font size. A value of zero uses the system font size; zero is the default.
    - `Timeout Margin`: additional timeout value, useful for example when accessing a Livebox remotely over a high-latency network.
    - `CSV Separator`: list delimiter used when exporting data to CSV files.
    - `Real-time Wi-Fi Device Statistics`: enables or disables real-time statistics for Wi-Fi devices. These appear in blue on the `Devices` tab and overlay the standard statistics, which appear in black every 30 seconds.
    - `Prevent Sleep`: prevents the computer from going to sleep while this program is running. This is useful when leaving the application running continuously, for example to export statistics or generate real-time notifications.
    - `Use Native GUI Style`: by default, the `Fusion` style is used on all platforms. This option enables a more native-looking graphical style on Windows and macOS. It has no effect on Linux.
    - `Save Passwords`: saves passwords (encrypted) in the configuration so you do not have to enter them every time the program starts.

- **`Change Profile...`**: opens a dialog to change the current profile and restart the program.

- **`Email Settings...`**: configures automatic email sending, for example for notifications.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_EmailSetup.png)

    The following settings can be configured:
    - `From Address`: sender email address.
    - `To Address`: recipient email address.
    - `Subject Prefix`: prefix added to the subject of outgoing messages.
    - `SMTP Server`: your email provider's SMTP server.
    - `Port`: SMTP port to use. Port 465 is recommended for SSL, and 587 for other protocols.
    - `Use TLS`: use the TLS encryption protocol (recommended).
    - `Use SSL`: use the SSL encryption protocol.
    - `Authentication`: select this if the server requires authentication.
    - `User`: username for authentication.
    - `Password`: password for authentication.

    The `Test Send` button sends a test message using the current settings without saving them.

    Configuration for Orange's email server is straightforward:
    - From address / user: enter your `address@orange.fr`.
    - To address: enter the intended recipient.
    - SMTP server: `smtp.orange.fr`.
    - Port: `587`.
    - Select `Use TLS` and `Authentication`.
    - Password: your Orange password.

    Gmail requires a few more steps:
    - First, configure your Gmail account to create a dedicated app password for this program.
    - Open your account's **Security** tab [here](https://myaccount.google.com/security).
    - Check that **2-Step Verification** is enabled; if not, enable it.
    - Click **2-Step Verification**, then open **App passwords** at the bottom.
    - Create a new app password for `LiveboxMonitor` and note it down without the spaces.
    - Return to LiveboxMonitor's email settings.
    - From address / user: enter your `address@gmail.com`.
    - To address: enter the intended recipient.
    - SMTP server: `smtp.gmail.com`.
    - Port: `587`.
    - Select `Use TLS` and `Authentication`.
    - Password: enter the app password you created above.

Debugging actions:
- **`Log Level...`**: changes the log level shown in the console. This setting is saved in the program configuration and persists the next time the program starts.
- **`Call APIs...`**: calls the Livebox REST/JSON APIs directly. A selection of calls is available as shortcuts, but any call can be made. Intended for advanced users only.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_CallAPIs.png)

- **`Generate API Documentation...`**: generates text files containing all documentation available for the Livebox APIs, organized by module. The program creates one file for each known module, an `_ALL_MODULES_` file containing all modules in one file, and a `_PROCESSES_` file listing the tasks running on the Livebox. Some files contain only a `Permission denied` error; this is normal because those modules are protected and inaccessible (though that might change in a future firmware version). Function parameters shown in parentheses are optional. By default, the program generates all instances found by resource type (or "object") as well as all values found for parameters. These values are filtered if you hold down `Ctrl` while clicking the button. This makes it possible to share the files without disclosing configuration-specific information. However, having the list of instances is essential for truly comprehensive documentation. Unfortunately, Orange has blocked this capability on the latest Livebox W7 and S models.

Other actions:
- **Exit the application**: closes the application. This is exactly equivalent to closing its window.
- Clicking the application's GitHub link opens the corresponding page in your web browser.

## Wi-Fi repeater tabs <a id="repeaters"></a>

![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Repeater.png)

The program dynamically creates a tab for each connected Orange repeater. If a repeater has a local name, that name is used for the tab; otherwise, the default name is `RW #` followed by the repeater number in detection order.
Repeaters are also treated as known devices, so they appear on the `Devices` tab and their names can be changed using `Assign Name...` on the `Device Info` tab.

An icon in the tab name indicates the connection status of the repeater:
- ![Icon](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Icon_Cross.png): the repeater is inactive or has no IP address assigned.
- ![Icon](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Icon_Prohibition.png): the repeater is active, but no session is open. If this status persists, try forcing a new session by clicking `Reauthenticate...`.
- ![Icon](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Icon_Tick.png): the repeater is active and a session has been created to communicate with it.

Traffic statistics for each interface appear in a list at the top left.
All detailed information is available through the row of buttons at the bottom; each button fills the attribute list on the right. You can also export all the information to a file. Finally, the buttons on the left provide a number of actions.

### Statistics

This list lets you monitor traffic handled by the repeater:
- **Name**: network interface name. `LAN` covers all traffic between the repeater and the Livebox. Statistics are also available for individual interfaces (the two Ethernet ports and the two Wi-Fi bands).
- **Rx**: number of bytes received by the interface. The time window covered by this total is unknown. It appears in red if transmission errors are detected. Note that this is a circular counter and cannot exceed 4 GB.
- **Tx**: number of bytes sent by the interface. The time window covered by this total is unknown. It appears in red if transmission errors are detected. Note that this is a circular counter and cannot exceed 4 GB.
- **RateRx**: bytes received per second by the interface over the last 3 seconds (the frequency can be changed in preferences).
- **RateTx**: bytes sent per second by the interface over the last 3 seconds (the frequency can be changed in preferences).

If a statistic is displayed in red, the repeater has detected transfer errors.
The statistics may sometimes look surprising, but they are an unfiltered interpretation of the data returned by the repeater; this is not a program defect.

### Actions — Action and control buttons

Wi-Fi actions:
- **`Wi-Fi ON`**: enables the repeater's Wi-Fi interface.
- **`Wi-Fi OFF`**: disables the repeater's Wi-Fi interface.
- **`Wi-Fi Scheduler...`**: configures the repeater's Wi-Fi schedule.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Actions_Scheduler.png)

    You can configure the schedule with the mouse (green = enabled / red = disabled), in 15-minute increments. The `Add Period` field also lets you configure enable/disable time ranges to the nearest minute. Note that clicking the table afterwards resets the resolution to 15-minute increments.
    If one or more other Orange Wi-Fi repeaters are connected to the Livebox, the `Apply to all repeaters` option applies exactly the same schedule to all of them.
    The `ON` and `OFF` buttons quickly enable or disable the scheduler.

The overall Wi-Fi status can be viewed using the `Global Wi-Fi Status...` button on the `Actions` tab.

Restart actions:
- **`Restart Repeater...`**: forces the repeater to restart.
- **`Reset Repeater...`**: resets the repeater to its factory settings.
- **`Restart History...`**: displays the history of recent restarts.

    ![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Repeater_RebootHistory.png)

    This history is especially useful for detecting restarts forced by Orange to update the repeater software.
    The list displays these columns:
    - **Restart Date**: date and time the repeater started.
    - **Restart Reason**: reason for the startup. Typically, `NMC` or `POR` indicates a software-forced startup, while `Unsupported chipset` indicates a restart caused by a power outage or by operating the repeater's power switch.
    - **Shutdown Date**: date and time the repeater stopped.
    - **Shutdown Reason**: reason for the shutdown. This is typically blank after a power outage, `Upgrade` for a software update, and `WebUI reboot` for a restart requested from the web interface or LiveboxMonitor.

Miscellaneous actions:
- **`Call APIs...`**: calls the repeater's REST/JSON APIs directly. A selection of calls is available as shortcuts, but any call can be made. Intended for advanced users only.
- **`Reauthenticate...`**: forces a new session to be created with the repeater. If you leave the program running for a long time without viewing the repeater statistics or performing any actions, the repeater automatically releases the session. Subsequent actions will then produce errors. Use this button to recreate the session and resume working without errors.

### Buttons
The Wi-Fi repeater tabs provide the following buttons:
- **`Repeater Information`**: displays key information about the repeater, such as software versions, model name, internal clock time, and so on.
- **`Wi-Fi Information`**: displays general Wi-Fi connectivity information and the status of each access point. Detailed information is available for each access point, including channel, standard, bandwidth, quality, band, and number of connected devices.
- **`LAN Information`**: displays general LAN connectivity information. For each Ethernet interface, you can see whether it is active, its bandwidth, and so on.
- **`Export...`**: exports all information displayed by the buttons to a text file. This is useful for sharing the information or tracking changes over time.


## TV decoder tabs <a id="tvdecoders"></a>

![Interface](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Tvdecoder.png)

The program dynamically creates a tab for each connected Orange TV decoder. If a decoder has a local name, that name is used for the tab; otherwise, the default name is `TV #` followed by the decoder number in detection order.
Decoders are also treated as known devices, so they appear on the `Devices` tab and their names can be changed using `Assign Name...` on the `Device Info` tab.

An icon in the tab name indicates the decoder's connection status:
- ![Icon](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Icon_Cross.png): the decoder is inactive or has no IP address assigned.
- ![Icon](http://p-dor.github.io/LiveboxMonitor/docs/png/Doc_Icon_Tick.png): the decoder is active.

### Information
- **`Status`**: the current status reported by the decoder.
- **`Info`**: information about the decoder returned by the Livebox.
- **`Description`**: information reported by the decoder itself.
- **`Channel`**: information about the TV channel currently being played. The EPG identifier corresponds to the channel number in the Electronic Program Guide. Channel icons and descriptions are stored in a local cache to avoid unnecessary calls to Orange servers. If this information becomes incorrect, use the `Rebuild Cache` button.

### Buttons
- **`Rebuild Cache`**: resets the local cache of channel information and icons.
- **`Change Channel`**: changes the channel. You can enter a channel number, a channel name (including a partial name), or a channel's EPG (Electronic Program Guide) identifier by typing `epg` followed by the identifier number.
- **`Remote Control`**: a virtual remote control for operating the decoder. The button at the bottom left displays the decoder administration screens on the TV.


## Managing and customizing device icons <a id="icons"></a>

All device icons are initially stored on the Livebox, and the program retrieves them one by one. This may cause some delay (for example, the first time you click `Assign Type...` to change a device's type).
To avoid this delay, the program stores all icons downloaded from the Livebox in a local cache so that they can be retrieved faster the next time it starts. This cache is stored in a directory named `lbcache_` followed by the Livebox firmware version, allowing multiple caches to be kept when you access Liveboxes running different firmware versions.
This directory is created automatically in the [program configuration directory](#configuration).

You can also modify these icons or create new device types with their own icons.
A dedicated guide explaining these options is available [here](https://github.com/p-dor/LiveboxMonitor/blob/main/docs/CustomIcons.md).


## Using Livebox APIs in your own Python program <a id="codingguide"></a>

All APIs used by the program to communicate with the Livebox are accessible through a software layer that is completely independent of the application itself.

You can easily use these APIs in your own Python program by following this short guide in English [here](https://github.com/p-dor/LiveboxMonitor/blob/main/docs/APICodingGuide.md).

The generated documentation for the Livebox APIs is available in [this directory](https://github.com/p-dor/LiveboxMonitor/tree/main/docs/API%20Documentation). Many APIs are not implemented by the program, but it is easy to add them.
