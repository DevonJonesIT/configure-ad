# On-Premises Active Directory Deployed in the Cloud (Azure)

This tutorial outlines the implementation of **on-premises Active Directory** within **Azure Virtual Machines**.

---

## **Video Demonstration**

- ### [YouTube: How to Deploy On-Premises Active Directory within Azure Compute](https://www.youtube.com/results?search_query=how+to+deploy+on-premises+active+directory+within+azure+compute)

---

## **Environments and Technologies Used**

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

---

## **Operating Systems Used**

- Windows Server 2022
- Windows 10 (21H2)

---

## **High-Level Deployment and Configuration Steps**

1. Create Azure Virtual Machines  
2. Configure Networking and Security Groups  
3. Install Active Directory Domain Services (AD DS)  
4. Promote the Server to a Domain Controller  
5. Join Client Machine to the Domain

---

## **Detailed Deployment and Configuration Steps**

### **Step 1: Create Azure Virtual Machines**

1. Log in to the **Azure Portal** at [portal.azure.com](https://portal.azure.com).
2. Create two virtual machines (VMs):
   - **Domain Controller VM**: Install **Windows Server 2022**.
   - **Client Machine VM**: Install **Windows 10**.
3. Ensure both VMs are in the **same virtual network (VNet)**.
4. Allocate appropriate **resource groups, storage, and virtual IP addresses**.
5. Once the VMs are created, take note of their **public and private IP addresses**.

---

### **Step 2: Configure Networking and Security Groups**

1. In the Azure portal, navigate to the **Networking** tab for each VM.
2. Create an **inbound security rule** to allow **Remote Desktop (RDP)**:
   - **Protocol:** TCP
   - **Port Range:** 3389
   - **Source:** Any (or specify your IP for extra security)
3. Create another **inbound security rule** to allow **DNS traffic** (port 53) for communication with the domain.
4. Open **PowerShell** on the client machine and ping the Domain Controller to verify connectivity:
   ```powershell
   ping <DomainControllerIP>


### **Step 3: Install Active Directory Domain Services (AD DS)**

1. Log in to the **Domain Controller VM** using **Remote Desktop**.
2. Open **Server Manager** -> **Add Roles and Features**.
3. Select **Active Directory Domain Services (AD DS)** and click **Next**.
4. Ensure that the necessary features, such as **DNS Server** and **Group Policy Management**, are checked.
5. Click **Install** and wait for the installation to complete.
6. After installation, a notification will appear prompting you to **Promote this server to a domain controller**.

---

### **Step 4: Promote the Server to a Domain Controller**

1. In the **Active Directory Domain Services Configuration Wizard**:
   - Choose **Add a new forest**.
   - Enter a **domain name** (e.g., `mydomain.local`).
2. Set the **Forest Functional Level** and **Domain Functional Level** to **Windows Server 2022**.
3. Create a **Directory Services Restore Mode (DSRM) password** and click **Next**.
4. Review the **DNS Options** and ensure that no critical warnings are present.
5. Click **Install** to promote the server. The server will restart automatically after the promotion process.
6. After the reboot, log in to the **Domain Controller** and verify the domain controller role:
   - Open **Active Directory Users and Computers (ADUC)** from **Server Manager**.

---

### **Step 5: Join the Client Machine to the Domain**

1. Log in to the **Windows 10 Client VM** using **Remote Desktop**.
2. Open **Settings** -> **System** -> **About**.
3. Under **Device Specifications**, click **Rename this PC (Advanced)**.
4. Click **Join a Domain** and enter the **domain name** (e.g., `mydomain.local`).
5. Enter the credentials of a **domain administrator** to authenticate the domain join.
6. Restart the client machine after joining the domain.
7. After the reboot, log in using a **domain user account**.

---

## **Validation and Testing**

1. Open **Active Directory Users and Computers (ADUC)** on the **Domain Controller VM**.
2. Create a new **Organizational Unit (OU)** and a new **user** inside the OU.
3. Log in to the **Client Machine VM** with the newly created **domain user**.
4. Verify successful login by opening **PowerShell** and running:
   ```powershell
   whoami



## **Additional Configuration Options**

- **Group Policy Management:**  
  - Open **Group Policy Management** from **Server Manager** to create policies for user settings (e.g., password policies, desktop restrictions).
- **DNS Configuration:**  
  - Verify that the **Domain Controller** is set as the **primary DNS** for the network to resolve domain names.
- **Remote Desktop Settings:**  
  - Configure **remote desktop permissions** to allow secure access to both the domain controller and client machine.

---

## **Final Notes and Best Practices**

- **Snapshots:**  
  Take snapshots of your VM states after each major configuration step to quickly restore in case of errors.
- **Backup:**  
  Set up regular **backups** of the Active Directory database to avoid data loss.
- **Monitoring:**  
  Use **Azure Monitor** to track VM performance and domain health.
