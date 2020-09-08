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

This repository contains resources to evaluate and develop with Silicon Labs Wi-SUN FAN stack.

## Content

- **border_router_image**: contains Wi-SUN border router images for different EFR32xG12 radio board references and different Wi-SUN PHYs.
- **wisun_cli_image**: contains Wi-SUN CLI application images for different EFR32xG12 radio board references. It acts as a Wi-SUN router node in a network.
- **wisun_stack_doc**: contains the Wi-SUN FAN stack Doxygen documentation in HTML format.

## Start your First Silicon Labs Wi-SUN Network

During this bring-up, we use **3 EFR32xG12 boards** to create a Wi-SUN network:
- One board acts as a **Wi-SUN Border Router**
- Two boards act as **Wi-SUN nodes/routers**

### Flash the Border Router and the Router Nodes

- Install [**Simplicity Studio 5**](https://www.silabs.com/products/development-tools/software/simplicity-studio/simplicity-studio-5) and [**Git**](https://git-scm.com/) on your machine
- Clone the [**Wi-SUN Application GitHub repository**](https://github.com/SiliconLabs/proprietary_wisun_applications) using the command below:

`git clone https://github.com/SiliconLabs/proprietary_wisun_applications.git`

This creates a *proprietary_wisun_applications* folder containing the necessary resources to create a Wi-SUN network.

- Open Simplicity Studio 5
- Connect the EFR32xG12 boards to your PC

To Flash the **Wi-SUN border router** with the border router application:
- In the **"Launcher"** perspective, right-click on the EFR32xG12 acting as border router in the **"Debug Adapter"** panel
- Click on **"Upload application..."**
- Under **"Application image path"**, browse to the cloned *proprietary_wisun_applications* folder and select your preferred configuration under *border_router_image/EFR32MG12_BRD41xx/EFR32MG12_BRD41xx-wisun-border-router-x-x-x.bin*
- Click on **"OK"**

To Flash the **Wi-SUN nodes** with Wi-SUN CLI application:
- Right-click on an EFR32xG12 acting as Wi-SUN node in the **"Debug Adapter"** panel
- Click on **"Upload application..."**
- Under **"Application image path"**, browse to the cloned *proprietary_wisun_applications* folder and select the appropriate binary file under *wisun_cli_image/EFR32MG12_BRD41xx_wisun_cli.bin*
- Click on **"OK"**
- Repeat the process for the other EFR32xG12 acting as Wi-SUN node

Additionally, the Wi-SUN CLI application can be built from source as explained in [**this dedicated section**](#build-the-wi-sun-cli-application-from-the-source-code).

### Connect a Terminal to the Wi-SUN CLI Application

- Install on your machine a serial terminal application like [**Putty**](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) or [**Tera Term**](https://osdn.net/projects/ttssh2/releases/) on your machine
- Configure the UART port settings to **115200 bps, 8 data bits, 1 stop bit and no parity**
- Open a connection to the COM port associated with an EFR32xG12 board running the Wi-SUN CLI application
- By pressing on the "RESET" button on the targeted board, a chevron is output indicating the CLI is ready to receive a command
```
>
```
- Type `wisun help` to access a list of the available Wi-SUN commands

### Configure the Wi-SUN PHY

Depending on the border router binary used in the section above, the Wi-SUN nodes have to be configured with a matching PHY configuration. To do so, the Wi-SUN CLI application provides three configuration APIs:
- `w s wisun.regulatory_domain [parameter]` should match X in the border router file name (EFR32MG12_BRD4163-wisun-border-router-**X**-Y-Z.bin)
- `w s wisun.operating_class [parameter]` should match Y in the border router file name (EFR32MG12_BRD4163-wisun-border-router-X-**Y**-Z.bin)
- `w s wisun.operating_mode [parameter]` should match Z in the border router file name (EFR32MG12_BRD4163-wisun-border-router-X-Y-**Z**.bin)

> Make sure the operating mode parameter is prefixed by '0x' in case the value has an hexadecimal format.

You can verify the commands have been taken into account by using the "get" commands below:
- `w g wisun.regulatory_domain`
- `w g wisun.operating_class`
- `w g wisun.operating_mode`

Below is a table of the currently supported Wi-SUN PHYs.

| Regulatory domain | Operating class | Operating mode | Freq band start (MHz) | Freq band end (MHz) | Region | Bitrate (kbits/s) | Channel spacing (kHz) |
|-|-|-|-|-|-|-|-|
| 3 (EU) | 1 | 0x1a | 863 | 870 | Europe | 50 | 100 |
| 3 (EU) | 2 | 0x2a | 863 | 870 | Europe | 100 | 200 |
| 3 (EU) | 2 | 0x03 | 863 | 870 | Europe | 150 | 200 |
| 3 (EU) | 3 | 0x1a | 870 | 876 | Europe | 50 | 100 |
| 3 (EU) | 4 | 0x2a | 870 | 876 | Europe | 100 | 200 |
| 3 (EU) | 4 | 0x03 | 870 | 876 | Europe | 150 | 200 |

Once configured with your preferred Wi-SUN PHY configuration, the configuration can be saved in the EFR32 Flash memory:

`wisun save`

The current "network_name", "regulatory_domain", "operating_class", "operating_mode" and other parameters are retained on reset.

### Connect the Nodes to the Network

Once the Wi-SUN node PHY is correctly configured, the node can be connected to the Wi-SUN border router. Enter the following command in the Wi-SUN CLI to start the connection process:

`w c` alias of `wisun connect`

After several hundreds of seconds (100s to 500s), a connection notification is output by the Wi-SUN CLI.
```
> w c
[Connecting to "Wi-SUN Network"]
> [Connected: 135 s]
[IPv6 address: fd00:7283:7e00:0:20d:6fff:fe20:b6f9]
```

The node has been successfully added to the Wi-SUN network. Follow this step for each Wi-SUN node you want to add to the network.

### Ping through the Wi-SUN Network

Once the two boards are connected to the border router, retrieve their IP addresses. In this section, we use the Wi-SUN Network below:

- **Border Router: [IPv6 address:fd00:6172:6d00:0:fd6f:d00:95bd:20fe]**
- **Node 1: [IPv6 address: fd00:7283:7e00:0:20d:6fff:fe20:bd45]**
- **Node 2: [IPv6 address: fd00:7283:7e00:0:20d:6fff:fe20:b6f9]**

To ping the **Border Router** from **Node 1**, use the command below on **Node 1**.

`w ping [Border Router IPv6 address]`

If the ping is successful, the Wi-SUN CLI on **Node 1** output the following trace.
```
> w ping fd00:6172:6d00:0:fd6f:d00:95bd:20fe
PING fd00:6172:6d00:0:fd6f:d00:95bd:20fe: 40 data bytes
> 40 bytes from fd00:6172:6d00:0:fd6f:d00:95bd:20fe: icmp_seq=1 time=47.534 ms
```

To ping the **Node 2** from **Node 1**, use the command below on **Node 1**.

`w ping [Node 2 IPv6 address]`

If the ping is successful, the Wi-SUN CLI on **Node 1** output the following trace.
```
> w ping fd00:7283:7e00:0:20d:6fff:fe20:b6f9
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

`w tcp_server [local port]`

For example, to use the TCP socket 80:
```
> w tcp_server 80
[Listening: 3]
```
On **Node 2**, the **Socket ID** is **3**.

On **Node 1**, enter:

`w tcp_client [Node 2 IPv6 address] [local port]` with "local port" matching the port previously configured on **"Node 2"**.
```
> w tcp_client fd00:7283:7e00:0:20d:6fff:fe20:b6f9 80
[Opening: fd00:7283:7e00:0:20d:6fff:fe20:b6f9 (80): 3]
> [Opened: 3]
```
On **Node 1**, the **Socket ID** is **3**.
On the **Node 2** side, a trace is output to confirm the successful socket connection.
```
> [Socket connection available: 3]
[Accepted fd00:7283:7e00:0:20d:6fff:fe20:bd45 (63516): 4]
```
To send data from **Node 1** to **Node 2**, use the command below:

`w socket_write [Socket ID] [Message]`

On **Node 1**, enter:
```
> w socket_write 3 "Hello Node 2"
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

`w udp_server [local port]`

For example, to use the UDP socket 80:
```
> w udp_server 80
[Listening: 3]
```
On **Node 2**, the **Socket ID** is **3**.

On **Node 1**, enter:

`w udp_client [Node 2 IPv6 address] [local port]` with "local port" matching the port previously configured on **"Node 2"**.
```
> w udp_client fd00:7283:7e00:0:20d:6fff:fe20:b6f9 80
[Opened: 3]
```
On **Node 1**, the **Socket ID** is **3**.

To send data from **Node 1** to **Node 2**, use the command below:

`w socket_write [Socket ID] [Message]`

On **Node 1**, enter:
```
> w socket_write 3 "Hello Node 2"
[Wrote 12 bytes]
```
On **Node 2**, the 12 bytes are successfully received on the socket.
```
> [Data from fd00:7283:7e00:0:20d:6fff:fe20:bd45 (54880): 4,12]
```
