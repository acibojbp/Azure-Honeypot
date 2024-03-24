# Honeypot Deployment on Microsoft Azure

## Overview

This project deploys the T-Pot, the All-in-One Multi Honeypot platform, on Microsoft Azure to bolster security measures and enable advanced threat detection and analysis. The virtual machine and network infrastructure are configured for seamless operation, ensuring optimal performance and security. Through active monitoring and analysis of the generated honeypot data, the project facilitates early detection and response to potential cyber threats, enhancing the organization's overall cybersecurity posture.

### Objectives

- To deploy a robust and scalable honeypot solution on Microsoft Azure to bolster the organization's cybersecurity posture.
- To gain practical experience in configuring, managing, and monitoring honeypot environments for advanced threat detection and analysis.    
### Skills Learned

- Cloud Computing (Microsoft Azure)
- Honeypot Configuration and Management
- Threat Detection, Analysis, and Intelligence Gathering
### Tools Used

- Microsoft Azure
- T-Pot Honeypot Platform
- GitHub (for documentation, reference, and collaboration)

## Contents

[Create Microsoft Azure Virtual Machine](#create-microsoft-azure-virtual-machine)

[Installing and Configuring Honeypot](#installing-and-configuring-honeypot)

[Accessing T-Pot Web Interface](#accessing-t-pot-web-interface)

[Deleting Honeypot Resources](#deleting-honeypot-resources)

## Steps
  
Given the project's objective of setting up a honeypot to lure hackers globally, it's essential to consider a cloud-based hosting solution for optimal security. Numerous cloud providers offer varying pricing models tailored to diverse needs. Notable options include AWS, Google Cloud, and Alibaba Cloud.

For this lab, I've opted to use Microsoft Azure for a few key reasons:

- **Reliability:** Azure offers a stable and reliable infrastructure, reducing the risk of downtime.
- **Integration:** It integrates smoothly with other Microsoft services and tools we might use.
- **Scalability:** Azure makes it easy to scale resources up or down to meet our project's needs.
- **Security:** With its robust security features and compliance certifications, Azure provides a secure environment for our honeypot deployment.

However, feel free to use the cloud provider you're already familiar with if you prefer.

To get started with this lab, you'll need an active Microsoft Azure account. If you haven't set one up yet, you can do so here: [Create Your Azure Free Account Today | Microsoft Azure](https://azure.microsoft.com/en-us/free/).

For our honeypot setup, we'll be deploying t-pot. t-pot stands out as a multi-honeypot platform that offers a comprehensive suite of tools and features tailored for our needs. It provides a user-friendly interface, extensive logging capabilities, and a variety of honeypot services, making it an ideal choice for our project.

If you encounter any issues during the installation process or simply want to learn more about its features, I recommend referring to their official documentation on [GitHub](https://github.com/telekom-security/tpotce/). It's a valuable resource to help you navigate the setup and understand its functionalities better.
### Create Microsoft Azure Virtual Machine

Upon accessing the main dashboard of the Microsoft Azure portal, you can initiate the creation of a virtual machine by selecting "Virtual Machines" under Azure services at the top of the page or by using the search bar.

![](/images/honeypot-01.png)

You'll be taken to a new page where you can click "Create" and opt for "Azure Virtual Machine".

![](/images/honeypot-02.png)

If a region selection prompt appears, choose the region closest to you to optimize costs, as pricing varies by region.

Next, you'll need to establish a Resource Group. Feel free to give it any name that aligns with the machine's purpose. For instance, I'll name mine "Honeypot".

![](/images/honeypot-03.png)

Similarly, provide a name for the virtual machine. As previously mentioned, select the region closest to you to manage costs effectively. For the image option, as specified in the documentation, choose Debian 11 and retain the default architecture setting.

For the size, the minimum requirement, as per their specifications, is 8 GB. However, we'll opt for 16 GB, so select "D4s_V3" and click "Select".

![](/images/honeypot-04.png)

Next, to authenticate with our virtual machine, you have the option to choose between SSH public key or Password. For the purposes of this lab, I'll select Password. 
Enter a username; I'll use "Honey", and create a secure password.

![](/images/honeypot-05.png)

Ensure that SSH is selected, as we'll use this to connect to our virtual machine later.

![](/images/honeypot-06.png)

Now, proceed to "Disks" by clicking "Next". It's recommended to use a 128 GB disk. To set this up, click "Create and attach a new disk", then select "Change size" and adjust it to 128 GB. Confirm by clicking "OK".

![](/images/honeypot-07.png)

![](/images/honeypot-08.png)

Next, navigate to the "Networking" section. You'll see that a virtual network has been created and a public IP has been assigned to our virtual machine. There's no need to make any changes here.

![](/images/honeypot-09.png)

Proceed to the "Management" section. Here, you'll find the "auto-shutdown" feature, which automatically stops our VM daily to help save costs. Since we need to keep our VM running for a while, we'll leave this option disabled for now. However, it's worth noting that this feature can be handy when you no longer have free Azure credits to avoid unexpected charges.

![](/images/honeypot-10.png)
  
It's crucial to determine the duration for which you plan to keep your VM. Even when turned off, you'll still incur charges due to the associated public IP. If you no longer intend to use your VM, it's advisable to delete all resource groups to avoid additional charges.

![](/images/honeypot-11.png)

To deploy our VM, proceed to "Review + create". After verifying all settings, click on "Create".

![](/images/honeypot-12.png)

![](/images/honeypot-13.png)

![](/images/honeypot-14.png)

Once the VM is deployed, we need to configure it for the honeypot by opening specific ports. In our scenario, we want all ports to be open. To do this, click on "Go to resource". From there, navigate to "Networking". To open the ports, add an inbound rule and specify the range of ports you want to open. In our case, input "0-65535" and leave the rest as default. Click "Add" to create a new security rule.

![](/images/honeypot-15.png)

![](/images/honeypot-16.png)

Additionally, make sure to note down your public IP as we'll need it to connect to our VM in the next step.

## Installing and Configuring Honeypot

**Connecting to VM via SSH**:

![](/images/honeypot-17.png)

- To install the honeypot, we need to connect to our created VM via SSH. If you're using Windows, you can download [PuTTY](https://www.putty.org/) to establish the connection.
- For this lab, I'm using Linux, so I'll be connecting using the terminal with the `ssh` command. Since I've used a secure password for this VM, I've made things easier by creating a folder, echoing the password to a txt file, and using `sshpass` to login.
- You can install `sshpass` by typing the command:
```
sudo apt install sshpass
```
- Now we can login to our VM by entering the command:
```
sshpass -p `cat pass.txt` ssh Honey@20.174.160.233
```

**Updating the System**:

- Once connected, update your system by entering the command:
```
sudo apt update && sudo apt upgrade -y
```

**Cloning the Repository**:
- To clone the repository to our VM, we need Git. Install Git by typing:
```
sudo apt install git
```

- Now, download the repository by typing:
```
git clone https://github.com/telekom-security/tpotce/
```

![](/images/honeypot-18.png)

**Navigating to the Installation Folder**:

- Navigate to the folder by typing:
```
cd tpotce/iso/installer
```

**Installing t-pot**:

- Install t-pot by typing:
```
sudo ./install.sh --type=user
```
![](/images/honeypot-19.png)  

- Make sure to run it as root. If prompted to continue, type `y`.
- You'll be brought to the installer. Select "standard" from here.

![](/images/honeypot-20.png)  

**Creating Web User**:

- We need to create our web user, which will be used to log in to the console later.

**Completing the Installation**:

- Once you've created the web user, the installation will complete, and we can proceed to the next step.

![](/images/honeypot-21.png)  

## Accessing T-Pot Web Interface

![](/images/honeypot-22.png)  

- To access t-pot, open a web browser and navigate to the following address:
```
http://[VM_Public_IP]:64297
```
- Here, replace `[VM_Public_IP]` with the public IP address of our VM. In my case it will be `20.174.160.233`. The web interface runs on port 64297.
- If you encounter a potential security risk warning, simply click on "Advanced," accept the risk, and continue.
- Authenticate using the web user credentials you created in the previous step.

![](/images/honeypot-23.png)  

### Exploring T-Pot Interface

- Once logged in, you'll see the t-pot interface with various options. For detailed information about each option, you can refer to their [GitHub](https://github.com/telekom-security/tpotce/) page.

#### Kibana

On the t-pot landing page, simply click on `Kibana` to be redirected to the Kibana interface. Here, you'll find a wide range of dashboards and visualizations specifically designed for the honeypots supported by t-pot.

If you want to view a consolidated dashboard featuring all the honeypots together, head over to the second page and select `T-Pot`.

#### Attack Map

![](/images/honeypot-24.png)  

On the t-pot landing page, simply click on `Attack Map` to access this feature. Due to the Attack Map's use of web sockets, you'll need to re-enter the `<web_user>` credentials for security purposes.

In the Attack Map, you'll be able to view a live visualization of ongoing attacks targeting our honeypot. This dynamic map provides a real-time display of attack sources, target destinations, and other relevant attack details, offering valuable insights into the current threat landscape and the effectiveness of your honeypot setup.

If you're interested in gathering more information about one of the IP addresses shown on the Attack Map, you can utilize OSINT (Open Source Intelligence) methods. A recommended approach would be to scan the IP address on platforms like [AbuseIPDB](https://www.abuseipdb.com/). This can provide insights into the reputation of the IP, associated malicious activities, and other relevant details that can aid in understanding the nature of the attacks targeting your honeypot.
 
#### Elasticvue

![](/images/honeypot-25.png)  

On the t-pot landing page, simply click on `Elastivue` to access this feature. Elastivue provides insights into the cluster running all the services. If your honeypot is frequently under attack, you can adjust the RAM allocation accordingly. Additionally, Elastivue allows you to view logs and databases associated with your honeypot setup. Feel free to explore these features to gain a deeper understanding of your honeypot's performance and activity.

![](/images/honeypot-26.png)  

If you're interested in learning more about the features offered by t-pot, you can refer to their GitHub repository and read the documentation. This resource provides detailed information about the capabilities and functionalities of t-pot, helping you to make the most out of this honeypot solution.

## Deleting Honeypot Resources

Once you've finished exploring and no longer require the honeypot or if you've run out of free credits, it's essential to delete the resources to avoid incurring unexpected charges.

You can do this by following these steps:

1. Navigate to the Microsoft Azure portal.
2. Locate and select the resource group associated with your honeypot deployment.
3. Within the resource group, select the specific resources (virtual machines, storage accounts, etc.) you wish to delete.
4. Click on the "Delete" option and follow the on-screen instructions to confirm the deletion of the selected resources.

By deleting the unnecessary resources, you can ensure that you're not billed for services you're not using.

In conclusion, this project focused on deploying a honeypot using Microsoft Azure, leveraging the platform's robust infrastructure and capabilities to create a simulated environment for monitoring and analyzing potential cyber threats. By meticulously configuring the virtual machine, networking settings, and security rules, we established a secure and efficient setup tailored for honeypot operations. Utilizing T-Pot as the honeypot platform enabled advanced threat detection and analysis, providing valuable insights into the methods and techniques employed by malicious actors. The project emphasized the importance of proactive cybersecurity measures and demonstrated how cloud-based solutions like Azure can be instrumental in enhancing an organization's defensive strategies. Moving forward, continuous monitoring and analysis of honeypot data will be essential for staying ahead of evolving cyber threats and maintaining a resilient security posture.
