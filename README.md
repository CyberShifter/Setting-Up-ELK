#### ELK in the Cloud
---
---
This documentation outlines the implementation of ELK SIEM (Elasticsearch, Logstash, and Kibana) along with Sysmon for enhanced logging capabilities. The goal is to illustrate the insights I gained into logging practices and the significance of these tools in the daily operations of a Security Operations Center (SOC) analyst or cybersecurity professional. The scope of this documentation covers the monitoring of logs from multiple laptop devices within my very own home network.

---
---

I combine three technologies to provide a powerful solution when working with large data sets. Additionally, I am able to set up SIEM rules to alert me as a defender to attacks on my very own home network.

* E - Elasticsearch: For storing and indexing log data.
* L - Logstash: For collecting, processing, and forwarding logs to Elasticsearch.
* K - Kibana: For visualizing and analyzing log data

ELK enables defenders to detect attacks and conduct threat hunting.

I searched up online for free SIEMs that I could use in my own home network and it led to me wanting to learn more about ELK. I didn't need several servers or to spend large sums of money. I discovered the opportunity to get into the driver's seat and experiment with ELK by using the Elastic Cloud 14-day trial. The trial didn't require a credit card to get started; I only needed my email and a password.

**1. Set up an account.**

[Start your free Elastic Cloud Trial](https://cloud.elastic.co/registration?settings=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJsZW5ndGgiOjE1MCwic2l6ZSI6NDA5NiwiZGVmYXVsdF9zaXplIjoxMDI0fQ.dS6xqdrcNBVkANlcS19AnsZmHVSqoPROLHprdeN-Qbc&source=education "https://cloud.elastic.co/registration?settings=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJsZW5ndGgiOjE1MCwic2l6ZSI6NDA5NiwiZGVmYXVsdF9zaXplIjoxMDI0fQ.dS6xqdrcNBVkANlcS19AnsZmHVSqoPROLHprdeN-Qbc&source=education")


I used this link to get to the trial sign up page.


![Signup page for Elastic Cloud](https://imgur.com/zJqQPdN.png)

---

After logging in, the site directed me to fill out confidential information for my account.

I filled out the proper fields with the correct information pictured below and selected the check boxes with red dots.

Once those fields were filled out, I clicked the  "Next" button seen on the bottom of the screen.

![Welcome To Elastic](https://imgur.com/HsJSYFJ.png)

---

**2. Start an ELK instance.**

Upon clicking next I was directed to the following page to name my deployment. I named it "security-deployment" and progressed by clicking "Create Deployment".

![Creating Delployment](https://imgur.com/zmKtc5d.png)

Next I'm directed to another page.

Elastic will present the credentials for this ELK stack. There is the option to download a CSV of the credentials. I saved this information in my personal e-journal to prevent the loss of this confidential info.

![Creating Creds](https://imgur.com/KfpJKzV.png)


Then I waited for the continue button to turn blue, once it was done, I clicked on continue

![Waiting For Deploy](https://imgur.com/SjCcO6Y.png)


I am greeted with a menu of options, but I skipped that menu.

![Skip Prompt](https://imgur.com/SHGTxWk.png)

Then at the top of the page, I clicked 'search' and typed "kibana" and hit 'enter'.

![Search Kibana](https://imgur.com/iX0k4sE.png)

When the next page loaded, I Selected the "Add Kibana" button.

![Add Kibana](https://imgur.com/J9dlB1w.png)

I am then prompted to "Install Elastic Agent" This is what I am going to put on the machine that monitors what's happening. I proceed by clicking "Install Elastic Agent"

![Add Elastic Agent](https://imgur.com/NyGspR5.png)

The next page, I am introduced to a wall of text. I will now Select windows.

Advance by clicking,  "Copy to Clipboard".

![Add Elastic Agent](https://imgur.com/2ZwzPWi.png)

I want to hold onto this command since it is alot to type.  It is recommended to paste this command into some file where you won't lose it. In this example, I saved it to a file I called "agent.txt."  I will come back and use this command later in this project.

<img src="https://imgur.com/gPk9Kpx.png" alt="Pasting information into agent_txt" style="zoom:70%;" />

The ELK stack is now configured and my connection information is saved.

---
---
#### Elastic Agents

I started an ELK instance in the Elastic Cloud in the screenshots above.

The Elastic Agent software enables me, as the user, to easily send logs to our ELK instance. Reminder to myself that this process is what security professionals call "ingesting."

---

**3. Download the Elastic Agent.**

I searched for the "applications" icon and typed "terminal." I made sure to click "Run as Admin."

![Terminal MacOS](https://imgur.com/cifAnAa.png)  

Once the PowerShell instance opened, I copied what I kept in the file, in my case, it was "Agent.txt," and pasted it into the PowerShell. Then, I hit enter.

![Terminal MacOS](https://imgur.com/6rFsqUt.png)

I made sure to type y and hit enter when prompted by the MacOS Terminal.
---

I switched back over to my browser, and indeed, I could see the message "1 Agent has been enrolled."

![Enrolled Machine](https://imgur.com/AWGmNK6.png)

Then Click "Add to Integration".

---

On the next page, I left everything default and clicked "Confirm Incoming Data.".

![Confirm Data](https://imgur.com/5BHC8PW.png)

The browser took a few seconds to confirm the machine is connected. Once that was finished, I clicked "View Assets."

![Enrolled](https://imgur.com/InrBd0p.png)

---

**4. Check The Fleet.**

Once that's done, I made sure the device has successfully connected before moving on to the next part.
![Fleet](https://imgur.com/txv0eM4.png)

---
---
By default, Windows logs are not ideal.  To get logs that are more readable and useful, I did some research and found out that I can use Sysmon. My macbook unfortunately cannot process Windows applications so I will be transferring everything to my VM that has Windows 10 in the hypervisor.  

**5. Download Sysmon**

This is the link used to download Sysmon.

[Download Sysmon](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon "https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon")

Find the "Download Sysmon" link.


![Download Sysmon](https://imgur.com/VA9vrMs.png)

Perform "Extract All" on the Sysmon Folder. Ensure the Sysmon folder is selected -- It will be highlighted blue.

![Sysmon Extract All](https://imgur.com/dQmg5OV.png)

"Extract" to the Downloads folder.  Windows should auto-populate the Downloads path.

In your search bar, type "PowerShell".
The following options will be presented.  Click "Run as Administrator."

<img src="https://imgur.com/aUT4dha.png" alt="Run as Administrator" style="zoom:50%;" />

In my PowerShell window, I entered the following command, substituting [USER] for the user I am using on my local system.
```powershell
cd C:\Users\[USER]\Downloads\Sysmon\
```


The following command will install and start Sysmon as a service.

```powershell
.\Sysmon.exe -i -n -accepteula
```

This is what my output looks like: 


![Sysmon is Running](https://imgur.com/DEypIrA.png)

Now that Sysmon is running on my system, I will configure my Elastic agent to gather these logs.  I will then Sign into my cloud account.


[Elastic Cloud Login](https://cloud.elastic.co/login "https://cloud.elastic.co/login")

Navigate to "Integrations" through the navigation menu.


![Integrations Tab](https://imgur.com/0O3dOET.png)

At the top of the page enter "windows" into the search bar.  Select the Windows option with the red square pictured below.


![Select Windows](https://imgur.com/CYSAJ8v.png)

Add this integration.


![Add Windows](https://imgur.com/5nCHeGU.png)


By default, the Sysmon logs channel should be active.  This can be checked under the "Collect events from the following Windows event log channels:" section of the "Add integration" page.


![Ensure Sysmon is Selected](https://imgur.com/syzpXwQ.png)

Save the Integration.


![Save the Integration](https://imgur.com/OsjCMkF.png)


When prompted I clicked "Add elastic agent to your hosts".


![Save and Deploy Changes](https://imgur.com/Jd12G0p.png)


In the Integrations menu, I foundd the "Installed integrations" tab.

In the beginning, I selected an Elastic Security configuration. In doing so, "Endpoint Security" and "System" are automatically installed in my Integrations.


![Installed Integrations](https://imgur.com/RkOpEtT.png)

Now that I have finished installation, I will be playing around on the computer that has Elastic Agent installed.  I moved files around, created files, started programs, and made some Google searches.  Doing all of that eventually generated some logs to ensure that I have Sysmon logs reaching our cloud.

After I created some log activity, I navigated to "Discover" by accessing the hamburger menu on the top left.


![Navigate to Discover](https://imgur.com/S1nYHJg.png)


I et a filter on my data to limit my results to sysmon data.  This can be done by searching the "data_stream.dataset" field for "windows.sysmon_operational" data. Then click "add filter". My filter is now officially set.


![Filter Data](https://imgur.com/Ael3zta.png)


My Sysmon data is being collected and sent to Elastic.


![Sysmon Results](https://imgur.com/x6SO99A.png)

---
---
Conclusion:
In my first documented project ever, I feel that I learned alot of valuable information that will help in my path to becoming a SOC Analyst. Within this project of handling my first ever SIEM I was able to learn how to:

* Implement ELK SIEM to centralize and analyze log data.
* Configure SIEM rules to alert on potential security incidents and anomalies.
* Enhance system monitoring with Sysmon for detailed event logging
* Extend monitoring to multiple laptop devices within the home network.
* Monitor a diverse range of logs, including system logs, network logs, and application logs.

After feeling a little accomplished with this project, I will use ELK as my primary SIEM, and probably try to find SIEMs that are notoriously used in the field, such as Splunk or QRadar to get a better understanding of SIEMs in general. I know that some SIEMs work differently than others and it only drives my hunger to test them in my home lab environment. To finally conclude this, this ELK SIEM and Sysmon implementation offers valuable insights into logging practices and their applications in the daily routines of SOC analysts and cybersecurity professionals. By monitoring the logs of multiple laptop devices in a home network, I was able to gain a holistic view of the security posture and potential threats. This documentation serves as a reference for maintaining a robust logging infrastructure and leveraging the power of ELK SIEM and Sysmon in enhancing cybersecurity measures.










