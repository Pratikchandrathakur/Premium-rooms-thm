# Task 1 — Introduction

Wireshark is an open-source, cross-platform network packet analyser tool capable of sniffing and investigating live traffic and inspecting packet captures (PCAP). It is commonly used as one of the best packet analysis tools. In this room, we will look at the basics of Wireshark and use it to perform fundamental packet analysis.

## Learning Objectives

- Navigate and configure Wireshark
- Inspect packets and discover information from the different layers of TCP/IP
- Apply display filters

## Prerequisites

- Networking Module

## Environment Setup

Press the **Start Machine** button below to start the virtual machine.

> **Start Machine**

The machine will start in Split-Screen view. If it is not visible, use the blue **Show Split View** button at the top of the page.

There are two capture files given in the VM. You can use the `http1.pcapng` file to simulate the actions shown in the screenshots. Please note that you need to use the `Exercise.pcapng` file to answer the questions.

## Answer the questions below

**Which file is used to simulate the screenshots?**  
`http1.pcapng`

**Which file is used to answer the questions?**  
`Exercise.pcapng`

---

# Task 2 — Tool Overview

## Use Cases

Wireshark is one of the most potent traffic analyser tools available in the wild. There are multiple purposes for its use:

- Detecting and troubleshooting network problems, such as network load failure points and congestion.
- Detecting security anomalies, such as rogue hosts, abnormal port usage, and suspicious traffic.
- Investigating and learning protocol details, such as response codes and payload data.

> **Note:** Wireshark is not an Intrusion Detection System (IDS). It only allows analysts to discover and investigate the packets in depth. It also doesn't modify packets; it reads them. Hence, detecting any anomaly or network problem highly relies on the analyst's knowledge and investigation skills.

## GUI and Data

Wireshark GUI opens with a single all-in-one page, which helps users investigate the traffic in multiple ways. At first glance, five sections stand out.

| Section | Description |
|---|---|
| Toolbar | The main toolbar contains multiple menus and shortcuts for packet sniffing and processing, including filtering, sorting, summarising, exporting and merging. |
| Display Filter Bar | The main query and filtering section. |
| Recent Files | List of the recently investigated files. You can recall listed files with a double-click. |
| Capture Filter and Interfaces | Capture filters and available sniffing points (network interfaces). The network interface is the connection point between a computer and a network. The software connection (e.g., `lo`, `eth0` and `ens33`) enables networking hardware. |
| Status Bar | Tool status, profile and numeric packet information. |

The picture below shows Wireshark's main window. The sections explained in the table are highlighted.

Now open Wireshark and follow along with the walkthrough.
<img width="1558" height="735" alt="image" src="https://github.com/user-attachments/assets/e0792973-647a-402b-8d28-5ca098fcbdc3" />


## Loading PCAP Files

The above picture shows Wireshark's empty interface. The only available information is the recently processed `http1.pcap` file. Let's load that file and see Wireshark's detailed packet presentation. Note that you can also use the **File** menu, dragging and dropping the file, or double-clicking on the file to load a pcap.
<img width="1200" height="460" alt="image" src="https://github.com/user-attachments/assets/b33a556c-3149-43bf-ac81-bd355ac4b4df" />


Now, we can see the processed filename, detailed number of packets and packet details. Packet details are shown in three different panes, which allow us to discover them in different formats.

| Pane | Description |
|---|---|
| Packet List Pane | Summary of each packet (source and destination addresses, protocol, and packet info). You can click on the list to choose a packet for further investigation. Once you select a packet, the details will appear in the other panels. |
| Packet Details Panel | Detailed protocol breakdown of the selected packet. |
| Packet Bytes Pane | Hex and decoded ASCII representation of the selected packet. It highlights the packet field depending on the clicked section in the details pane. |

## Colouring Packets

Along with quick packet information, Wireshark also colour packets in order of different conditions and the protocol to spot anomalies and protocols in captures quickly (this explains why almost everything is green in the given screenshots). This glance at packet information can help track down exactly what you're looking for during analysis. You can create custom colour rules to spot events of interest by using display filters, and we will cover them in the next room. Now let's focus on the defaults and understand how to view and use the represented data details.

Wireshark has two types of packet colouring methods: temporary rules that are only available during a program session and permanent rules that are saved under the preference file (profile) and available for the next program session. You can use the **right-click menu** or **View → Coloring Rules** menu to create permanent colouring rules. The **Colourise Packet List** menu activates/deactivates the colouring rules. Temporary packet colouring is done with the **right-click menu** or **View → Conversation Filter** menu, which is covered in TASK-5.

The default permanent colouring is shown below.
<img width="1590" height="710" alt="image" src="https://github.com/user-attachments/assets/2c52e124-6e6f-414a-af5f-8e369d15b6d9" />


## Traffic Sniffing

You can use the blue **shark button** to start network sniffing (capturing traffic), the red button will stop the sniffing, and the green button will restart the sniffing process. The status bar will also provide the used sniffing interface and the number of collected packets.
<img width="1809" height="813" alt="image" src="https://github.com/user-attachments/assets/d9d60e3a-0f97-4b8c-a9d1-233d7ebb011e" />


## Merge PCAP Files

Wireshark can combine two pcap files into one single file. You can use the **File → Merge** menu path to merge a pcap with the processed one. When you choose the second file, Wireshark will show the total number of packets in the selected file. Once you click **open**, it will merge the existing pcap file with the chosen one and create a new pcap file. Note that you need to save the **merged** pcap file before working on it.

![66c44fd9733427ea1181ad58-1760973498209](https://github.com/user-attachments/assets/7bdca779-64f9-4775-959e-9d815b1b3026)


## View File Details

Knowing the file details is helpful. Especially when working with multiple pcap files, sometimes you will need to know and recall the file details (File hash, capture time, capture file comments, interface and statistics) to identify the file, classify and prioritise it. You can view the details by following **Statistics → Capture File Properties** or by clicking the **pcap icon located on the left bottom**.
![66c44fd9733427ea1181ad58-1760975214987](https://github.com/user-attachments/assets/f6443d46-c511-4edb-8089-0204e26226aa)


## Answer the questions below

**Use the `Exercise.pcapng` file to answer the questions. Read the "capture file comments". What is the flag?**  
`TryHackMe_Wireshark_Demo`

**What is the total number of packets?**  
`58620`

**What is the SHA256 hash value of the capture file?**  
`f446de335565fb0b0ee5e5a3266703c778b2f3dfad7efeaeccb2da5641a6d6eb`

---

# Task 3 — Packet Dissection

Packet dissection is also known as protocol dissection, which investigates packet details by decoding available protocols and fields. Wireshark supports a long list of protocols for dissection, and you can also write your dissection scripts. You can find more details on dissection here(opens in new tab).

> **Note:** This section covers how Wireshark uses OSI layers to break down packets and how to use these layers for analysis. It is expected that you already have background knowledge of the OSI model and how it works.

## Packet Details

You can click on a packet in the packet list pane to open its details (double-click will open details in a new window). Packets consist of 5 to 7 layers based on the OSI model. We will go over all of them in an HTTP packet from a sample capture. The picture below shows viewing packet number 27.
<img width="1953" height="1058" alt="image" src="https://github.com/user-attachments/assets/ff312f70-1bbc-4a91-b163-548bf23f7139" />


Each time you click a detail, it will highlight the corresponding part in the packet bytes pane.
<img width="1950" height="412" alt="image" src="https://github.com/user-attachments/assets/195d4993-324b-42aa-9ec0-299d5789b2b6" />


Let's have a closer view of the details pane.
<img width="898" height="161" alt="image" src="https://github.com/user-attachments/assets/7dca9201-2a9e-4184-938f-7f4564876efc" />


We can see seven distinct layers to the packet: frame/packet, source [MAC], source [IP], protocol, protocol errors, application protocol, and application data. Below we will go over the layers in more detail.

- **The Frame (Layer 1):** This will show you what frame/packet you are looking at and details specific to the Physical layer of the OSI model.
<img width="827" height="418" alt="image" src="https://github.com/user-attachments/assets/82e53917-e234-4eea-85d4-29c7806ddaac" />

- **Source [MAC] (Layer 2):** This will show you the source and destination MAC Addresses; from the Data Link layer of the OSI model.
<img width="1134" height="112" alt="image" src="https://github.com/user-attachments/assets/d7ca724d-3afd-40bb-ba74-2088eb04d8d9" />

- **Source [IP] (Layer 3):** This will show you the source and destination IPv4 Addresses; from the Network layer of the OSI model.
<img width="829" height="331" alt="image" src="https://github.com/user-attachments/assets/7cbf33d6-76a6-47be-9583-69ac63a80364" />

- **Protocol (Layer 4):** This will show details of the protocol used (UDP/TCP) and source and destination ports; from the Transport layer of the OSI model.
<img width="1054" height="532" alt="image" src="https://github.com/user-attachments/assets/6d1df221-4bc0-4b08-85b8-e33701283df4" />

- **Protocol Errors:** This continuation of the 4th layer shows specific segments from TCP that needed to be reassembled.
<img width="1259" height="135" alt="image" src="https://github.com/user-attachments/assets/949ec47f-fe09-4275-8e94-4904b34d01cc" />

- **Application Protocol (Layer 5):** This will show details specific to the protocol used, such as HTTP, FTP, and SMB. From the Application layer of the OSI model.
<img width="1261" height="352" alt="image" src="https://github.com/user-attachments/assets/f4997d23-4e9d-4c1d-950c-9904a84f8a55" />

- **Application Data:** This extension of the 5th layer can show the application-specific data.
<img width="1159" height="102" alt="image" src="https://github.com/user-attachments/assets/23c30ad8-1bf8-48cb-a358-1f32d9c30429" />


## Answer the questions below

**Use the `Exercise.pcapng` file to answer the questions. View packet number 38. Which markup language is used under the HTTP protocol?**  
`eXtensible Markup Language`

**What is the arrival date of the packet?**  
`05/13/2004`

**What is the TTL value?**  
`47`

**What is the TCP payload size?**  
`424`

**What is the e-tag value?**  
`9a01a-4696-7e354b00`

---

# Task 4 — Packet Navigation

## Packet Numbers

Wireshark calculates the number of investigated packets and assigns a unique number for each packet. This helps the analysis process for big captures and makes it easy to go back to a specific point of an event.
<img width="995" height="811" alt="image" src="https://github.com/user-attachments/assets/bc90c71f-3e68-40a9-a303-69c4f18e5c87" />


## Go to Packet

Packet numbers do not only help to count the total number of packets or make it easier to find/investigate specific packets. This feature not only navigates between packets up and down; it also provides in-frame packet tracking and finds the next packet in the particular part of the conversation. You can use the **Go** menu and toolbar to view specific packets.

![66c44fd9733427ea1181ad58-1761116433008](https://github.com/user-attachments/assets/6205b673-35bb-4904-937c-df0568711f44)

## Find Packets

Apart from packet number, Wireshark can find packets by packet content. You can use the **Edit → Find Packet** menu to make a search inside the packets for a particular event of interest. This helps analysts and administrators to find specific intrusion patterns or failure traces.

There are two crucial points in finding packets. The first is knowing the input type. This functionality accepts four types of inputs: **Display filter**, **Hex**, **String** and **Regex**. String and regex searches are the most commonly used search types. Searches are case insensitive, but you can set the case sensitivity in your search by clicking the radio button.

The second point is choosing the search field. You can conduct searches in the three panes (packet list, packet details, and packet bytes), and it is important to know the available information in each pane to find the event of interest. For example, if you try to find the information available in the packet details pane and conduct the search in the packet list pane, Wireshark won't find it even if it exists.
![66c44fd9733427ea1181ad58-1761116433011](https://github.com/user-attachments/assets/021752df-3315-44d8-956d-26e5090744c0)


## Mark Packets

Marking packets is another helpful functionality for analysts. You can find/point to a specific packet for further investigation by marking it. It helps analysts point to an event of interest or export particular packets from the capture. You can use the **Edit** or the **right-click** menu to mark/unmark packets.

Marked packets will be shown in black regardless of the original colour representing the connection type. Note that marked packet information is renewed every file session, so marked packets will be lost after closing the capture file.
![66c44fd9733427ea1181ad58-1761116433047](https://github.com/user-attachments/assets/d6e4c0f1-dd65-421d-b8d7-f2919bcfdcf1)


## Packet Comments

Similar to packet marking, commenting is another helpful feature for analysts. You can add comments for particular packets that will help the further investigation or remind and point out important/suspicious points for other layer analysts. Unlike packet marking, the comments can stay within the capture file until the operator removes them.
![66c44fd9733427ea1181ad58-1763530577252](https://github.com/user-attachments/assets/accc11fc-20e7-45f1-bfef-a37ef8423fce)


## Export Packets

Capture files can contain thousands of packets in a single file. As mentioned earlier, Wireshark is not an IDS, so sometimes, it is necessary to separate specific packages from the file and dig deeper to resolve an incident. This functionality helps analysts share the only suspicious packages (decided scope). Thus redundant information is not included in the analysis process. You can use the **File** menu to export packets.
![66c44fd9733427ea1181ad58-1761116433024](https://github.com/user-attachments/assets/2613c19f-5539-4692-83e3-96b0be1997cf)


## Export Objects (Files)

Wireshark can extract files transferred through the wire. For a security analyst, it is vital to discover shared files and save them for further investigation. Exporting objects are available only for selected protocol's streams (DICOM, HTTP, IMF, SMB and TFTP).
![66c44fd9733427ea1181ad58-1761116433258](https://github.com/user-attachments/assets/16bd4ede-cfce-4601-961e-411bc445d7c0)


## Time Display Format

Wireshark lists the packets as they are captured, so investigating the default flow is not always the best option. By default, Wireshark shows the time in **Seconds Since Beginning of Capture**, the common usage is using the UTC Time Display Format for a better view. You can use the **View → Time Display Format** menu to change the time display format.
![66c44fd9733427ea1181ad58-1761116433016](https://github.com/user-attachments/assets/f0acde8d-4a6a-4d99-9ff2-562e23768b9c)
<img width="1214" height="359" alt="image" src="https://github.com/user-attachments/assets/1ca8f6bf-1f31-4067-bf43-5d670b5731a0" />



## Expert Info

Wireshark also detects specific states of protocols to help analysts easily spot possible anomalies and problems. Note that these are only suggestions, and there is always a chance of having false positives/negatives. Expert info can provide a group of categories in three different severities. Details are shown in the table below.

| Severity | Colour | Info |
|---|---|---|
| Chat | Blue | Information on usual workflow. |
| Note | Cyan | Notable events like application error codes. |
| Warn | Yellow | Warnings like unusual error codes or problem statements. |
| Error | Red | Problems like malformed packets. |

Frequently encountered information groups are listed in the table below.

| Group | Info |
|---|---|
| Checksum | Checksum errors |
| Comment | Packet comment detection |
| Deprecated | Deprecated protocol usage |
| Malformed | Malformed packet detection |

You can use the **lower left bottom section** in the status bar or **Analyse → Expert Information** menu to view all available information entries via a dialogue box. It will show the packet number, summary, group protocol and total occurrence.
<img width="1463" height="530" alt="image" src="https://github.com/user-attachments/assets/989031a3-5c70-4f9d-af55-22fa61808efc" />


## Answer the questions below

**Search the "r4w" string in packet details. What is the name of artist 1?**  
`r4w8173`

**Go to packet 12 and read the packet comments. What is the answer?**  
`911cd574a42865a956ccde2d04495ebf`

**There is a ".txt" file inside the capture file. Find the file and read it; what is the alien's name?**  
`PACKETMASTER`

**Look at the expert info section. What is the number of warnings?**  
`1636`

---

# Task 5 — Packet Filtering

Wireshark has a powerful filter engine that helps analysts to narrow down the traffic and focus on the event of interest. Wireshark has two types of filtering approaches: capture and display filters. Capture filters are used for "capturing" only the packets valid for the used filter. Display filters are used for "viewing" the packets valid for the used filter. We will discuss these filters' differences and advanced usage in the next room. Now let's focus on basic usage of the display filters, which will help analysts in the first place.

Filters are specific queries designed for protocols available in Wireshark's official protocol reference. While the filters are only the option to investigate the event of interest, there are two different ways to filter traffic and remove the noise from the capture file. The first one uses queries, and the second uses the right-click menu. Wireshark provides a powerful GUI, and there is a golden rule for analysts who don't want to write queries for basic tasks: **"If you can click on it, you can filter and copy it"**

## Apply as Filter

This is the most basic way of filtering traffic. While investigating a capture file, you can click on the field you want to filter and use the **right-click menu** or **Analyse → Apply as Filter** menu to filter the specific value. Note that the number of total and displayed packets are always shown on the status bar.
<img width="1520" height="932" alt="image" src="https://github.com/user-attachments/assets/f5c6633d-1431-423e-9788-2a1ede499c5c" />


## Conversation Filter

When you use the **Apply as a Filter** option, you will filter only a single entity of the packet. This option is a good way of investigating a particular value in packets. However, suppose you want to investigate a specific packet number and all linked packets by focusing on IP addresses and port numbers. In that case, the **Conversation Filter** option helps you view only the related packets and hide the rest of the packets easily. You can use the **right-click menu** or **Analyse → Conversation Filter** menu to filter conversations.
<img width="1520" height="932" alt="image" src="https://github.com/user-attachments/assets/0d5f34b3-b0b3-42a9-9f15-363e1017814c" />


## Colourise Conversation

This option is similar to the **Conversation Filter** with one difference. It highlights the linked packets without applying a display filter and decreasing the number of viewed packets. This option works with the **Colouring Rules** option and changes the packet colours without considering the previously applied colour rule. You can use the **right-click menu** or **View → Colourise Conversation** menu to colourise a linked packet in a single click. Note that you can use the **View → Colourise Conversation → Reset Colourisation** menu to undo this operation.
<img width="1520" height="932" alt="image" src="https://github.com/user-attachments/assets/f67fdda6-813c-4edc-ba71-7f2a3f47486a" />


## Prepare as Filter

Similar to **Apply as Filter**, this option helps analysts create display filters using the **right-click** menu. However, unlike the previous one, this model doesn't apply the filters after the choice. It adds the required query to the pane and waits for the execution command (enter) or another chosen filtering option by using the ".. and/or.." from the **right-click menu**.
<img width="1586" height="932" alt="image" src="https://github.com/user-attachments/assets/cb8ecfac-6e2f-4bfb-add9-f2cfe535843d" />


## Apply as Column

By default, the packet list pane provides basic information about each packet. You can use the **right-click menu** or **Analyse → Apply as Column** menu to add columns to the packet list pane. Once you click on a value and apply it as a column, it will be visible on the packet list pane. This function helps analysts examine the appearance of a specific value/field across the available packets in the capture file. You can enable/disable the columns shown in the packet list pane by clicking on the top of the packet list pane.
<img width="1520" height="932" alt="image" src="https://github.com/user-attachments/assets/75c63bd8-8358-458f-aa77-bb91a3ba9a43" />


## Follow Stream

Wireshark displays everything in packet portion size. However, it is possible to reconstruct the streams and view the raw traffic as it is presented at the application level. Following the protocol, streams help analysts recreate the application-level data and understand the event of interest. It is also possible to view the unencrypted protocol data like usernames, passwords and other transferred data.

You can use the **right-click menu** or **Analyse → Follow TCP/UDP/HTTP Stream** menu to follow traffic streams. Streams are shown in a separate dialogue box; packets originating from the server are highlighted with blue, and those originating from the client are highlighted with red.
<img width="1515" height="932" alt="image" src="https://github.com/user-attachments/assets/b271b37e-6ce6-4cc1-8c30-bfc28a660397" />


Once you follow a stream, Wireshark automatically creates and applies the required filter to view the specific stream. Remember, once a filter is applied, the number of the viewed packets will change. You will need to use the **X button** located on the right upper side of the display filter bar to remove the display filter and view all available packets in the capture file.

## Simple Display Filter Queries

The easiest way to filter quickly the huge amount of packets, is by applying a display filter using the **Apply a display filter** bar shown on the image below.
<img width="1665" height="530" alt="image" src="https://github.com/user-attachments/assets/3f29095b-dea4-4ed0-8285-e36d5a7265de" />


There are many filter queries available and each of them can be extensively tweaked to show very specific results. Below are some simple filters to get started.

### Filter By Protocol Name or Port

There are two basic ways to filter based on a specific protocol: by protocol name and by protocol port number.

To filter by protocol name, simply type in the protocol name and hit enter or click on the arrow button at the right hand side of the display filter bar. The GIF below shows an example of how to filter for http traffic. You can similarly filter for other protocols using keywords like `arp`, `dhcp`, `ftp`, `smtp`, `pop`, `imap`, and more.
<img width="1657" height="532" alt="image" src="https://github.com/user-attachments/assets/605ce16f-56bc-4607-8a46-985345695f02" />


To filter by protocol port number, you can use the structure `tcp.port == <port number>` or `udp.port == <port number>`. For example, if you want to see only http packets, you would use the filter `tcp.port == 80` and then hit enter.
<img width="1657" height="532" alt="image" src="https://github.com/user-attachments/assets/37527e5e-75e9-4fdf-a7cb-5d016f6ff7a4" />


### Filter By IP

When analyzing a packet capture, there is often a need to filter for a specific IP. To filter for a specific IP, you can use the structure `ip.addr == <IP address>`. So if you need to search for the IP `192.168.1.2`, your filter would be `ip.addr == 192.168.1.2`.
<img width="1657" height="532" alt="image" src="https://github.com/user-attachments/assets/71f4ce23-3d54-4c4f-a727-f2d9321a30ef" />


## Answer the questions below

**Go to packet number 4. Right-click on the "Hypertext Transfer Protocol" and apply it as a filter. Now, look at the filter pane. What is the filter query?**  
`http`

**What is the number of displayed packets?**  
`1089`

**Go to packet number 33790, follow the HTTP stream, and look carefully at the responses. Looking at the web server's response, what is the total number of artists?**  
`3`

**What is the name of the second artist?**  
`Blad3`

---

# Task 6 — Conclusion

Congratulations! You just finished the "Wireshark: The Basics" room. In this room, we covered Wireshark, what it is, how it operates, and how to use it to investigate traffic captures.

Want to learn more? We invite you to complete the Wireshark: Packet Operations room to improve your Wireshark skills by investigating packets in-depth.

## Answer the questions below

**Proceed to the next room and keep learning!**  
`No answer needed`
