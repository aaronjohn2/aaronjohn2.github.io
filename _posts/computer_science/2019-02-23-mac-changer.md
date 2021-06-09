---
layout: post
title:  How to Write a Python Script to Change the MAC Address of a Network Interface
date:   2019-02-23
tags: computer_science
---

A MAC address is a permanent, physical and unique address that is assigned to a network interface--by the device manufacturer. However, some people, like me, would like to increase anonymity, impersonate other devices, or even bypass filters. Hence, by changing the MAC address one can accomplish that.

Usually, in terminal, by typing three lines of system commands, one can change the MAC address of a specific interface. For example, if I want to change the MAC address of wlan0, I would type the following.
```shell
ifconfig wlan0 down
ifconfig wlan0 hw ether 00:11:22:33:44:55
ifconfig wlan0 up
```

However, I personally like to be efficient and save time while completing my day to day tasks. Hence, in this documentation, I will go through the process on how to write a simple Python script that will change the MAC address for us in an instant. Therefore, instead of executing three lines of system commands, we will be down to executing just one line of system command.

1. First we will start off by importing the the necessary modules to complete the task. Here, I added a shebang at the beginning of the script, so that the script recognizes the interpreter type when you execute the script from shell. Here, by using the subprocess module, we can execute system commands from the script. The optparse module will help us handle user input command line arguments. Finally, the re module will help us extract a substring using regex.
    ```python
    #!usr/bin/env python
    
    import subprocess
    import optparse
    import re
    ```

2. Usually in terminal, when you execute a program like airodump-ng you would pass arguments with it in order to get a certain result. For example, say you want airodump-ng to hop on a band 'a' frequency channel using your wlan0 interface; then you would execute the following command to identify all the flowing packets with that band:
    ```shell
    airodump-ng --band a wlan0
    ```
    Hence, we will start off by writing a function to get_arguments for our mac_changer function (which I will explain in the next step). Here, I will use optparse to create a new "parser" object/entity to handle user input command line arguments that will be saved in this parser object that I created below. This object inherits all the properties from OptionParser, which lets us add arguments using the add_option function and lets us save the user input using the parse_args() function. (There are many more functions in optparse but these are the two functions that I will use for this documentation.)
    ```python
    def get_arguments():
        parser = optparse.OptionParser()
        parser.add_option("-i", "--interface", dest="interface", help="Specify the interface of which you want to change the MAC address.")
        parser.add_option("-m", "--mac", dest="new_mac", help="Specify a random MAC address you would like to the interface to use.")
        (options, arguments) = parser.parse_args()
        if not options.interface():
            parser.error("[-] Error: interface not specified, use --help for more info.")
        elif not options.new_mac:
            parser.error("[-] Error: MAC address not specified, use --help for more info.")
        return options
    ```
    (Note: for the parser.add_option() function I allowed the user to enter an input value using either of the two arguments that I provided. Here, I have also given a short description of what each argument is suppose to do, in case the user requires --help. Finally, it stores the value entered by the user in the variable specified in dest. Hence, in the parse_args() function, it returns the input value specified after the argument which gets saved in the options.[dest] while the argument entered by the user gets saved in the arguments variable. However, for this tool we only require the value entered by the user. Therefore, we will return the options variable which contains the value of the Interface and new_mac.

3. Next, we will write a function to change the MAC address. Here, we will use the subprocess module and the call function to execute OS system commands for us in the background.
    ```python
    def change_mac(interface, new_mac):
        print("[+] Changing MAC address for " + interface + " to " + new_mac)
        subprocess.call(["ifconfig", interface, "down"])
        subprocess.call(["ifconfig", interface, "hw", "ether", new_mac])
        subprocess.call(["ifconfig", interface, "up"])
    ```

4. Next, we will write a function to get the current MAC address so that we can display it on the user's terminal. In order to accomplish this function, we will use regex to extract the MAC substring from the "ifconfig" command.
    ```python
    def get_current_mac(interface):
        ifconfig_result = subprocess.check_output(["ifconfig", interface])
        regex_mac_search_result = re.search(r"\w\w:\w\w:\w\w:\w\w:\w\w:\w\w", ifconfig_result)
        if regex_mac_search_result:
            return regex_mac_search_result.group(0)
        else:
            print("[-] Error: could not read MAC address.")
    ```
    (Note - here, I am using the check_output() function of the subprocess module to to capture the result (read system output), and we save this result in the variable ifconfig_result. Then we search this ifconfig_result variable using a regex rule to filter the pattern of the MAC address. We save this filtered pattern (MAC address) in regex_mac_search_result. In the above search() function, 'r' followed by quotes is for inputing the regex rule. Here, .group(0) will give us the first MAC address pattern that is found, since it saves the result as a list. For more info - I used pythex.org to create the regex rule, where "\w" is the rule to match alphanumeric characters.)

5. Finally, we will call all the above functions to make this script functional. First, we will call the get_arguments() function and we will save the returned output to the options variable. Next, we will call the get_current_mac() function to get the current MAC address of the user's interface before it is changed, and we will display that MAC on the screen. Here, for some reason, if the MAC address is not found, (i.e. if the user enters a wrong interface, or simply if the interface does not have a MAC) then we should note that since the function has nothing to return, its returning an object called None, which basically means, its returning nothing. Since Python does not know how to handle none types we tell it to handle this None type by wrapping it in str (a.k.a. Casting). Next we call the change_mac() function to change the MAC address of the specified interface. Lastly, we will display the altered MAC address on the user's terminal stating if the MAC successfully changed or not. Here, I compare the old MAC address (options.new_mac) with the new MAC address (current_mac) to accomplish this.
    ```python
    options = get_arguments()

    current_mac = get_current_mac(options.interface)
    print("The current MAC Address = " + str(current_mac))

    change_mac(options.interface, options.new_mac)

    current_mac = get_current_mac(options.interface)
    if current_mac == options.new_mac:
        print("[+] The MAC address successfully changed to " + current_mac)
    else:
        print("[-] Error: the MAC address did not get changed. Contact tech support for help.")
    ```
