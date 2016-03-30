{{{
  "title": "Getting started with RackWare Management Module v5",
  "date": "March 28, 2016",
  "author": "Todd A. Matters",
  "attachments": [],
  "contentIsHTML": false
}}}

![Rackware Logo](../../images/RW_horizontal_lg.jpeg)

### Technology Profile

RackWare provides a software management suite for cloud called RackWare Management Module (RMM).  The RMM solution is Enterprise focused, making cloud computing viable and economical for Enterprises.  While Enterprise focused, handling large complex applications, RackWare is also applicable to a wide variety of other computing environments including SMB and test dev.   The RMM solution enables Enterprises to implement 4 major use cases: workload mobility, backup, disaster recovery, and AutoScaling.   The underpinning technology is a patented any to any replication engine that includes physical servers on both origin and target, is hypervisor and cloud agnostic.  The use cases and replication confluence spans data centers, private, and public clouds.  



### Description

The latest features in the RMM include:
Discover and Analysis
Image replication
	Physical servers (as origin and target)
	All major hypervisors (across disparate hypervisors)
	Cloud agnostic
	All major databases
	Custom applications
	Tiered applications
	Enterprise class applications
Backup with multiple recovery points
Disaster Recovery
	Aggressive RPO and RTO
	Dynamic provisioning (provision compute only on failover)
	Pre-provisioned (optimize RTO)
	Test mode
	Entirely automated failover and fallback

For more information, please visit [Rackware's web page](http://www.rackwareinc.com)

### Audience
CenturyLink Cloud Users

### Impact
After reading this article, the user should be understand the use cases for the RackWare RMM and to provision and install the RMM software.  

### Prerequisite

IP level network connectivity is required from the RMM Server to the Origin workloads.  Additional information regarding prerequisites can be found in the RMM v5 Prerequisites and Operational Requirements document.  


### Postrequisite
If using the RMM to migrate workloads to the CenturyLink cloud, the RMM Server can be decommissioned after the replications are complete.  

If using the RMM to for Disaster Recovery, the DR configuration and operation can be monitored by configuring email alerts in the DR Policy.  Consult the RackWare v5 Disaster Recovery Guide for more information.  

### Deploying the <name of the blueprint> Blueprint

#### Steps to Deploy Blueprint
1. **Locate the "Install RackWare RMM on Linux" Blueprint**
  1. Starting from the CenturyLink Control Panel, navigate to the Blueprints Library.
  2. Search for “RackWare RMM” in the keyword search on the right side of the page.
  3. Locate the 'Install RackWare RMM on Linux' Blueprint

2. **Choose and Deploy the Blueprint.**
   Click the "Install RackWare RMM on Linux" Blueprint.

3. **Customize the Blueprint**
  1. **Provide the input global blueprint values.**
    1. Customer name
    2. License Type (proc/prod)
    3. Functional License Type (DR, Migration)
    4. Number of licenses
    5. Customer email ID
    6. RMM installer web link

  2. **Customize server name and Build servers**
    1. Provide build server password and other network details to access server after deployment.

4. **Review and Confirm the Blueprint**
  1. Click “next: step 2”
  2. Verify your configuration details.

5. **Deploy the Blueprint**
  1. Once verified, click on the ‘deploy blueprint’ button. You will see the deployment details along with an email stating the Blueprint is queued for execution.
  2. This will kick off the blueprint deploy process and load a page to allow you to track the progress of the deployment.

6. **Monitor the Activity Queue**
  1. Monitor the Deployment Queue to view the progress of the blueprint.
  2. You can access the queue at any time by clicking the Queue link under the Blueprints menu on the main navigation drop-down.
  3. Once the blueprint completes successfully, you will receive an email stating that the blueprint build is complete. Please do not use the application until you have received this email notification.

### Deploy the RMM to an existing server (alternate option)  

The RMM is available as a Script Package for deployment on a newly provisioned server or an existing server based on your own sizing requirements.


#### Steps
1. **Deploy or Identify an Existing Server**
Identify the server targeted for RMM  installation.  The Operating system must be supported by the Script Package.  The server must be a server within your CenturyLink Cloud account.

Installation set up for RackWare is comprised of a dedicated server (physical or virtual) running the RMM.  The RMM server requirements are as follows:
• x86_64 architecture (Intel or AMD)
• RHEL / CentOS v6 (6.5 or higher)
• 10 GB or more of storage on /opt/
• 5 GB or more storage on /srv/   
• 5 GB or more of storage on /tmp
• 16 GB memory
• 4 cores (vCPU)
• Additional storage on /srv/ sufficient to hold captured Images ***

The RMM installation requires the following additional configuration:
• Working yum manager with EPEL package
• SELinux is disabled
• Name resolution must be configured and working.  
• Access to the Internet for additional packages
• Install as root;   if root access is not available on the RMM, a userid in the sudoers file may be used as long as the userid has” ALL=(ALL)  ALL”permissions in the sudoers file.  
• The root user must be defined in the sudoers file.

If mount points are set up for /opt, /srv, or /tmp, be sure to set up those mount points in the /etc/fstab file.

The following ports are required to the Internet:
       TCP port 443

*** These requirements are necessary only during the installation.  After a successful installation, these requirements do not apply for normal RMM operation.

After installing RHEL/CentOS, ensure that the kernel, packages, and state information is consistent with the repository by executing “yum update kernel”.  
• A general "yum update" should be executed as long as the kernel update is included.  
• After executing the ‘yum update’ or ‘yum update kernel’ command, you must reboot the server.
• If using RHEL without a subscription, a DVD should be configured as the repo.     



2. **Execute the RMM Pre-installation script**

Download and execute the "rackware-pre-installation_v1.1.sh" script on the RMM Server.

This script must be executed from root or with sudo with root permissions.  It will generate a file called:
       rackware-pre-install-output-(date-time)

This file must be emailed to licensing@rackwareinc.com  RackWare will generate a license file called: 
rackware-license-(date-time)

Where the (date-time) is the original date-time in the original rackware-pre-install-output-(date-time).

Next, create the directory /etc/rackware, and copy the rackware-license-(date-time) to /etc/rackware/.






3. **Execute the Installation Script**

From root user, execute:
     ./rackware-(VERSION)-x86_64.sh

Read and accept the EULA and the Microsoft licenses by entering "yes".  

 Answer the prompts.   
There will be a series of prompts.    Accept the default values for all Yes/No questions, as well as for questions related to the VPN subnet, the GUI type,   unless instructed otherwise by RackWare.  

 Select the interfaces the RMM will use to interface with Client Hosts.

In general, select all available interfaces.  If using the Image mobility features (migrations and DR) see the section Image Mobility Requirements.

4. **Verify the RMM Installation**

A couple of commands will ensure that the software is installed correctly.  

This series of tests will:
• Verify that the RMM is running.
• Query general information about the RMM.

Verify the RMM is running correctly.    
   Execute: "$ rwadm status all"

*** Note that rwadm commands should be run from root.

The expected output is:
Dependent services:
 * running: httpd[(process id)]
 * running: dhcpd[(process id)] (may not be running)
 * running: xinetd[(process id)] (may not be running)
 * running: ietd[(process id)]
 * running: proftpd[(process id)]
 * running: syslog-ng[(process id)]
 * running: rmm[(process id)]
 * running: sshd[(process id)]

Services dhcp and xinetd are not required for the RMM to run, and will not be running unless configured otherwise.


Query the RMM.    
   Execute "$ rw rmm show"

This will display general information about the RMM.  An example:

[root@rjrmm2 tmp]# rw rmm show
RMM Info
  Software version  :  v5.0.0.4
  Build date/time   :  Feb  5 2016 at 08:37:17
  Uptime            :  2 minutes, 6 seconds
  Installation Time :  Wed Apr 15 09:10:25 MDT 2015
  Environment
    LocalConfigDB   :  /opt/rackware/data/rmm.db
    RemoteConfigDB  :  /opt/rackware/data/rmm.db
    RmmInterfaces   :  eth0
  Configuration
    Default boot method:  tftp
  Plugins
    None installed
Operation Statistics
    Total Host Captured     :  9
    Total Assign Operations :  10
    Total Data Migrations   :  0
    Total Hosts Discovered  :  0
License Data
    Version                                    :  2.0
    Type                                       :  LICENSE_TYPE_PROD
    License Install Date                       :  Sun Feb 14 14:53:56 2016
    Allowed Hosts for Discover                 :  100
    Allowed Hosts for Capture                  :  50
    Allowed Hosts for Assign/Sync2/DirectSync  :  50
    Allowed Hosts for DR Policy                :  0
    Allowed Hosts for Snapshot Configurations  :  0

Query the available storage available to the RMM
   Execute "$ rwcli pool show"

This will display the available storage to the RMM.

Name      Array         RAID        Size   
-------  ---------  ---------------  -----
default  array-iet  RAID_LEVEL_NONE  500 GB

If sufficient storage is not available to hold captured Images, captures will fail.


### Pricing
The costs associated with this Blueprint deployment are for the CenturyLink Cloud infrastructure only.  RackWare licensing information can be obtained by email info@rackwareinc.com.  

### About RackWare
CenturyLink Cloud works with [Rackware's web page](http://www.rackwareinc.com) to provide a software management suite for cloud called RackWare Management Module (RMM).  The RMM solution enables Enterprises to implement 4 major use cases: workload mobility, backup, disaster recovery, and AutoScaling.   


### Frequently Asked Questions

#### Who should I contact for support?
* For issues related to deploying the RackWare Management Module (RMM) Blueprint on CenturyLink Cloud, Licensing or Accessing the deployed software, please contact RackWare at:

support@rackwareinc.com
1 844-797-8776

* For issues related to cloud infrastructure (VM's, network, etc), or is you experience a problem deploying the Blueprint or Script Package, please open a CenturyLink Cloud Support ticket by emailing [noc@ctl.io](mailto:noc@ctl.io) or [through the CenturyLink Cloud Support website](https://t3n.zendesk.com/tickets/new).
