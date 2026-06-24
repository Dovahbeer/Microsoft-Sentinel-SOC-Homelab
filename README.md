# SOC Homelab
### STATUS: 🔴 In Progress
### Disclaimer
As this is one of my earlier projects there will be parts that addresses even the most basic fundemental concepts. This ensures that I've managed to adequately understand and explain each step of the process for myself and hopefully clearly enough for you!

### Concept
<img width="1877" height="1038" alt="concept" src="https://github.com/user-attachments/assets/c44d845b-cad3-4b5c-8769-1b61b9a778e3" />

The goal is to set up a Windows VM and disable firewalls. When attackers attempt to RDP into it we'll receieve failed login attempts, which will then be sent to Microsoft Sentinel and be available for analysis. 

# Step 1 - Setting up Microsoft Azure.
Thankfully Microsoft allows users to explore Azure for free, meaning that we'll only need to provide a Microsoft account to get started.

<img width="858" height="452" alt="Confidential" src="https://github.com/user-attachments/assets/270f7e23-e413-4818-b414-6dcd1df06c76" />

After providing credentials we're given *200*$ worth of credit, allowing us to proceed with the following steps.

📝*Note: If you recieve the message "You are not eligible for an Azure subscription", you can resolve this issue by creating a new account even if the credentials matches the address, phone number and credit card of the illegible account*

# Step 2 - Configuring the Virtual Machine.
The goal is to set up a Virtual Machine that will act as honeypot. This requires both creating a new Virtual Machine and lowering the security measures which makes it enticing for real hackers to attempt to infiltrate. These attempts will later on generate logs for us to analyze.
We need to create the neccesary structure in Azure in order for this lab to work. This includes ensuring that they all belong to the same subscription and subsequent resource group, virtual network etc. 

### Resource Group
First we create a Resource group, which allows us to group everything associated with this lab in one convenient spot. In a real SOC scenario this could be used to create and deploy templates, handle cost management or in this case organising labs in seperate entities.

<img width="722" height="203" alt="image" src="https://github.com/user-attachments/assets/36dcb45b-3e85-41d4-839d-0a3aa4d38349" />


### Virtual Network
To ensure that our VM will be able to communicate with other system and resources, we need to create a virtual network. This allows us to set up private ip-addresses, set up security rules and policies and allows us to configure internet access control.

<img width="750" height="374" alt="image" src="https://github.com/user-attachments/assets/5b848400-1144-420d-b980-5984663522b0" />


📝*Note: Ensure the region matches the one set in the resource group to prevent future issues*

When navigating to the Security tab, make sure that none of the boxes are checked in. We do not want protections in place as that may negatively affect the amount of attacks on our VM. In a real scenario however it's expected to configure these settings appropriately. Furthermore we'll leave the address space untouched - we could do by with less ip-addresses, but for this basic level lab it's fine to just leave as is.

<img width="700" height="274" alt="image" src="https://github.com/user-attachments/assets/9578dab3-4d32-4867-8bdd-f55c5fc5be51" />


### Virtual Machine
Our honeypot that need to be configured properly to attract attackers. At this point of the lab we're approaching closely to our free budget limit. If you're following this guide yourself, navigate in various menues to reach my configurations:

<img width="775" height="744" alt="WarningArrow" src="https://github.com/user-attachments/assets/fa4fcc99-f3c8-48d4-ba72-48fb703549da" />

‼️⚠️ **If you are following this guide, make absolutely sure you've checked in "Run with Azure Spot discount", otherwise your VM will bill you thousands of dollars per month.** ⚠️‼️

This was my final configuration, you can verify your pricing but looking at the highlighted part of the screenshot:

<img width="771" height="743" alt="PricingHighlight" src="https://github.com/user-attachments/assets/ff2bf69a-2b13-42a7-bb80-e8280395e7b8" />

When proceeding with the following tabs:

- **Network**
   - Ensure that "Virtual Network" is set to the one we created.
   - Check the box for "Delete public IP and NIC when VM is deleted"

- **Monitoring**
  - On "Boot diagnostics", check "disable".

After the VM has been reviewed and created, we can head back to our resource group and verify that everything has been successfully deployed:

<img width="1095" height="863" alt="image" src="https://github.com/user-attachments/assets/6a57c437-81eb-4510-852a-6e206c9c8587" />

# Step 4 - Configuring the Network Security Group (NSG) & Windows Firewall

### Network Security Group
From the resource group we navigate to "/.../-nsg" on the list.

As we want our VM to be accesible to the general public, we'll remove the default RDP rule and replacing it with one that is significantly more dangerous. 

<img width="1658" height="417" alt="image" src="https://github.com/user-attachments/assets/f982c9d1-d81b-4ca0-8b18-a244775596fb" />

In order to create a less secure inbound rule, we navigate to Settings --> Inbound Security rules --> + Add.

Settings:
- Source - Any
- Source Port Ranges - *
- Destination - Any
- Service - Custom
- Destination Port Ranges - *
- Protocol - Any
- Action - Allow
- Priority - 100
- Name - Dangerous_InBoundRule

This setup generates a high amount of warnings:

<img width="541" height="398" alt="image" src="https://github.com/user-attachments/assets/c04e1b2e-80a4-4bc0-96c6-fe97352a6510" />

### Windows Defender

# Step 5 - Setting up log transmission to Sentinel.

# Step 6 - Analyzing a few logs.

# What I've learned. 📝
