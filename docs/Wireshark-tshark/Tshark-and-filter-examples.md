## Tshark tutorial and filter examples

**tshark is a packet capture tool that also has powerful reading and parsing features for pcap analysis.**

Rather than repeat the information in the extensive man page and on the wireshark.org documentation archive, I will focus on providing practical examples for how you can get started using tshark and begin carving valuable information from the wire.
Tshark examples

Use these as the basis for starting to build your extraction commands. As you can see the syntax for capturing and reading a pcap is very similar to tcpdump.
Capture Packets with Tshark

      tshark -i wlan0 -w capture-output.pcap

Read a Pcap with Tshark

         tshark -r capture-output.pcap

HTTP Analysis with Tshark

In the following example you can see that we extract data from any HTTP requests that are seen. Using the `-T` we specify that we want to extract fields and with the  `-e` 
options we identify which fields we want to extract.

           tshark -i wlan0 -Y http.request -T fields -e http.host -e http.user_agent

         searchdns.netcraft.com	Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:36.0) Gecko/20100101 Firefox/36.0
        searchdns.netcraft.com	Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:36.0) Gecko/20100101 Firefox/36.0
         ads.netcraft.com	Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:36.0) Gecko/20100101 Firefox/36.0

The default separator for the fields in the output above is TAB. We could also use the parameter `-E seperator=`, to change the delimeter to a comma.
Parse User Agents and Frequency with Standard Shell Commands

Using the previous command to extract `http.user_agent`, this time extracting from a pcap rather than off the live interface. Note in this 
example combining with standard shell commands allows us to sort and count the occurrences of the http.user_agent.

         tshark -r example.pcap -Y http.request -T fields -e http.host -e http.user_agent | sort | uniq -c | sort -n

Using this we can quickly parse a pcap, even if it is very large and get a summary of all the user agents seen. This can be used to detect malware, old browsers on your network and scripts.
Using additional HTTP filters in Analysis

We could perform a similar analysis with the request URL in place of the user agent -e http.request.full_uri. Other fields we could include in the output are -e ip.dst and -e http.request.method. As you can see by combing different filters and output fields we can create very complex data extraction commands for tshark that can be used to find interesting things within a capture.

              tshark -r example.pcap -Y http.request -T fields -e http.host -e ip.dst -e http.request.full_uri

DNS Analysis with Tshark

Here is an example that extracts both the DNS query and the response address.

                tshark -i wlan0 -f "src port 53" -n -T fields -e dns.qry.name -e dns.resp.addr

              68 campus-map.stanford.edu	171.64.144.142
              www.google.com	
              itunes.apple.com	104.74.40.29
              71 itunes.apple.com	
              campus-map.stanford.edu	
              admission.stanford.edu	171.67.215.200
              74 financialaid.stanford.edu	171.67.215.200
              admission.stanford.edu	

Add time and source / destination IP addresses `-e frame.time -e ip.src -e ip.dst` to your output.

    tshark -i wlan0 -f "src port 53" -n -T fields -e frame.time -e ip.src -e ip.dst -e dns.qry.name -e dns.resp.addr

      Apr 22, 2015 23:20:16.922103000 8.8.8.8 192.168.1.7 wprecon.com	198.74.56.127
    1 Apr 22, 2015 23:20:17.314244000 8.8.8.8 192.168.1.7 wprecon.com	
    2 Apr 22, 2015 23:20:18.090110000 8.8.8.8 192.168.1.7 code.jquery.com

 One of the great advantages that tshark has over the wireshark GUI is stdout giving you many options to manipulate and clean the output.

Lets get passwords.... in a HTTP post. By not specifying the fields option as above we will receive the full TCP stream of the HTTP Post. If we add the filter tcp contains "password" and grep for that password we will just get the actual POST data line.

         tshark -i wlan0 -Y 'http.request.method == POST and tcp contains "password"' | grep password

            csrfmiddlewaretoken=VkRzURF2EFYb4Q4qgDusBz0AWMrBXqN3&password=abc123
 
For the Next Trick you will need to install Tshark 2.4

The latest version of Tshark 2.4 includes a number of useful new features. To install the latest version on Ubuntu 16.04 or 17.04 use the following commands to add the package repository.

              sudo add-apt-repository ppa:dreibh/ppa
               sudo apt-get update && sudo apt-get install wireshark tshark

Now one excellent feature of the latest version of tshark is the ability to export objects (files) from pcaps using the command line.
Extract Files from PCAP using Tshark

The export objects feature has been available in wireshark for a long time now. Having this ability available on the command line is an excellent addition to tshark.

You will need version 2.3.0 or higher for the export objects parameter to be available to tshark.

This command will extract files from an SMB stream and extract them to the location tmpfolder.

          tshark -nr test.pcap --export-objects smb,tmpfolder

And this command will do the same except from HTTP, extracting all the files seen in the pcap.

        tshark -nr test.pcap --export-objects http,tmpfolder

It is a quick and easy way to get all the images, html, js and other HTTP objects from a pcap containing HTTP traffic.

Hopefully this tutorial has given you a quick taste of the useful features that are available to you when using tshark for extracting data from the wire or from pcaps.

