
# home_SOC_ab

step by step

In this project, I will create a home lab for SOC analysis.

During the setup, I faced a few hardware limitations. First, my computer only has 12GB of RAM. Second, Windows uses a lot of memory—it takes up at least 4GB of RAM just sitting idle. On top of that, Splunk needs about 4GB, which easily adds up to over 8GB of memory usage.

To solve this, I decided to use Linux as my main host operating system. I chose the EndeavourOS distribution because it is lightweight and only uses about 1GB of RAM.

Our lab will consist of 3 virtual machines:

    Attacker * Victim * Analyst

<img width="1611" height="837" alt="Screenshot From 2026-06-22 06-35-31" src="https://github.com/user-attachments/assets/01a2ae24-85a0-4394-a429-1d078625ddaf" />
<br><br><br>
The attacker machine will attack the victim machine:

<video src="https://github.com/user-attachments/assets/4435e20a-703e-44a5-9ba2-e4db5f2e1602" width="100%" controls preload="none"></video>
<br><br><br>
The victim machine will then send its logs to the analyst's SIEM:

<video src="https://github.com/user-attachments/assets/a63ee215-3918-4870-bf3b-a83177d8ac62" width="100%" controls preload="none"></video>
<br><br><br>
Let's start configuring each machine.

I set up each machine in VMware. Now, I will configure them so they can talk to each other without being connected to my actual host computer (to reduce security risks).

<img width="1770" height="1030" alt="Screenshot_٢٠٢٦٠٦٢٢_٠٧٤٥١٤" src="https://github.com/user-attachments/assets/426cc2cc-da40-4b3b-bcd3-29b558a6345f" />

<br><br><br>
Let's start with the analyst virtual machine:
<img width="1920" height="1035" alt="Screenshot_٢٠٢٦٠٦٢٢_١٤٣٣٥٥" src="https://github.com/user-attachments/assets/8d50177b-57a8-4103-b10e-b468dc6b8392" />


Here, we see a few different network options:

* **Bridged:** Connects directly to your physical home network. The virtual machines will be on the same network as your real devices, acting like they are plugged into the same router (for example: VM1 = 192.168.0.1, VM2 = 192.168.0.2, VM3 = 192.168.0.3). *Since they are on your real home network, do not run malware using this option.*
* **NAT:** Shares the host's IP address. It creates a separate network using the host's network adapter. If we have three virtual machines, each one will be on a different network from the host, and they will use the host's IP to access the internet (for example: Host = 10.10.10.1, VM1 = 192.168.0.1, VM2 = 192.168.0.2, VM3 = 192.168.0.3).
* **Host-only:** A private network shared only with the host. Each virtual machine can talk to the host and any other virtual machines set to host-only. This is a decent option for our project.
* **LAN Segment:** A private network shared only with specific virtual machines. They can talk to each other, but not to the host, and you have to manually configure the IP address for each machine. **This is the best option for our lab.** It gives you full control over the network, keeps it completely isolated, and blocks access to your host computer so you can test safely without worrying.


Now, let's create our new virtual network and name it LAN10.

<img width="1920" height="1035" alt="Screenshot_٢٠٢٦٠٦٢٢_١٥١٣٣٥" src="https://github.com/user-attachments/assets/8b6206d8-f242-4755-9cc6-b699edd580a3" />

Click Save, and we are done.

Next, we will configure each virtual machine and change their network settings to our LAN10 virtual network.

<img width="1920" height="1035" alt="Screenshot_٢٠٢٦٠٦٢٢_١٥١٨٥٣" src="https://github.com/user-attachments/assets/4aa347b7-8ca9-45de-999e-3c6dcdce0b7a" />
Click done to finish setting up the victim virtual machine, and do the exact same thing for the attacker virtual machine.


and we will do the same with the attacker virual machine


Now, let's configure and assign IP addresses to each machine, starting with the analyst machine:

<img width="1897" height="885" alt="Screenshot From 2026-06-23 06-32-29" src="https://github.com/user-attachments/assets/4244398c-7ba5-4134-9a48-f18c40011b66" />

Then, we will configure the victim machine:

<img width="1439" height="895" alt="Annotation 2026-06-22 201352" src="https://github.com/user-attachments/assets/74ebd322-fc3a-4c2a-86f7-51adc5bebe2f" />

Let's test their connection to see if they can talk to each other. We will use the ping command. If everything is set up correctly, we should see a reply from the analyst machine.

<img width="1421" height="877" alt="Annotation 2026-06-22 205406" src="https://github.com/user-attachments/assets/f802462d-a07f-4e54-ac7c-11b205a12a8d" />

Great! We are on the right track. Now, let's configure the attacker machine (Kali Linux):

<img width="1906" height="903" alt="Screenshot_2026-06-23_00_34_03" src="https://github.com/user-attachments/assets/f4297c0f-e524-43b9-b9c8-aad1a1eba67f" />

If we did it right, we should see a reply in our ping test here as well:
<img width="1440" height="860" alt="Annotation 2026-06-22 214656" src="https://github.com/user-attachments/assets/a0e1c519-ee3f-426e-b0b0-434e92ba3368" />

As you can see, all the machines are configured properly.

Next, we need to install Sysmon on the Windows machine and set it up to forward its logs to Splunk on the analyst machine.

We will use Splunk Universal Forwarder to make our victim machine send logs over to Splunk. We will also use Sysmon because it provides cleaner and more detailed event logs.

Downloading them is straightforward. For the Splunk Forwarder, go to the official Splunk website. For Sysmon, download it directly from Microsoft.

Let's start by installing the Splunk Forwarder just like any other standard software:
<img width="1440" height="900" alt="setup splunkforword" src="https://github.com/user-attachments/assets/92d695d1-7a30-40e4-93d9-a9c7e38404a4" />

During the setup, we need to specify where to send the logs. We will enter the IP address of our analyst machine:
<img width="1125" height="593" alt="splunkforwordsendip" src="https://github.com/user-attachments/assets/e722f175-5f25-4824-9ae9-3de81b6777f9" />

And the installation is finished 

<img width="1125" height="593" alt="finshSetupOfSeplunForword" src="https://github.com/user-attachments/assets/0033da73-7fda-4cc5-b69d-eaa1d621eb04" />

Now, let's check our outputs.conf file to verify where the logs are being sent:
<img width="1440" height="900" alt="splunkforwordOutputconf" src="https://github.com/user-attachments/assets/deec0bf1-c983-4587-85d1-93fa1ba4633b" />

Great, we can see that our logs are correctly pointing to the analyst machine.

we also want to edite the input.conf to tell the splunkforward what data to collect and what lable it with a specifi name 

<img width="1440" height="860" alt="take this" src="https://github.com/user-attachments/assets/7da14c2d-1f9e-4df0-a7b7-b08078c15e49" />

Now, we just need to restart the Splunk Forwarder service to apply the configuration.
<img width="1440" height="860" alt="splunk restart" src="https://github.com/user-attachments/assets/f962eaac-2f25-4c38-a732-5ed996fd1948" />

Now, let's move over to our analyst machine and check if port 9997 is listening (open) to receive the logs.

<img width="1687" height="900" alt="Screenshot From 2026-06-24 06-54-05" src="https://github.com/user-attachments/assets/75bf5b0f-7027-4b06-8f4b-26c66ed81b07" />
As we can see here, the port is closed (because it doesn't show up in the list).

Let's fix that. We need to make Splunk listen on that port by configuring the inputs.conf file. Since we don't have an inputs.conf file in this Splunk directory yet, we will create it from scratch and configure it:
<img width="1629" height="870" alt="Screenshot From 2026-06-24 07-20-38" src="https://github.com/user-attachments/assets/90629ead-a6ca-478d-8aca-93811f2286de" />

Let's check again to see if the port is open:
<img width="1629" height="870" alt="Screenshot From 2026-06-24 07-26-18" src="https://github.com/user-attachments/assets/c093c321-f9ad-40c5-805f-e7347ec9e81d" />
Great, now Splunk is successfully listening on port 9997.

However, I still wasn't getting a connection from the Splunk Forwarder.

<img width="1629" height="870" alt="Screenshot From 2026-06-24 07-28-34" src="https://github.com/user-attachments/assets/f489313f-4a42-4f04-84ba-aeee68f023d4" />
I checked the firewall thinking it might be blocking port 9997, but the firewall was completely inactive.

After 2 hours of searching, I found the mistake: I accidentally wrote a lowercase "w" instead of an uppercase "W" in my local/inputs.conf file! Ahhhhhhhhhhh!

<img width="1440" height="900" alt="inputconfcorrected" src="https://github.com/user-attachments/assets/703c360f-71b6-4e5b-a5f3-0d432e9762dd" />

But hey, I learned a lot from troubleshooting that.

Now, let's test our setup by creating a fake system event using a built-in Windows tool called `eventcreate`.

<img width="1440" height="860" alt="create massage" src="https://github.com/user-attachments/assets/88e51ae9-9ad5-4f66-80a8-4c58c63fdce9" />

Here, I created a mock event with ID 555 and the message "Hello omar".

And here is the final result inside Splunk:
<img width="1629" height="871" alt="event" src="https://github.com/user-attachments/assets/f166cf12-a5c8-4bde-a19f-75f2e6d112c6" />

Great! Everything works.

Next, we will download Sysmon and it confgraion file set it up on our Windows machine so we can get even more detailed event data.

----

We will download Sysmon from Microsoft and download this specific Sysmon configuration file from GitHub:

<img width="1871" height="989" alt="Screenshot_٢٠٢٦٠٦٢٥_٠٥٣٥٢٩" src="https://github.com/user-attachments/assets/cce6aad0-2b45-476d-9361-a51ee280e382" />

Now that we have both Sysmon and its configuration file, we need to extract (unzip) the Sysmon folder. After that, we will open PowerShell with Administrator privileges.

Next, we need to navigate to the directory where Sysmon was extracted and move our configuration file into that same folder:

`image of the sysmonmveconfig`

<img width="1440" height="860" alt="sysmonmoveconfigtosysmon" src="https://github.com/user-attachments/assets/94a817ab-80ef-4667-b6cf-63fde0ed025f" />

Then, we will install Sysmon using the command line`
image of sysmonunstall`
<img width="1440" height="860" alt="installsysmon" src="https://github.com/user-attachments/assets/70b5b0f1-0e13-4a1a-8528-7a0a60c8d7a1" />

Note that the `-i`flag is used to include our configuration file during the installation:

To verify that Sysmon installed correctly, we can open the Windows Services app and check for the Sysmon service:
`chich sysmon install correctly`
<img width="1440" height="860" alt="check sysomn is insatll it corrrectily" src="https://github.com/user-attachments/assets/d93170c5-4230-46eb-9120-2b3d652725c8" />


Wow, let's configure the Splunk Forwarder to send these new Sysmon logs to our Splunk analyst machine. We will open the inputs.conf file located at `C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf.`

In this file, we specify exactly where the forwarder can find the logs we want to send:
`image update input conf sysmon`
<img width="1440" height="900" alt="11" src="https://github.com/user-attachments/assets/20dae9a1-4a13-43b5-ae5a-147b89c1879d" />


Here, we tell the Splunk Forwarder that it can find the Sysmon logs under Event Viewer -> Applications and Services Logs -> Microsoft -> Windows -> Sysmon/Operational. We also configure it to send these logs specifically to a target index named sysmon.

After saving the file, we restart the Splunk Forwarder service.


Next, we need to jump over to our analyst machine and create the new sysmon index so Splunk knows where to store the incoming data. Inside the Splunk web interface, navigate to Settings -> Indexes and click New Index:
`iamge of creating sysmon index`

<img width="1853" height="1048" alt="Screenshot From 2026-06-25 06-59-30" src="https://github.com/user-attachments/assets/1dc4011f-5a6d-4b2b-810d-a4c79c6b17d2" />


However, when I tried to search for index=sysmon, absolutely nothing showed up! Why?

To find out, I checked the Splunk Forwarder's internal log file (splunkd.log) on the victim machine. I discovered that the issue was a permission error
`error from splunkforward log`
<img width="1440" height="860" alt="error of falid sysmon to connect" src="https://github.com/user-attachments/assets/bc781450-7c69-4fc9-99da-f7053d540b77" />

The logs showed errorcode=5, which means Access Denied.

I opened the Windows Services app to check which account was running the Splunk Forwarder service. I found that it was running under NT SERVICE\splunkforwarder. This is a virtual service account, meaning it operates with "least privileges" and doesn't have permission to read advanced event logs by default:

<img width="1440" height="860" alt="name of the leat peivalge account" src="https://github.com/user-attachments/assets/5a9f720f-0db1-4762-bd15-a12ecdf537a1" />

The solution is to add this service account to the local group that is allowed to read Windows event logs.

I copied the account name`NT SERVICE\splunkforwarder` and opened Computer Management -> Local Users and Groups -> Groups. From there, I double-clicked on the Event Log Readers group, clicked Add, pasted the account name, and clicked Check Names:

`iamge of imag cmputer mangment to edite evint reader pramion`
<img width="1440" height="860" alt="computermangment to edite even rader permation" src="https://github.com/user-attachments/assets/e6971306-1f77-40fa-b330-abcc096f166e" />

After completing those steps and restarting the Splunk Forwarder service one more time, our Sysmon index successfully started receiving data

<img width="1853" height="1048" alt="Screenshot From 2026-06-25 08-14-08" src="https://github.com/user-attachments/assets/8921e5dc-9336-4b81-833c-5e0aaec08ef6" />

as we see in the above image there is not fileds in the splunk there is only about 6 fileds and they are not that importent with text of row data so we need to fix that
to handle sysmon we will downlaod add-ons called **Splunk Add-on for Sysmon** 
and for handling standered windows events (like system,security..etc) we will download **Splunk Add-on for Microsoft Windows**

what is the panvit of extrction filed??
it make life easy rether then use complx splunk spl to get all ips or event id you will find them ready for you if you install these add-on let is take example
let is see that we wnat to search for event ip of 1 (process creation)
1- we need to extract all eventID and put them in filed classed EventID
`image for extracting eventID`

<img width="1839" height="871" alt="extractiondata with spl" src="https://github.com/user-attachments/assets/94ac6a85-2290-4f20-a641-8a13320145c5" />

then we will use that feiled to look for eventID=1
`image of search for eventID`
and here as we see these are all pross created like notepad

so we can do that without using any of that by using add-on we talk about

so we will go to splunk/app websit and search for that add on
`splunk/app/sysmon website iamge`

we will download that add-on and then upload it to splunk
`image of uploading add-on sysmon`
<img width="1839" height="871" alt="uoloadsysmon addon" src="https://github.com/user-attachments/assets/70485eb6-18ba-40c0-a45e-b81ff8dbee83" />

then we will restart splunk 
sudo /opt/splunk/bin/splunk restart --run-as-root

and we see that nothing changed
so i say for about 4H searching for way to solve that then i descaver way to do that

form what i read the promple that splunk realse update to splunk add-on said that all windwos sourcetype should be convert it to lowercase and they dont confiuger windows-sysmon add-on
so when the logs came to splunk and the sysmon look for "WinEventLog" and it see only "wineventlog" so it ignort it and didnt extract fileds.
i found two way to slove this 1)is by make splunk look for every logs came with wineventlog and apply the extraction,this way it worked but it is not the perfect way why?is becase this way will nake splunk apply the rule of extrcting to all windwos log including systemlog firewall log or any logs came from windows
2)is by make splunk look inside the xml raw and look for sysmon data and change it to XmlWinEventLog:Microsoft-Window...../Operational

to do that we need to write rule to make splunk change look for xmlwineventlog:microsoft-..../Oprational and change it to XmlWinEventLog:Micro..../Oprational

so we need to edit the props.conf and transforms.conf 
what is the props.con and transfroms.conf so the props,conf is seting there to look at data when it come and look to its sourcetype if it mach a rule to will apply it like if you see sourcetype of wineventlog redirected to there
transforms.conf when props.conf redirct data to transforms.conf it can apply some editing to it in our case it will change the sourcetype 
why they make it in sprade filed why they dont do it in one file?? becase simply to save and speed the prosses the props.conf it only look for the sourcetype if it mach it will send it to the transforms.conf to edite it 

so let start oure configruion
we will open props.conf first
sudo nano  /opt/splunk/etc/apps/Splunk_TA_windows/local/props.conf
we wil write this rule in it 
[xmlwineventlog]

TRANSFORMS-route_sysmon = force_sysmon_sourcetype

xmlwineventlog:tell splunk to check all incoming logs and look for log that its sourcetype is xmlwineventlog

TRANSFORMS-route_sysmon = force_sysmon_sourcetype:telling splunk to route this data to transforms.conf and look for rule force_sysmon_sourcetype to spply it to this logs

save that file then we will open transforms.conf
sudo nano  /opt/splunk/etc/apps/Splunk_TA_windows/local/transforms.conf

[force_sysmon_sourcetype]
REGEX = <Provider\s+Name="Microsoft-Windows_Sysmon"
DEST__KEY = MetaData:Sourcetype
FORMAT = sourcetype::XmlWinEventLog:Microsoft-Windows-Sysmon/Operational

here is our rule and it contain some instrecation to look for only sysmon sourctype log  and change to "XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"

now here is our confirutions
`image of cat the props and transfomt`

<img width="1839" height="871" alt="reslute of confuring props and trans" src="https://github.com/user-attachments/assets/40a39c1f-c97c-43ba-9add-d2ee803630de" />

then we will need to restart splunk by using 
sudo /opt/splunk/bin/splunk restart --run-as-root

and we finshed let look in splunk and see what happend

<img width="1839" height="871" alt="Screenshot From 2026-06-28 06-06-56" src="https://github.com/user-attachments/assets/7e3b259a-96ac-4484-bbe8-439dfc5e2965" />


now let do the intersting part we will attack the victem machine with our kali(attacker machine)
notice:i add some log to send in the splunkForWard input.conf so it will send security,powershell logs

































