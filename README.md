<table border="0">
  <tr>
    <td align="left" valign="middle">
    <h1>EFR32 Wi-SUN Application Examples</h1>
  </td>
  <td align="left" valign="middle">
    <a href="https://www.silabs.com/community/blog.entry.html/2020/06/30/guest_blog_q_a_withwi-sunalliancepresidentc-3p1o">
      <img src="http://pages.silabs.com/rs/634-SLU-379/images/WGX-transparent.png"  title="Silicon Labs Gecko and Wireless Gecko MCUs" alt="EFM32 32-bit Microcontrollers" width="250"/>
    </a>
  </td>
  </tr>
</table>

# Silicon Labs Wi-SUN Application Examples

This repository contains resources to evaluate Silicon Labs Wi-SUN FAN stack. It allows a user to flash binary files to EFR32xG12 radio boards and create their first Silicon Labs Wi-SUN network. The document below describes the first possible interactions available through the Wi-SUN CLI application (connect, ping, open a socket...).

To access the Wi-SUN FAN stack source code and start development, refer to the [**dedicated Wi-SUN FAN stack repository**](https://github.com/SiliconLabs/proprietary_wisun_stack).

## Content

- **border_router_images**: contains Wi-SUN border router images for different EFR32xG12 radio board references and different Wi-SUN PHYs (only delivered as a binary image).
- **wisun_cli_images**: contains Wi-SUN CLI application images for different EFR32xG12 radio board references. It acts as a Wi-SUN router node in a network (source code available in the [**Wi-SUN FAN stack repository**](https://github.com/SiliconLabs/proprietary_wisun_stack)).
- **wisun_stack_doc**: contains the Wi-SUN FAN stack Doxygen documentation in HTML format.
- **images**: contains the readme images.

## Start your First Silicon Labs Wi-SUN Network

During this bring-up, we use **3 EFR32xG12 boards** to create a Wi-SUN network:
- One board acts as a **Wi-SUN Border Router**
- Two boards act as **Wi-SUN nodes/routers**

We refer at the two Wi-SUN nodes as **"Node 1"** and **"Node 2"**.

![Wi-SUN network](/images/wisun-network.png)

### Flash the Border Router and the Router Nodes

- Install [**Simplicity Studio 5**](https://www.silabs.com/products/development-tools/software/simplicity-studio/simplicity-studio-5)
- Download as ZIP or clone the [**Wi-SUN Application GitHub repository**](https://github.com/SiliconLabs/proprietary_wisun_applications) using the command below:

`git clone https://github.com/SiliconLabs/proprietary_wisun_applications.git`

This creates a *proprietary_wisun_applications* folder containing the necessary resources to create a Wi-SUN network.
- Open Simplicity Studio 5
- When prompted by the Installation Manager, select **"Install by technology type"**
- In the **"Select Technology Type"** panel, select **"Proprietary"** and click on **"Next"**
- In the **"Package Installation Options"** panel, directly click on **"Next"**
- Accept the License Agreement
- Simplicity Studio downloads the Proprietary Gecko SDK (takes several minutes)
- When done, click **"Close"**
- Click **"Restart"**
- Connect the EFR32xG12 boards to your PC

You can identify and differentiate the boards by using the ID available in the **"Debug Adapters"** panel as well as on the board screen.

![screen ID](/images/screen_id.png)

To Flash the **Wi-SUN border router** with the border router application:
- In the **"Launcher"** perspective, right-click on the EFR32xG12 acting as border router in the **"Debug Adapters"** panel
- Click on **"Upload application..."**
- Under **"Application image path"**, browse to the cloned *proprietary_wisun_applications* folder and select your preferred configuration under *border_router_images/EFR32MG12_BRD41xx/EFR32MG12_BRD41xx-wisun-border-router-X-Y-Z.bin*, where:
  - **X** stands for the border router **"regulatory domain"**
  - **Y** stands for the border router **"operating class"**
  - **Z** stands for the border router **"operating mode"**

To choose your preferred configuration, refer to the table below linking **regulatory domain/operating class/operating mode** to the currently **supported Wi-SUN PHYs**.

| Regulatory domain | Operating class | Operating mode | Freq band start (MHz) | Freq band end (MHz) | Region | Bitrate (kbits/s) | Channel spacing (kHz) | Antenna |
|-|-|-|-|-|-|-|-|-|
| 3 (EU) | 1 | 1a (26) | 863 | 870 | Europe | 50 | 100 | Grey |
| 3 (EU) | 3 | 1a (26) | 870 | 876 | Europe | 50 | 100 | Grey |
| 5 (IN) | 1 | 1a (26) | 865 | 867 | India | 50 | 100 | Grey |
| 7 (BZ) | 1 | 1b (27) | 902 | 928 | Brazil | 50 | 200 | Yellow |
| 1 (NA) | 1 | 1b (27) | 902 | 928 | North America | 50 | 200 | Yellow |

- Make sure you mount the antenna associated with the frequency band used on the 3 radio boards
- Click on **"OK"**

To Flash the **Wi-SUN nodes** with the Wi-SUN CLI application:
- Right-click on an EFR32xG12 acting as Wi-SUN node in the **"Debug Adapters"** panel
- Click on **"Upload application..."**
- Under **"Application image path"**, browse to the cloned *proprietary_wisun_applications* folder and select the appropriate binary file under *wisun_cli_images/EFR32MG12_BRD41xx_wisun_cli.bin*
- Click on **"OK"**
- Repeat the process for the other EFR32xG12 radio board acting as Wi-SUN node

Additionally, the Wi-SUN CLI application can be built from source as explained in the [**Wi-SUN FAN stack repository**](https://github.com/SiliconLabs/proprietary_wisun_stack).

### Connect a Console to the Wi-SUN Border Router

- Right-click on the EFR32xG12 acting as Wi-SUN border router in the **"Debug Adapter"** panel
- Click on **"Launch Console..."**
- On the newly opened console, select the **"Serial 1"** panel
- Make sure the console is connected. Check the icon in the bottom left corner of the console panel. If its status is ![console not connected](/images/console_not_connected.png) then the serial console is yet not connected. Press **"Enter"** in the console, the icon should change its status to ![console connected](/images/console_connected.png).
- Press the **"RESET"** button in the bottom right corner of the border router board

If the Wi-SUN border router application has been successfully flashed and the serial connection is established, a similar trace as the one below is output.

```
[INFO][app ]: Build: Sep 15 2020 09:16:34
[INFO][app ]: Mesh type: MBED_CONF_NSAPI_DEFAULT_MESH_TYPE
[INFO][app ]: Mbed OS version: 5.15.5
[INFO][brro]: NET_IPV6_BOOTSTRAP_AUTONOMOUS
[INFO][app ]: Using NONE backhaul driver...
[INFO][wsbs]: WS tasklet init
[INFO][wspt]: Key timers revocation lifetime: 86400, new activation time: 3600, max mismatch 3840, time to update: 82800
[INFO][brro]: Wi-SUN regulatory domain: 3, operating_class: 1, operating_mode: 26
[INFO][brro]: Wi-SUN network_name: Wi-SUN Network BD45
[INFO][wsbs]: MAC address: 00:0d:6f:ff:fe:20:bd:45
[INFO][addr]: Address added to IF 1: fe80::20d:6fff:fe20:bd45
[DBG ][rout]:                    fe80::/64  if:1 src:'Static' id:0 lifetime:infinite
[DBG ][rout]:      On-link (met 128)
[DBG ][rout]:                    ff00::/8   if:1 src:'Static' id:0 lifetime:infinite
[DBG ][rout]:      On-link (met 192)
[INFO][brro]: mesh0 bootstrap ongoing..
[INFO][wsbs]: Discovery start
[INFO][wsbs]: Border router start network
[INFO][fhss]: fhss Configuration set, UC channel: 2, BC channel: 12, UC CF: 2, BC CF: 2, channels: BC 69 UC 69, uc dwell: 255, bc dwell: 255, bc interval: 1020, bsi:0
[INFO][mlme]: RF config update:
[INFO][mlme]: Frequency(ch0): 863100000Hz
[INFO][mlme]: Channel spacing: 100000Hz
[INFO][mlme]: Datarate: 50000bps
[INFO][mlme]: Number of channels: 69
[INFO][mlme]: Modulation: 4
[INFO][mlme]: Modulation index: 0
[INFO][mcth]: Initialized CCA threshold: 69, -60, -60, -100
[INFO][fhss]: TX slot length: 127ms
[INFO][wspa]: GTK install new index: 0, lifetime: 2592000 system time: 0
[INFO][wspc]: GTK hash set 29:b6:4a:ff:f1:5a:9f:93 00:00:00:00:00:00:00:00 00:00:00:00:00:00:00:00 00:00:00:00:00:00:00:00
[INFO][wspc]: NW key set: 0, hash: 29:b6:4a:ff:f1:5a:9f:93
[INFO][wspc]: NW name: Wi-SUN Network BD45
[INFO][wspc]: NW name: 57:69:2d:53:55:4e:20:4e:65:74:77:6f:72:6b:20:42:44:34:35
[INFO][wspc]: GTK: 9f:7b:a5:9d:52:e0:51:7d:88:21:65:b6:36:09:d8:50
[INFO][wspc]: GAK: 23:94:39:06:65:ff:2e:27:5a:16:f0:e1:02:1e:00:b2
[INFO][wspc]: NW send key index set: 1
[INFO][wsbs]: operation start
[INFO][wsbs]: Routing ready
[INFO][brro]: Wisun bootstrap ready
```

- At this step, **retrieve the Wi-SUN network name and the border router IPv6 address** from the traces. You can also verify that the Wi-SUN regulatory domain, operating class and operating mode match the configuration you want to use. Finally, if you want to export Wi-SUN radio traces to Wireshark, you need the **"GAK"** key and **"key index set"**. More information in the [**Export Wi-SUN Traces to Wireshark**](#export-wi-sun-traces-to-wireshark) section.

In the trace above, the network name is `Wi-SUN Network BD45`, the border router IP address is `fe80::20d:6fff:fe20:bd45`, the GAK key is `23:94:39:06:65:ff:2e:27:5a:16:f0:e1:02:1e:00:b2` and the key index set is `1`.

If you prefer to use another serial terminal, the UART port settings are **115200 bps, 8 data bits, 1 stop bit and no parity**.

### Connect a Console to the Wi-SUN CLI Application

- Right-click on an EFR32xG12 acting as Wi-SUN node in the **"Debug Adapter"** panel
- Click on **"Launch Console..."**
- On the newly opened console, select the **"Serial 1"** panel
- Press **"Enter"**

If the Wi-SUN CLI application has been successfully flashed and the serial connection is established, a chevron is output indicating the CLI is ready to receive a command:
```
>
```
- Type `wisun help` to list the available Wi-SUN commands
- Type `wisun get wisun` to list the available Wi-SUN parameters
- Repeat the process for the other EFR32xG12 radio board acting as Wi-SUN node

If you prefer to use another serial terminal, the UART port settings are **115200 bps, 8 data bits, 1 stop bit and no parity**.

### Configure the Wi-SUN CLI Nodes

In this section, we use the Wi-SUN CLI interface to configure a Wi-SUN node. This step has to be reproduced on each Wi-SUN node. A first step is to configure the Wi-SUN network name.
- `wisun set wisun.network_name [network name output by the border router]`. The border router network name can be retrieved in the section [**Connect a Console to the Wi-SUN Border Router**](#connect-a-console-to-the-wi-sun-border-router).

For example:
```
> wisun set wisun.network_name "Wi-SUN Network 20FE"
>
```
Depending on the border router binary used in the section above, the Wi-SUN nodes have to be configured with a matching PHY configuration. To do so, the Wi-SUN CLI application provides three configuration APIs:
- `wisun set wisun.regulatory_domain [parameter]` should match **X** in the border router file name (EFR32MG12_BRD41xx-wisun-border-router-**X**-Y-Z.bin)
- `wisun set wisun.operating_class [parameter]` should match **Y** in the border router file name (EFR32MG12_BRD41xx-wisun-border-router-X-**Y**-Z.bin)
- `wisun set wisun.operating_mode [parameter]` should match **the decimal value of Z expressed in hexadecimal in the border router name** (EFR32MG12_BRD41xx-wisun-border-router-X-Y-**Z**.bin). For example:
  - if the border router Z value is '1a', `wisun set wisun.operating_mode 26` should be configured
  - if the border router Z value is '1b', `wisun set wisun.operating_mode 27` should be configured
  - ...

You can verify the commands have been taken into account by using the "get" commands below:
- `wisun get wisun.network_name`
- `wisun get wisun.regulatory_domain`
- `wisun get wisun.operating_class`
- `wisun get wisun.operating_mode`

or print out all the Wi-SUN parameters:
- `wisun get wisun`

Once configured with the correct Wi-SUN network name and Wi-SUN PHY configuration, the parameters can be saved in the EFR32 Flash memory:

`wisun save`

The current "network_name", "regulatory_domain", "operating_class", "operating_mode" and other Wi-SUN parameters are retained on reset.

### Connect the Nodes to the Network

Once the Wi-SUN node is correctly configured, it can be connected to the Wi-SUN border router. Enter the following command in the Wi-SUN CLI to start the connection process:

`wisun connect`

After several hundreds of seconds (100 s to 500 s), a connection notification is output by the Wi-SUN CLI with the IPv6 address assigned to the Wi-SUN node.
```
> wisun connect
[Connecting to "Wi-SUN Network"]
> [Connected: 135 s]
[IPv6 address: fd00:7283:7e00:0:20d:6fff:fe20:b6f9]
```

The node IP address is `fd00:7283:7e00:0:20d:6fff:fe20:b6f9`. The node has been successfully added to the Wi-SUN network. Follow this step for each Wi-SUN node you want to add to the network.

### Ping through the Wi-SUN Network

Once the two boards are connected to the border router, retrieve their IP addresses. In this section, we use the Wi-SUN Network below:

- **Border Router: [IPv6 address:fd00:6172:6d00:0:fd6f:d00:95bd:20fe]**
- **Node 1: [IPv6 address: fd00:7283:7e00:0:20d:6fff:fe20:bd45]**
- **Node 2: [IPv6 address: fd00:7283:7e00:0:20d:6fff:fe20:b6f9]**

If you want to retrieve the border router IP address, type the following command:

`wisun get wisun.border_router`

To ping the **Border Router** from **Node 1**, use the command below on **Node 1**.

`wisun ping [Border Router IPv6 address]`

If the ping is successful, the Wi-SUN CLI on **Node 1** outputs the following trace.
```
> wisun ping fd00:6172:6d00:0:fd6f:d00:95bd:20fe
PING fd00:6172:6d00:0:fd6f:d00:95bd:20fe: 40 data bytes
> 40 bytes from fd00:6172:6d00:0:fd6f:d00:95bd:20fe: icmp_seq=1 time=47.534 ms
```

To ping the **Node 2** from **Node 1**, use the command below on **Node 1**.

`wisun ping [Node 2 IPv6 address]`

If the ping is successful, the Wi-SUN CLI on **Node 1** outputs the following trace.
```
> wisun ping fd00:7283:7e00:0:20d:6fff:fe20:b6f9
PING fd00:7283:7e00:0:20d:6fff:fe20:b6f9: 40 data bytes
> 40 bytes from fd00:7283:7e00:0:20d:6fff:fe20:b6f9: icmp_seq=1 time=58.482 ms
```

## Use the Wi-SUN Stack Socket API

### Send TCP Packets through the Wi-SUN Network

Using the same Network configuration as in the previous section:

- **Border Router: [IPv6 address:fd00:6172:6d00:0:fd6f:d00:95bd:20fe]**
- **Node 1: [IPv6 address: fd00:7283:7e00:0:20d:6fff:fe20:bd45]**
- **Node 2: [IPv6 address: fd00:7283:7e00:0:20d:6fff:fe20:b6f9]**

To use the TCP socket API, first on **Node 2** enter:

`wisun tcp_server [local port]`

For example, to use the TCP socket 80:
```
> wisun tcp_server 80
[Listening: 3]
```
On **Node 2**, the **Socket ID** is **3**.

On **Node 1**, enter:

`wisun tcp_client [Node 2 IPv6 address] [local port]` with "local port" matching the port previously configured on **Node 2**.
```
> wisun tcp_client fd00:7283:7e00:0:20d:6fff:fe20:b6f9 80
[Opening: fd00:7283:7e00:0:20d:6fff:fe20:b6f9 (80): 3]
> [Opened: 3]
```
On **Node 1**, the **Socket ID** is **3**.
On the **Node 2** side, a trace is output to confirm the successful socket connection.
```
> [Socket connection available: 3]
[Accepted fd00:7283:7e00:0:20d:6fff:fe20:bd45 (63516): 4]
```
At this point, you can use the `wisun socket_list` command to list the available sockets:
```
> wisun socket_list
!  ##  Type        Info
#   3  TCP client  fd00:7283:7e00:0:20d:6fff:fe20:b6f9 (80) (ACTIVE)
!
```

To send data from **Node 1** to **Node 2**, use the command below:

`wisun socket_write [Socket ID] [Message]`

On **Node 1**, enter:
```
> wisun socket_write 3 "Hello Node 2"
[Wrote 12 bytes]
> [Data sent: 3,2048]
```
On **Node 2**, the 12 bytes are successfully received on the socket.
```
> [Data from fd00:7283:7e00:0:20d:6fff:fe20:bd45 (63516): 4,12]
```

### Send UDP Packets through the Wi-SUN Network

Using the same Network configuration as in the previous section:

- **Border Router: [IPv6 address:fd00:6172:6d00:0:fd6f:d00:95bd:20fe]**
- **Node 1: [IPv6 address: fd00:7283:7e00:0:20d:6fff:fe20:bd45]**
- **Node 2: [IPv6 address: fd00:7283:7e00:0:20d:6fff:fe20:b6f9]**

To use the UDP socket API, first on **Node 2** enter:

`wisun udp_server [local port]`

For example, to use the UDP socket 90:
```
> wisun udp_server 90
[Listening: 3]
```
On **Node 2**, the **Socket ID** is **3**.

On **Node 1**, enter:

`wisun udp_client [Node 2 IPv6 address] [local port]` with "local port" matching the port previously configured on **Node 2**.
```
> wisun udp_client fd00:7283:7e00:0:20d:6fff:fe20:b6f9 90
[Opened: 3]
```
On **Node 1**, the **Socket ID** is **3**.

To send data from **Node 1** to **Node 2**, use the command below:

`wisun socket_write [Socket ID] [Message]`

On **Node 1**, enter:
```
> wisun socket_write 3 "Hello Node 2"
[Wrote 12 bytes]
```
On **Node 2**, the 12 bytes are successfully received on the socket.
```
> [Data from fd00:7283:7e00:0:20d:6fff:fe20:bd45 (54880): 4,12]
```

### Send Multicast Packets through the Wi-SUN Network

In addition to send data to unicast addresses, the Wi-SUN socket API can also send data to predefined multicast groups listed below.

```
ff01::1 All Nodes Address (Interface-Local) (implicit)
ff02::1 All Nodes Address (Link-Local) (implicit)
ff03::1 All Nodes Address (Realm-Local)
ff01::2 All Routers Address (Interface-Local)
ff02::2 All Routers Address (Link-Local)
ff03::2 All Routers Address (Realm-Local)
ff05::2 All Routers Address (Site-Local)
ff02::1a All RPL Nodes Address (Link-Local) [RFC6550]
ff02::1:ffxx:xxxx Solicited-Node Address (Link-Local) [RFC4291]
ff03::fc All MPL Forwarders Address (Realm-Local) [RFC7731]
```

All you need to do to send a multicast packet through the network is to have several node in the network listening to a port number and from another node send a packet using one of the addresses above.  

## Going Further in the Silicon Labs Wi-SUN Stack

### Access RTT traces in the Wi-SUN CLI application

The Wi-SUN CLI application provides a logging mechanism based on the Segger RTT feature to allow a finer tracing capability. To access the Wi-SUN CLI application RTT traces:
- Install [**J-Link RTT Viewer**](https://www.segger.com/products/debug-probes/j-link/tools/rtt-viewer/)
- Open JLink RTT Viewer
- In the **"Configuration"** panel, select **"USB"** in the **"Connection to J-Link"** section
- In the **"Specify Target Device"** list, select the **"EFR32MG12PXXXF1024"**
- In the **"Target Interface & Speed"** panel, select **"JTAG"** and **"4000 kHz"**
- In the **"RTT Control Block"** panel, select **"Auto Detection"**
- Click on **"OK"**
- If you have several boards connected, a list appears.
- Select a board running the Wi-SUN CLI application
- Click on **"OK"**

A terminal opens and the Wi-SUN CLI application traces are output as shown below.

```
[DBG ][wisun]: net_init_core: 0
[DBG ][wisun]: sli_wisun_task_event_handler_id: 2
[DBG ][SLRF]: sli_wisun_driver_register()
[DBG ][SLRF]: sli_wisun_driver_register() - driver_id: 0
[DBG ][SLRF]: rf_address_write: PHY_MAC_64BIT: 00:0d:6f:ff:fe:20:bd:95
[DBG ][mlme]: SW-MAC driver support rf extension 50000 symbol/seconds  20 us symbol time length
[DBG ][swm ]: Set MAC mode to IEEE 802.15.4-2011, MTU size: 127
[DBG ][SLRF]: rf_address_write: PHY_MAC_64BIT: 00:0d:6f:ff:fe:20:bd:95
[DBG ][wisun]: arm_nwk_interface_lowpan_init: 1
```

These logs can be used to report an issue to Silicon Labs.

### Export Wi-SUN Traces to Wireshark

Simplicity Studio's Network Analyzer enables debugging of complex wireless systems on the EFR32 family. It can be taken advantage of when running Silicon Labs Wi-SUN FAN stack.

> The Network Analyzer does not yet include a native Wi-SUN protocol analyzer. However, it can be used to export traces to another analyzer like Wireshark.

To export Wi-SUN traces with the Network Analyzer to Wireshark, follow the steps described below:
- Install [**Wireshark**](https://www.wireshark.org/download.html)
- Right-click on an EFR32xG12 running the Wi-SUN CLI application in the **"Debug Adapter"** panel
- Click on **"Start capture"**
- A new **"Live"** window opens. It traces packets sent and received by the Wi-SUN node
- Once you have traced the communication, click on the export icon ![export icon](/images/export.png)
- In the **"Select export format:"** list, select the **"PCAP NG exporter"** option
- Enter a path and a file name where to store the trace in the **"Output file:"** field
- In the **"Export mode:"** field, select the **"Wi-SUN"** option
- Click on **"OK"**
- Open the new file with Wireshark

The file should automatically be analyzed as a Wi-SUN exchange by Wireshark. The communication is initially encrypted thanks to the Wi-SUN encryption protocol. To decrypt the communications, you need the GAK key and key index set information. They can be retrieved during the border router boot as described in the [**Connect a Console to the Wi-SUN Border Router**](#connect-a-console-to-the-wi-sun-border-router) section.
- Click on **"Edit"**
- Click on **"Preferences..."**
- Extend the **"Protocols"** list and select **"IEEE 802.15.4**
- In the **"IEEE 802.15.4 Low-Rate Wireless PAN"** panel, next to **"Decryption Keys"**, click on **"Edit..."**
- In the **"Keys"** window, click on **"+"**
- Enter the GAK key in **"Decryption key"** field and the key index set in the **"Decryption key index"**
- Click on **"OK"**

Wireshark is now able to decrypt the traces and the higher level protocols (ICMP, TCP, UDP...). Below is an example of traces showing a router pinging its border router.

![wireshark](/images/wireshark_traces.png)
