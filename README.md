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

This repository contains resources to use and evaluate Silicon Labs Wi-SUN FAN stack.

## Content

- **border_router_image**: contains Wi-SUN border router images for different EFR32xG12 radio board references and different Wi-SUN PHYs
- **wisun_cli_image**: contains Wi-SUN CLI application images for different EFR32xG12 radio board references
- **wisun_stack_doc**: contains the Wi-SUN FAN stack Doxygen documentation in HTML format

## Start your First Silicon Labs Wi-SUN Network

During this bring-up, we use **3 EFR32xG12 boards** to create a Wi-SUN network:
- One board acts as a **Wi-SUN Border Router**
- Two boards act as **Wi-SUN nodes/routers**

### Flash the Border Router and the Router Node

- Install [**Simplicity Studio 5**](https://www.silabs.com/products/development-tools/software/simplicity-studio/simplicity-studio-5) and [**Git**](https://git-scm.com/) on your machine
- Clone the [**Wi-SUN Application GitHub repository**](https://github.com/SiliconLabs/proprietary_wi-sun_applications) using the command below:

`git clone https://github.com/SiliconLabs/proprietary_wi-sun_applications.git`

This creates a *proprietary_wi-sun_applications* folder containing the necessary resources to create a Wi-SUN network.

- Open Simplicity Studio 5
- Connect the EFR32xG12 boards to your PC

To Flash the **Wi-SUN border router** with the border router application:
- In the **"Launcher"** perspective, right-click on the EFR32xG12 acting as border router in the **"Debug Adapter"** panel
- Click on **"Upload application..."**
- Under **"Application image path"**, browse to the cloned **proprietary_wi-sun_applications** folder and select your preferred configuration under **border_router_image/EFR32MG12_BRD41xx/EFR32MG12_BRD41xx-wisun-border-router-x-x-x.bin**
- Click on **"OK"**

To Flash the **Wi-SUN nodes** with Wi-SUN CLI application:
- Right-click on an EFR32xG12 acting as Wi-SUN node in the **"Debug Adapter"** panel
- Click on **"Upload application..."**
- Under **"Application image path"**, browse to the cloned *proprietary_wi-sun_applications* folder and select the appropriate binary file under **wisun_cli_image/EFR32MG12_BRD41xx_wisun_cli.bin**
- Click on **"OK"**
- Repeat the process for the other EFR32xG12 acting as Wi-SUN node

Additionally, the Wi-SUN CLI application can be built from source as explained in [**the dedicated section**](#build-the-wisun-cli-application-from-the-source-code).

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
- `w s wisun.regulatory_domain [parameter]` should macth X in the border router file name (*EFR32MG12_BRD4163-wisun-border-router-X-Y-Z.bin*)
- `w s wisun.operating_class [parameter]` should match Y in the border router file name (*EFR32MG12_BRD4163-wisun-border-router-X-Y-Z.bin*)
- `w s wisun.operating_mode [parameter]` should match Z in the border router file name (*EFR32MG12_BRD4163-wisun-border-router-X-Y-Z.bin*)

You can make sure the commands have been taken into account by using the "get" command below:
- `w g wisun.regulatory_domain`
- `w g wisun.operating_class`
- `w g wisun.operating_mode`

Below is a table of the currently supported Wi-SUN PHYs.

| Regulatory domain | Operating class | Operating mode | Freq band start (MHz) | Freq band end (MHz) | Region | Bitrate (kbits/s) | Channel spacing (kHz) |
|-|-|-|-|-|-|-|-|
| 3 | 1 | 1a | 863 | 870 | Europe | 50 | 100 |
| 3 | 2 | 2a | 863 | 870 | Europe | 100 | 200 |
| 3 | 2 | 3 | 863 | 870 | Europe | 150 | 200 |
| 3 | 3 | 1a | 870 | 876 | Europe | 50 | 100 |
| 3 | 4 | 2a | 870 | 876 | Europe | 100 | 200 |
| 3 | 4 | 3 | 870 | 876 | Europe | 150 | 200 |

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

The node has been successfully added to the Wi-SUN network.

### Ping through the Wi-SUN Network

Once the two boards are connected to the border router, retrieve their IP addresses. In this section, we use the Wi-SUN Network below:

- **Border Router: [IPv6 address:fd00:6172:6d00:0:fd6f:d00:95bd:20fe]**
- **Node 1: [IPv6 address: fd00:7283:7e00:0:20d:6fff:fe20:bd45]**
- **Node 2: [IPv6 address: fd00:7283:7e00:0:20d:6fff:fe20:b6f9]**

To ping the **Border Router** from **Node 1**, use the command below on **Node 1**.

`w ping [Border Router IPv6 address]`

If the ping is successful, the Wi-SUN CLI output the following trace.
```
> w ping fd00:6172:6d00:0:fd6f:d00:95bd:20fe
PING fd00:6172:6d00:0:fd6f:d00:95bd:20fe: 40 data bytes
> 40 bytes from fd00:6172:6d00:0:fd6f:d00:95bd:20fe: icmp_seq=1 time=47.534 ms
```

To ping the **Node 2** from **Node 1**, use the command below on **Node 1**.

`w ping [Node 2 IPv6 address]`

If the ping is successful, the Wi-SUN CLI output the following trace.
```
> w ping fd00:7283:7e00:0:20d:6fff:fe20:b6f9
PING fd00:7283:7e00:0:20d:6fff:fe20:b6f9: 40 data bytes
> 40 bytes from fd00:7283:7e00:0:20d:6fff:fe20:b6f9: icmp_seq=1 time=58.482 ms
```

### Send TCP/UDP Packets through the Wi-SUN Network

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

`w tcp_client [Node 2 IPv6 address] [local port]`
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
On **Node 2**, the 12 bytes are received on the socket.
```
> [Data from fd00:7283:7e00:0:20d:6fff:fe20:bd45 (63516): 4,12]
```

## Build the Wi-SUN CLI Application from the Source Code

### Install the Required Software

- Install [**Simplicity Studio 5**](https://www.silabs.com/products/development-tools/software/simplicity-studio/simplicity-studio-5) and [**Git**](https://git-scm.com/) on your machine
- Open Simplicity Studio 5
- When prompted by the Installation Manager, select **"Install by technology type"**
- In the **"Select Technology Type"** panel, select **"Proprietary"** and click **"Next"**
- **"Package Installation Options"**, **"Next"**
- Accept the License Agreement
- Simplicity Studio download the Proprietary Gecko SDK (takes several minutes)
- When done, click **"Close"**
- Click **"Restart"**
- Once Simplicity Studio is reopened, close it again

### Retrieve and Include the Wi-SUN Stack and Applications to the Gecko SDK

- Under the default path *SiliconLabs\SimplicityStudio\v5\developer\sdks\gecko_sdk_suite\v3.0\protocol* (default path), clone the [**Wi-SUN stack GitHub repository**](https://github.com/SiliconLabs/proprietary_wi-sun_stack) using the command below:

`git clone https://github.com/SiliconLabs/proprietary_wi-sun_stack.git`

The command creates a "wisun" folder containing the Wi-SUN stack and Wi-SUN CLI application.

- Under *wisun/wisun/gsdk-integration*, run *gsdk-setup.sh*
- Restart Simplicity Studio

### Create a Wi-SUN CLI Application in your Workspace

- Click on *File/New/Project...*
- In **"Select a wizard"**, select */Simplicity Studio/Silicon Labs MCU Project*
- Select in **"Boards"** the EFR32 radio board you want to use (e.g., BRD4170A)
- Under **"SDK"**, make sure the Gecko SDK lists the Wi-SUN stack and click on **"Next"**
- Select **"Example"** and click on **"Next"**
- In the examples list, select **"Wi-SUN CLI"** under **"Wi-SUN Examples"** and click on **"Next"**
- Optionally, you can change the **"Project name"**. Click on **"Finish"**

Simplicity Studio loads the Wi-SUN CLI example in the workspace.

### Build and Load the Wi-SUN CLI Application

- Connect a WSTK board with the appropriate EFR32xG12 radio board
- Select the **"wisun_cli"** project (default name)
- Click on the **"hammer"** icon to build the project
- Make sure the build is successful
- Click on the **"bug"** icon to flash the board and start a debug session
- Once in the **"Debug"** view, click on the **"green arrow"** icon to start the application
