---
title: Client settings
titleSuffix: Configuration Manager
description: Learn about the default and custom settings for controlling client behaviors
ms.date: 05/21/2019
ms.prod: configuration-manager
ms.technology: configmgr-client
ms.topic: conceptual
ms.assetid: f7560876-8084-4570-aeab-7fd44f4ba737
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.collection: M365-identity-device-management
---

# About client settings in Configuration Manager

*Applies to: System Center Configuration Manager (current branch)*

Manage all client settings in the Configuration Manager console from the **Client Settings** node in the **Administration** workspace. Configuration Manager comes with a set of default settings. When you change the default client settings, these settings are applied to all clients in the hierarchy. You can also configure custom client settings, which override the default client settings when you assign them to collections. For more information, see [How to configure client settings](/sccm/core/clients/deploy/configure-client-settings).

The following sections describe settings and options in further detail.  


## Background Intelligent Transfer Service (BITS)  

### Limit the maximum network bandwidth for BITS background transfers

When this option is **Yes**, clients use BITS bandwidth throttling. To configure the other settings in this group, you must enable this setting.

### Throttling window start time

Specify the local start time for the BITS throttling window.  

### Throttling window end time

Specify the local end time for the BITS throttling window. If the end time is equal to the **Throttling window start time**, BITS throttling is always enabled.  

### Maximum transfer rate during throttling window (Kbps)

Specify the maximum transfer rate that clients can use during the window.  

### Allow BITS downloads outside the throttling window

Allow clients to use separate BITS settings outside the specified window.  

### Maximum transfer rate outside the throttling window (Kbps)

Specify the maximum transfer rate that clients can use outside the BITS throttling window.  



## Client cache settings

### Configure BranchCache

Set up the client computer for [Windows BranchCache](/sccm/core/plan-design/configs/support-for-windows-features-and-networks#branchcache). To allow BranchCache caching on the client, set **Enable BranchCache** to **Yes**.

- **Enable BranchCache**: Enables BranchCache on client computers.

- **Maximum BranchCache cache size (percentage of disk)**: The percentage of the disk that you allow BranchCache to use.

### Configure client cache size

The Configuration Manager client cache on Windows computers stores temporary files used to install applications and programs. If this option is set to **No**, the default size is 5,120 MB.

If you choose **Yes**, then specify:

- **Maximum cache size (MB)**
- **Maximum cache size (percentage of disk)**: The client cache size expands to the maximum size in megabytes (MB), or the percentage of the disk, whichever is less.

### Enable Configuration Manager client in full OS to share content

Enables [peer cache](/sccm/core/plan-design/hierarchy/client-peer-cache) for Configuration Manager clients. Choose **Yes**, and then specify the port through which the client communicates with the peer computer.

- **Port for initial network broadcast** (default 8004): Configuration Manager uses this port in Windows PE or the full Windows OS. The task sequence engine in Windows PE sends the broadcast to get content locations before it starts the task sequence.<!--SCCMDocs issue 910-->

- **Port for content download from peer** (default 8003): Configuration Manager automatically configures Windows Firewall rules to allow this traffic. If you use a different firewall, you must manually configure rules to allow this traffic.



## Client policy  

### Client policy polling interval (minutes)

Specifies how frequently the following Configuration Manager clients download client policy:

- Windows computers (for example, desktops, servers, laptops)  
- Mobile devices that Configuration Manager enrolls  
- Mac computers  
- Computers that run Linux or UNIX  

This value is 60 minutes by default. Reducing this value causes clients to poll the site more frequently. With a lot of clients, this behavior can have a negative impact on the site performance. The [size and scale guidance](/sccm/core/plan-design/configs/size-and-scale-numbers) is based on the default value. Increasing this value causes clients to poll the site less often. Any changes to client policies, including new deployments, take longer for clients to download and process.<!-- SCCMDocs issue 823 -->

### Enable user policy on clients

When you set this option to **Yes**, and use [user discovery](/sccm/core/servers/deploy/configure/about-discovery-methods#bkmk_aboutUser), then clients receive applications and programs targeted to the signed-in user.  

The Application Catalog receives the list of available software for users from the site server. Thus, this setting doesn't have to be **Yes** for users to see and request applications from the Application Catalog. If this setting is **No**, users can't install the applications that they see in the Application Catalog.  

In addition, if this setting is **No**, users don't receive required applications that you deploy to users. Users also don't receive any other management tasks in user policies.  

This setting applies to users when their computer is on either the intranet or the internet. It must be **Yes** if you also want to enable user policies on the internet.  

### Enable user policy requests from internet clients

Set this option to **Yes** for users to receive the user policy on internet-based computers. The following requirements also apply:  

- The client and site are configured for [internet-based client management](/sccm/core/clients/manage/plan-internet-based-client-management) or a [cloud management gateway](/sccm/core/clients/manage/cmg/plan-cloud-management-gateway).  

- The **Enable user policy on clients** setting is **Yes**.  

- The internet-based management point successfully authenticates the user by using Windows authentication (Kerberos or NTLM). For more information, see [Considerations for client communications from the internet](/sccm/core/plan-design/hierarchy/communications-between-endpoints#BKMK_clientspan).  

- Starting in version 1710, the cloud management gateway successfully authenticates the user by using Azure Active Directory. For more information, see [Deploy user-available applications on Azure AD-joined devices](/sccm/apps/deploy-use/deploy-applications#deploy-user-available-applications-on-azure-ad-joined-devices).  

If you set this option to **No**, or any of the previous requirements aren't met, then a computer on the internet only receives computer policies. In this scenario, users can still see, request, and install applications from an internet-based Application Catalog. If this setting is **No**, but **Enable user policy on clients** is **Yes**, users don't receive user policies until the computer is connected to the intranet.  

> [!NOTE]  
> For internet-based client management, application approval requests from users don't require user policies or user authentication. The cloud management gateway doesn't support application approval requests.  



## Cloud services

### Allow access to cloud distribution point

Set this option to **Yes** for clients to obtain content from a cloud distribution point. This setting doesn't require the device to be internet-based.

### Automatically register new Windows 10 domain joined devices with Azure Active Directory

When you configure Azure Active Directory to support hybrid join, Configuration Manager configures Windows 10 devices for this functionality. For more information, see [How to configure hybrid Azure Active Directory joined devices](https://docs.microsoft.com/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup).

### Enable clients to use a cloud management gateway

By default, all internet-roaming clients use any available [cloud management gateway](/sccm/core/clients/manage/plan-cloud-management-gateway). An example of when to configure this setting to **No** is to scope usage of the service, such as during a pilot project or to save costs.



## Compliance settings  

### Enable compliance evaluation on clients

Set this option to **Yes** to configure the other settings in this group.

### Schedule compliance evaluation

Select **Schedule** to create the default schedule for configuration baseline deployments. This value is configurable for each baseline in the **Deploy Configuration Baseline** dialog box.  

### Enable User Data and Profiles

Choose **Yes** if you want to deploy [user data and profiles](/sccm/compliance/deploy-use/create-user-data-and-profiles-configuration-items) configuration items.



## Computer agent  

### User notifications for required deployments

For more information about the following three settings, see [User notifications for required deployments](/sccm/apps/deploy-use/deploy-applications#user-notifications-for-required-deployments):

- **Deployment deadline greater than 24 hours, remind user every (hours)**
- **Deployment deadline less than 24 hours, remind user every (hours)**
- **Deployment deadline less than 1 hour, remind user every (minutes)**

### Default Application Catalog website point

> [!Note]  
> Starting in version 1806, the application catalog website point is no longer *required*, but still *supported*. For more information, see [Configure Software Center](/sccm/apps/plan-design/plan-for-software-center#bkmk_userex).
>
> The **Silverlight user experience** for the application catalog website point is no longer supported. For more information, see [Removed and deprecated features](/sccm/core/plan-design/changes/deprecated/removed-and-deprecated-cmfeatures).  

Configuration Manager uses this setting to connect users to the Application Catalog from Software Center. Select **Set Website** to specify a server that hosts the Application Catalog website point. Enter its NetBIOS name or FQDN, specify automatic detection, or specify a URL for customized deployments. In most cases, automatic detection is the best choice.

### Add default Application Catalog website to Internet Explorer trusted sites zone

> [!Note]  
> Starting in version 1806, the application catalog website point is no longer *required*, but still *supported*. For more information, see [Configure Software Center](/sccm/apps/plan-design/plan-for-software-center#bkmk_userex).
>
> The **Silverlight user experience** for the application catalog website point is no longer supported. For more information, see [Removed and deprecated features](/sccm/core/plan-design/changes/deprecated/removed-and-deprecated-cmfeatures).  

If this option is **Yes**, the client automatically adds the current default Application Catalog website URL to the Internet Explorer trusted sites zone.  

This setting ensures that the Internet Explorer setting for Protected Mode isn't enabled. If Protected Mode is enabled, the Configuration Manager client might not be able to install applications from the Application Catalog. By default, the trusted sites zone also supports user sign-in for the Application Catalog, which requires Windows authentication.  

If you leave this option as **No**, Configuration Manager clients might not be able to install applications from the Application Catalog. An alternative method is to configure these Internet Explorer settings in another zone for the Application Catalog URL that clients use.  

### Allow Silverlight applications to run in elevated trust mode

> [!Important]  
> Starting in Configuration Manager version 1802, the client doesn't automatically install Silverlight.
>
> Starting in version 1806, the **Silverlight user experience** for the application catalog website point is no longer supported. Users should use the new Software Center. For more information, see [Configure Software Center](/sccm/apps/plan-design/plan-for-software-center#bkmk_userex).  

This setting must be **Yes** for users to use the Application Catalog.  

If you change this setting, it takes effect when users next load their browser, or refresh their currently opened browser window.  

For more information about this setting, see [Certificates for Microsoft Silverlight 5, and elevated trust mode required for the application catalog](/sccm/apps/plan-design/security-and-privacy-for-application-management#BKMK_CertificatesSilverlight5).  

### Organization name displayed in Software Center

Type the name that users see in Software Center. This branding information helps users to identify this application as a trusted source. For more information about the priority of this setting, see [Branding Software Center](/sccm/apps/plan-design/plan-for-and-configure-application-management#branding-software-center).  

### Use new Software Center

Starting in Configuration Manager 1802, the default setting is **Yes**.

If you set this option to **Yes**, then all client computers use the Software Center. Software Center shows user-available apps that were previously accessible only in the Application Catalog. The Application Catalog requires Silverlight, which isn't a prerequisite for the Software Center.

Starting in version 1806, the application catalog website point and web service point roles are no longer *required*, but still *supported*. For more information, see [Configure Software Center](/sccm/apps/plan-design/plan-for-software-center#bkmk_userex).

> [!Note]  
> The **Silverlight user experience** for the application catalog website point is no longer supported. For more information, see [Removed and deprecated features](/sccm/core/plan-design/changes/deprecated/removed-and-deprecated-cmfeatures).  

### Enable communication with Health Attestation Service

Set this option to **Yes** for Windows 10 devices to use [Health attestation](/sccm/core/servers/manage/health-attestation). When you enable this setting, the following setting is also available for configuration.

### Use on-premises Health Attestation Service

Set this option to **Yes** for devices to use an on-premises service. Set to **No** for devices to use the Microsoft cloud-based service.  

### Install permissions

> [!IMPORTANT]  
> This setting applies to the Application Catalog and Software Center. This setting has no effect when users use the Company Portal.  

Configure how users can initiate the installation of software, software updates, and task sequences:  

- **All Users**: Users with any permission except Guest.  

- **Only Administrators**: Users must be a member of the local Administrators group.  

- **Only Administrators and primary users**: Users must be a member of the local Administrators group, or a primary user of the computer.  

- **No Users**: No users signed in to a client computer can initiate the installation of software, software updates, and task sequences. Required deployments for the computer always install at the deadline. Users can't initiate the installation of software from the Application Catalog or Software Center.  

### Suspend BitLocker PIN entry on restart

If computers require BitLocker PIN entry, then this option bypasses the requirement to enter a PIN when the computer restarts after a software installation.  

- **Always**: Configuration Manager temporarily suspends BitLocker after it has installed software that requires a restart, and has initiated a restart of the computer. This setting applies only to a computer restart initiated by Configuration Manager. This setting doesn't suspend the requirement to enter the BitLocker PIN when the user restarts the computer. The BitLocker PIN entry requirement resumes after Windows startup.

- **Never**: Configuration Manager doesn't suspend BitLocker after it has installed software that requires a restart. In this scenario, the software installation can't finish until the user enters the PIN to complete the standard startup process and load Windows.

### Additional software manages the deployment of applications and software updates

Enable this option only if one of the following conditions applies:  

- You use a vendor solution that requires this setting to be enabled.  

- You use the Configuration Manager software development kit (SDK) to manage client agent notifications, and the installation of applications and software updates.  

> [!WARNING]  
> If you choose this option when neither of these conditions apply, the client doesn't install software updates and required applications. This setting doesn't prevent users from installing applications from the Application Catalog, or the installation of packages, programs, and task sequences.  

### PowerShell execution policy

Configure how Configuration Manager clients can run Windows PowerShell scripts. You might use these scripts for detection in configuration items for compliance settings. You might also send the scripts in a deployment as a standard script.  

- **Bypass**: The Configuration Manager client bypasses the Windows PowerShell configuration on the client computer, so that unsigned scripts can run.  

- **Restricted**: The Configuration Manager client uses the current PowerShell configuration on the client computer. This configuration determines whether unsigned scripts can run.  

- **All Signed**: The Configuration Manager client runs scripts only if a trusted publisher has signed them. This restriction applies independently from the current PowerShell configuration on the client computer.  

This option requires at least Windows PowerShell version 2.0. The default is **All Signed**.  

> [!TIP]  
> If unsigned scripts fail to run because of this client setting, Configuration Manager reports this error in the following ways:  
>
> - The **Monitoring** workspace in the console displays deployment status error ID **0x87D00327**. It also displays the description **Script is not signed**.  
> - Reports display the error type **Discovery Error**. Then reports display either error code **0x87D00327** and the description **Script is not signed**, or error code **0x87D00320** and the description **The script host has not been installed yet**. An example report is: **Details of errors of configuration items in a configuration baseline for an asset**.  
> - The **DcmWmiProvider.log** file displays the message **Script is not signed (Error: 87D00327; Source: CCM)**.  

### Show notifications for new deployments

Choose **Yes** to display a notification for deployments available for less than a week. This message appears each time the client agent starts.

### Disable deadline randomization

After the deployment deadline, this setting determines whether the client uses an activation delay of up to two hours to install required software updates. By default, the activation delay is disabled.  

For virtual desktop infrastructure (VDI) scenarios, this delay helps distribute the CPU processing and data transfer for a host machine with multiple virtual machines. Even if you don't use VDI, having many clients installing the same updates at the same time can negatively increase CPU usage on the site server. This behavior can also slow down distribution points, and significantly reduce the available network bandwidth.  

If clients must install required software updates at the deployment deadline without delay, then configure this setting to **Yes**.

### Grace period for enforcement after deployment deadline (hours)

If you want to give users more time to install required application or software update deployments beyond the deadline, set this option to **Yes**. This grace period is for a computer turned off for an extended time, and the user needs to install many application or update deployments. For example, this setting is helpful if a user returns from vacation, and has to wait for a long time while the client installs overdue application deployments.

Set a grace period of 1 to 120 hours. Use this setting along with the deployment property **Delay enforcement of this deployment according to user preferences**. For more information, see [Deploy applications](/sccm/apps/deploy-use/deploy-applications#delay-enforcement-with-a-grace-period).


## Computer restart

The following settings must be shorter in duration than the shortest maintenance window applied to the computer:

- **Display a temporary notification to the user that indicates the interval before the user is logged off or the computer restarts (minutes)**
- **Display a dialog box that the user cannot close, which displays the countdown interval before the user is logged off or the computer restarts (minutes)**

For more information about maintenance windows, see [How to use maintenance windows](/sccm/core/clients/manage/collections/use-maintenance-windows).

**When a deployment requires a restart, show a dialog window to the user instead of a toast notification**<!--3555947-->: Starting in version 1902, configuring this setting to **Yes** changes the user experience to be more intrusive. This setting applies to all deployments of applications, task sequences, and software updates. For more information, see [Plan for Software Center](/sccm/apps/plan-design/plan-for-software-center#bkmk_impact).



## Delivery Optimization

<!-- 1324696 -->
You use Configuration Manager boundary groups to define and regulate content distribution across your corporate network and to remote offices. [Windows Delivery Optimization](https://docs.microsoft.com/windows/deployment/update/waas-delivery-optimization) is a cloud-based, peer-to-peer technology to share content between Windows 10 devices. Starting in version 1802, configure Delivery Optimization to use your boundary groups when sharing content among peers.

> [!Note]
> Delivery Optimization is only available on Windows 10 clients

### Use Configuration Manager Boundary Groups for Delivery Optimization Group ID

Choose **Yes** to apply the boundary group identifier as the Delivery Optimization group identifier on the client. When the client communicates with the Delivery Optimization cloud service, it uses this identifier to locate peers with the desired content.



## Endpoint Protection

> [!Tip]
> In addition to the following information, you can find details about using Endpoint Protection client settings in [Example scenario: Using Endpoint Protection to protect computers from malware](/sccm/protect/deploy-use/scenarios-endpoint-protection).

### Manage Endpoint Protection client on client computers

Choose **Yes** if you want to manage existing Endpoint Protection and Windows Defender clients on computers in your hierarchy.  

Choose this option if you've already installed the Endpoint Protection client, and want to manage it with Configuration Manager. This separate installation includes a scripted process that uses a Configuration Manager application or package and program. Starting in Configuration Manager 1802, Windows 10 devices don't need to have the Endpoint Protection agent installed. However, those devices will still need **Manage Endpoint Protection client on client computers** enabled. <!--503654-->

### Install Endpoint Protection client on client computers

Choose **Yes** to install and enable the Endpoint Protection client on client computers that aren't already running the client. Starting in Configuration Manager 1802, Windows 10 clients don't need to have the Endpoint Protection agent installed.  

> [!NOTE]  
> If the Endpoint Protection client is already installed, choosing **No** doesn't uninstall the Endpoint Protection client. To uninstall the Endpoint Protection client, set the **Manage Endpoint Protection client on client computers** client setting to **No**. Then, deploy a package and program to uninstall the Endpoint Protection client.  

### Allow Endpoint Protection client installation and restarts outside maintenance windows. Maintenance windows must be at least 30 minutes long for client installation

Set this option to **Yes** to override typical installation behaviors with maintenance windows. This setting meets business requirements for the priority of system maintenance for security purposes.

### For Windows Embedded devices with write filters, commit Endpoint Protection client installation (requires restarts)

Choose **Yes** to disable the write filter on the Windows Embedded device, and restart the device. This action commits the installation on the device.  

If you choose **No**, the client installs on a temporary overlay that clears when the device restarts. In this scenario, the Endpoint Protection client doesn't fully install until another installation commits changes to the device. This configuration is the default.  

### Suppress any required computer restarts after the Endpoint Protection client is installed

Choose **Yes** to suppress a computer restart after the Endpoint Protection client installs.  

> [!IMPORTANT]  
> If the Endpoint Protection client requires a computer restart and this setting is **No**, then the computer restarts regardless of any configured maintenance windows.  

### Allowed period of time users can postpone a required restart to complete the Endpoint Protection installation (hours)

If a restart is necessary after the Endpoint Protection client installs, this setting specifies the number of hours that users can postpone the required restart. This setting requires that the setting for **Suppress any required computer restarts after the Endpoint Protection client is installed** is **No**.  

### Disable alternate sources (such as Microsoft Windows Update, Microsoft Windows Server Update Services, or UNC shares) for the initial definition update on client computers

Choose **Yes** if you want Configuration Manager to install only the initial definition update on client computers. This setting can be helpful to avoid unnecessary network connections, and reduce network bandwidth, during the initial installation of the definition update.  



## Enrollment

### Polling interval for mobile device legacy clients

Select **Set Interval** to specify the length of time, in minutes or hours, that legacy mobile devices poll for policy. These devices include platforms such as Windows CE, Mac OS X, and Unix or Linux.

### Polling interval for modern devices (minutes)

Enter the number of minutes that modern devices poll for policy. This setting is for Windows 10 devices that are managed through on-premises mobile device management.

### Allow users to enroll mobile devices and Mac computers

To enable user-based enrollment of legacy devices, set this option to **Yes**, and then configure the following setting:

- **Enrollment profile**: Select **Set Profile** to create or select an enrollment profile. For more information, see [Configure client settings for enrollment](/sccm/core/clients/deploy/deploy-clients-to-macs#configure-client-settings-for-enrollment).

### Allow users to enroll modern devices

To enable user-based enrollment of modern devices, set this option to **Yes**, and then configure the following setting:

- **Modern device enrollment profile**: Select **Set Profile** to create or select an enrollment profile. For more information, see [Create an enrollment profile that allows users to enroll modern devices](/sccm/mdm/get-started/set-up-device-enrollment-on-premises-mdm#bkmk_createProf).



## Hardware inventory  

### Enable hardware inventory on clients

By default, this setting is **Yes**. For more information, see [Introduction to hardware inventory](/sccm/core/clients/manage/inventory/introduction-to-hardware-inventory).

### Hardware inventory schedule

Select **Schedule** to adjust the frequency that clients run the hardware inventory cycle. By default, this cycle occurs every seven days.

### Maximum random delay (minutes)

Specify the maximum number of minutes for the Configuration Manager client to randomize the hardware inventory cycle from the defined schedule. This randomization across all clients helps load-balance inventory processing on the site server. You can specify any value between 0 and 480 minutes. By default, this value is set to 240 minutes (4 hours).

### Maximum custom MIF file size (KB)

Specify the maximum size, in kilobytes (KB), allowed for each custom Management Information Format (MIF) file that the client collects during a hardware inventory cycle. The Configuration Manager hardware inventory agent doesn't process any custom MIF files that exceed this size. You can specify a size of 1 KB to 5,120 KB. By default, this value is set to 250 KB. This setting doesn't affect the size of the regular hardware inventory data file.  

> [!NOTE]  
> This setting is available only in the default client settings.  

### Hardware inventory classes

Select **Set Classes** to extend the hardware information that you collect from clients without manually editing the sms_def.mof file. For more information, see [How to configure hardware inventory](/sccm/core/clients/manage/inventory/configure-hardware-inventory).  

### Collect MIF files

Use this setting to specify whether to collect MIF files from Configuration Manager clients during hardware inventory.  

For a MIF file to be collected by hardware inventory, it must be in the correct location on the client computer. By default, the files are located in the following paths:  

- **IDMIF files** should be in the Windows\System32\CCM\Inventory\Idmif folder.

- **NOIDMIF files** should be in the Windows\System32\CCM\Inventory\Noidmif folder.

> [!NOTE]  
> This setting is available only in the default client settings.



## Metered internet connections  

Manage how Windows 8 and later computers use metered internet connections to communicate with Configuration Manager. Internet providers sometimes charge by the amount of data that you send and receive when you are on a metered internet connection.  

> [!NOTE]  
> The configured client setting isn't applied in the following scenarios:  
>
> - If the computer is on a roaming data connection, the Configuration Manager client doesn't perform any tasks that require data to be transferred to Configuration Manager sites.  
> - If the Windows network connection properties are configured as non-metered, the Configuration Manager client behaves as if the connection is non-metered, and so transfers data to the site.  

### Client communication on metered internet connections

Choose one of the following options for this setting:  

- **Allow**: All client communications are allowed over the metered internet connection, unless the client device is using a roaming data connection.  

- **Limit**: Only the following client communications are allowed over the metered internet connection:  

    - Client policy retrieval  

    - Client state messages to send to the site  

    - Software installation requests by using the Application Catalog  

    - Required deployments (when the installation deadline is reached)  

    > [!IMPORTANT]  
    > The client always permits software installations from Software Center or the Application Catalog, regardless of the metered internet connection settings.  

    If the client reaches the data transfer limit for the metered internet connection, the client no longer tries to communicate with Configuration Manager sites.  

- **Block**: The Configuration Manager client doesn't try to communicate with Configuration Manager sites when it's on a metered internet connection. This option is the default.  



## Power management  

### Allow power management of devices

Set this option to **Yes** to enable power management on clients. For more information, see [Introduction to power management](/sccm/core/clients/manage/power/introduction-to-power-management).

### Allow users to exclude their device from power management

Choose **Yes** to let users of Software Center exclude their computer from any configured power management settings.  

### Allow network wake-up

Added in 1810. When set to **Enable**, configures the power settings on the network adapter to allow the network adapter to wake up the device. When set to **Disable**, the power settings on the network adapter are configured not to allow the network adapter to wake up the device.

### Enable wake-up proxy

Specify **Yes** to supplement the site’s Wake On LAN setting, when it's configured for unicast packets.  

For more information about wake-up proxy, see [Plan how to wake up clients](/sccm/core/clients/deploy/plan/plan-wake-up-clients).  

> [!WARNING]  
> Don't enable wake-up proxy in a production network without first understanding how it works and evaluating it in a test environment.  

Then, configure the following additional settings as needed:

- **Wake-up proxy port number (UDP)**: The port number that clients use to send wake-up packets to sleeping computers. Keep the default port 25536, or change the number to a value of your choice.  

- **Wake On LAN port number (UDP)**: Keep the default value of 9, unless you've changed the Wake On LAN (UDP) port number on the **Ports** tab of the site **Properties**.  

    > [!IMPORTANT]  
    > This number must match the number in the site **Properties**. If you change this number in one place, it isn't automatically updated in the other place.  

- **Windows Defender Firewall exception for wake-up proxy**: The Configuration Manager client automatically configures the wake-up proxy port number on devices that run Windows Defender Firewall. Select **Configure** to specify the desired firewall profiles.  

    If clients run a different firewall, manually configure it to allow the **Wake-up proxy port number (UDP)**.  

- **IPv6 prefixes if required for DirectAccess or other intervening network devices. Use a comma to specify multiple entries**: Enter the necessary IPv6 prefixes for wake-up proxy to function on your network.



## Remote tools  

### Enable Remote Control on clients, and Firewall exception profiles

Select **Configure** to enable the Configuration Manager remote control feature. Optionally, configure firewall settings to allow remote control to work on client computers.  

Remote control is disabled by default.  

> [!IMPORTANT]  
> If you don't configure firewall settings, remote control might not work correctly.  

### Users can change policy or notification settings in Software Center

Choose whether users can change remote control options from within Software Center.  

### Allow Remote Control of an unattended computer

Choose whether an admin can use remote control to access a client computer that is logged off or locked. Only a logged-on and unlocked computer can be remotely controlled when this setting is disabled.  

### Prompt user for Remote Control permission

Choose whether the client computer shows a message asking for the user's permission before allowing a remote control session.  

### Prompt user for permission to transfer content from shared clipboard

Before transferring content from the shared clipboard in a remote control session, allow your users the opportunity to accept or deny file transfers. Users only need to grant permission once per session. The viewer can't give themselves permission to transfer the file.

### Grant Remote Control permission to local Administrators group

Choose whether local admins on the server that initiates the remote control connection can establish remote control sessions to client computers.  

### Access level allowed

Specify the level of remote control access to allow. Choose from the following settings:  

- **No Access**
- **View Only**
- **Full Control**  

### Permitted viewers of Remote Control and Remote Assistance

Select **Set Viewers** to specify the names of the Windows users who can establish remote control sessions to client computers.  

### Show session notification icon on taskbar

Configure this setting to **Yes** to show an icon on the client's Windows taskbar to indicate an active remote control session.  

### Show session connection bar

Set this option to **Yes** to show a high-visibility session connection bar on clients, to indicate an active remote control session.  

### Play a sound on client

Set this option to use sound to indicate when a remote control session is active on a client computer. Select one of the following options:

- **No sound**
- **Beginning and send of session** (default)
- **Repeatedly during session**  

### Manage unsolicited Remote Assistance settings

Configure this setting to **Yes** to let Configuration Manager manage unsolicited Remote Assistance sessions.  

In an unsolicited Remote Assistance session, the user at the client computer didn't request assistance to initiate the session.  

### Manage solicited Remote Assistance settings

Set this option to **Yes** to let Configuration Manager manage solicited Remote Assistance sessions.  

In a solicited Remote Assistance session, the user at the client computer sent a request to the admin for remote assistance.  

### Level of access for Remote Assistance

Choose the level of access to assign to Remote Assistance sessions that are initiated in the Configuration Manager console. Select one of the following options:

- **None** (default)
- **Remote Viewing**
- **Full Control**

> [!NOTE]  
> The user at the client computer must always grant permission for a Remote Assistance session to occur.  

### Manage Remote Desktop settings

Set this option to **Yes** to let Configuration Manager manage Remote Desktop sessions for computers.  

### Allow permitted viewers to connect by using Remote Desktop connection

Set this option to **Yes** to add users specified in the permitted viewer list to the Remote Desktop local user group on clients.  

### Require network level authentication on computers that run Windows Vista operating system and later versions

Set this option to **Yes** to use network-level authentication (NLA) to establish Remote Desktop connections to client computers. NLA initially requires fewer remote computer resources, because it finishes user authentication before it establishes a Remote Desktop connection. Using NLA is a more secure configuration. NLA helps protect the computer from malicious users or software, and it reduces the risk from denial-of-service attacks.  



## Software Center

### Select these new settings to specify company information

Set this option to **Yes**, and then specify the following settings to brand Software Center for your organization:

- **Company name**: Enter the organization name that users see in Software Center.  

- **Color scheme for Software Center**: Click **Select Color** to define the primary color used by Software Center.  

- **Select a logo for Software Center**: Click **Browse** to select an image to appear in Software Center. The logo must be a JPEG, PNG, or BMP of 400 x 100 pixels, with a maximum size of 750 KB. The logo file name shouldn't contain spaces.  

### <a name="bkmk_HideUnapproved"></a> Hide unapproved applications in Software Center

Starting in Configuration Manager version 1802, when you enable this option, user-available applications that require approval are hidden in Software Center.<!--1355146-->

### <a name="bkmk_HideInstalled"></a> Hide installed applications in Software Center

Starting in Configuration Manager version 1802, when you enable this option, applications that are already installed no longer show in the Applications tab. This option is set as the default when you install or upgrade to Configuration Manager 1802. Installed applications are still available for review under the installation status tab. <!--1357592-->

### <a name="bkmk_HideAppCat"></a> Hide Application Catalog link in Software Center

Starting in Configuration Manager version 1806, you can specify the visibility of the Application Catalog web site link in Software Center. When this option is set, users won't see the Application Catalog web site link in the Installation status node of Software Center. <!--1358214-->


### Software Center tab visibility

Configure the additional settings in this group to **Yes** to make the following tabs visible in Software Center:

- **Applications**
- **Updates**
- **Operating Systems**
- **Installation Status**
- **Device Compliance**
- **Options**
- **Specify a custom tab for Software Center** (starting in version 1806) <!--1358132-->
    - **Tab name**
    - **Content URL**

    >[!Important]  
    > Some website features may not work when using it as a custom tab in Software Center. Make sure to test the results before deploying this to clients. <!--519659-->
    >
    > Specify only trusted or intranet website addresses when you add a custom tab.<!--SCCMDocs issue 1575-->

For example, if your organization doesn't use compliance policies, and you want to hide the Device Compliance tab in Software Center, set **Enable Device Compliance tab** to **No**.



## Software deployment  

### Schedule re-evaluation for deployments

Configure a schedule for when Configuration Manager reevaluates the requirement rules for all deployments. The default value is every seven days.  

> [!IMPORTANT]  
> This setting is more invasive to the local client than it is to the network or site server. A more aggressive reevaluation schedule negatively affects the performance of your network and client computers. Microsoft doesn't recommend setting a lower value than the default. If you change this value, closely monitor performance.  

Initiate this action from a client as follows: in the **Configuration Manager** control panel, from the **Actions** tab, select **Application Deployment Evaluation Cycle**.  



## Software inventory  

### Enable software inventory on clients

This option is set to **Yes** by default. For more information, see [Introduction to software inventory](/sccm/core/clients/manage/inventory/introduction-to-software-inventory).

### Schedule software inventory and file collection

Select **Schedule** to adjust the frequency that clients run the software inventory and file collection cycles. By default, this cycle occurs every seven days.

### Inventory reporting detail

Specify one of the following levels of file information to inventory:

- **File only**
- **Product only**
- **Full details** (default)

### Inventory these file types

If you want to specify the types of file to inventory, select **Set Types**, and then configure the following options:  

> [!NOTE]  
> If multiple custom client settings are applied to a computer, the inventory that each setting returns is merged.  

- Select **New** to add a new file type to inventory. Then specify the following information in the **Inventoried File Properties** dialog box:  

    - **Name**: Provide a name for the file that you want to inventory. Use an asterisk (`*`) wildcard to represent any string of text, and a question mark (`?`) to represent any single character. For example, if you want to inventory all files with the extension .doc, specify the file name `*.doc`.  

    - **Location**: Select **Set** to open the **Path Properties** dialog box. Configure software inventory to search all client hard disks for the specified file, search a specified path (for example, `C:\Folder`), or search for a specified variable (for example, `%windir%`). You can also search all subfolders under the specified path.  

    - **Exclude encrypted and compressed files**: When you choose this option, any compressed or encrypted files aren't inventoried.  

    - **Exclude files in the Windows folder**: When you choose this option, any files in the Windows folder and its subfolders aren't inventoried.  

    Select **OK** to close the **Inventoried File Properties** dialog box. Add all the files that you want to inventory, and then select **OK** to close the **Configure Client Setting** dialog box.  

### Collect files

If you want to collect files from client computers, select **Set Files**, and then configure the following settings:  

> [!NOTE]  
> If multiple custom client settings are applied to a computer, the inventory that each setting returns is merged.  

- In the **Configure Client Setting** dialog box, select **New** to add a file to be collected.  

- In the **Collected File Properties** dialog box, provide the following information:  

    - **Name**: Provide a name for the file that you want to collect. Use an asterisk (`*`) wildcard to represent any string of text, and a question mark (`?`) to represent any single character.  

    - **Location**: Select **Set** to open the **Path Properties** dialog box. Configure software inventory to search all client hard disks for the file that you want to collect, search a specified path (for example, `C:\Folder`), or search for a specified variable (for example, `%windir%`). You can also search all subfolders under the specified path.  

    - **Exclude encrypted and compressed files**: When you choose this option, any compressed or encrypted files aren't collected.  

    - **Stop file collection when the total size of the files exceeds (KB)**: Specify the file size, in kilobytes (KB), after which the client stops collecting the specified files.  

    > [!NOTE]  
    > The site server collects the five most recently changed versions of collected files, and stores them in the `<ConfigMgr installation directory>\Inboxes\Sinv.box\Filecol` directory. If a file hasn't changed since the last software inventory cycle, the file isn't collected again.  
    >
    > Software inventory doesn't collect files larger than 20 MB.  
    >
    > The value **Maximum size for all collected files (KB)** in the **Configure Client Setting** dialog box shows the maximum size for all collected files. When this size is reached, file collection stops. Any files already collected are retained and sent to the site server.  

    > [!IMPORTANT]
    > If you configure software inventory to collect many large files, this configuration might negatively affect the performance of your network and site server.  

    For information about how to view collected files, see [How to use Resource Explorer to view software inventory](/sccm/core/clients/manage/inventory/use-resource-explorer-to-view-software-inventory).  

    Select **OK** to close the **Collected File Properties** dialog box. Add all the files that you want to collect, and then select **OK** to close the **Configure Client Setting** dialog box.  

### Set Names

The software inventory agent retrieves manufacturer and product names from file header information. These names aren't always standardized in the file header information. When you view software inventory in Resource Explorer, different versions of the same manufacturer or product name can appear. To standardize these display names, select **Set Names**, and then configure the following settings:  

- **Name type**: Software inventory collects information about both manufacturers and products. Choose whether you want to configure display names for a **Manufacturer** or a **Product**.  

- **Display name**: Specify the display name that you want to use in place of the names in the **Inventoried names** list. To specify a new display name, select **New**.  

- **Inventoried names**: To add an inventoried name, select **New**. This name is replaced in software inventory by the name chosen in the **Display name** list. You can add multiple names to replace.  



## Software Metering

### Enable software metering on clients

This setting is set to **Yes** by default. For more information, see [Software metering](/sccm/apps/deploy-use/monitor-app-usage-with-software-metering#configure-software-metering).

### Schedule data collection

Select **Schedule** to adjust the frequency that clients run the software metering cycle. By default, this cycle occurs every seven days.



## Software updates  

### Enable software updates on clients

Use this setting to enable software updates on Configuration Manager clients. When you disable this setting, Configuration Manager removes existing deployment policies from clients. When you re-enable this setting, the client downloads the current deployment policy.  

> [!IMPORTANT]  
> When you disable this setting, compliance policies that rely on software updates will no longer function.  

### Software update scan schedule

Select **Schedule** to specify how often the client initiates a compliance assessment scan. This scan determines the state for software updates on the client (for example, required or installed). For more information about compliance assessment, see [Software updates compliance assessment](/sccm/sum/understand/software-updates-introduction#BKMK_SUMCompliance).  

By default, this scan uses a simple schedule to initiate every seven days. You can create a custom schedule. You can specify an exact start day and time, use Universal Coordinated Time (UTC) or the local time, and configure the recurring interval for a specific day of the week.  

> [!NOTE]  
> If you specify an interval of less than one day, Configuration Manager automatically defaults to one day.  

> [!WARNING]  
> The actual start time on client computers is the start time plus a random amount of time, up to two hours. This randomization prevents client computers from initiating the scan and simultaneously connecting to the active software update point.  

### Schedule deployment re-evaluation

Select **Schedule** to configure how often the software updates client agent reevaluates software updates for installation status on Configuration Manager client computers. When previously installed software updates are no longer found on clients but are still required, the client reinstalls the software updates.

Adjust this schedule based on company policy for software update compliance, and whether users can uninstall software updates. Every deployment re-evaluation cycle results in network and client computer processor activity. By default, this setting uses a simple schedule to initiate the deployment re-evaluation scan every seven days.  

> [!NOTE]  
> If you specify an interval of less than one day, Configuration Manager automatically defaults to one day.  

### When any software update deployment deadline is reached, install all other software update deployments with deadline coming within a specified period of time

Set this option to **Yes** to install all software updates from required deployments with deadlines occurring within a specified period of time. When a required software update deployment reaches a deadline, the client initiates installation for the software updates in the deployment. This setting determines whether to install software updates from other required deployments that have a deadline within the specified time.  

Use this setting to expedite installation for required software updates. This setting also has the potential to increase client security, decrease notifications to the user, and decrease client restarts. By default, this setting is set to **No**.  

### Period of time for which all pending deployments with deadline in this time will also be installed

Use this setting to specify the period of time for the previous setting. You can enter a value from 1 to 23 hours, and from 1 to 365 days. By default, this setting is configured for seven days.  

### Enable installation of Express installation files on clients

Set this option to **Yes** to allow clients to use express installation files. For more information, see [Manage Express installation files for Windows 10 updates](/sccm/sum/deploy-use/manage-express-installation-files-for-windows-10-updates).


### Port used to download content for Express installation files

This setting configures the local port for the HTTP listener to download express content. It's set to 8005 by default. You don't need to open this port in the client firewall.

### Enable management of the Office 365 Client Agent

When you set this option to **Yes**, it enables the configuration of Office 365 installation settings. It also enables downloading files from Office Content Delivery Networks (CDNs), and deploying the files as an application in Configuration Manager. For more information, see [Manage Office 365 ProPlus](/sccm/sum/deploy-use/manage-office-365-proplus-updates).

### <a name="bkmk_SUMMaint"></a> Enable installation of software updates in "All deployments" maintenance window when "Software Update" maintenance window is available

When you set this option to **Yes** and the client has at least one "Software Update" maintenance window defined, software updates will install during an "All deployments" maintenance window. By default, this setting is set to **No**. This client setting was added in Configuration Manager version 1810. <!--2839307-->

### <a name="bkmk_thread-priority"></a> Specify thread priority for feature updates

<!--3734525-->
Starting in Configuration Manager version 1902, you can adjust the priority with which Windows 10 version 1709 or later clients install a feature update through [Windows 10 servicing](/sccm/osd/deploy-use/manage-windows-as-a-service). This setting has no impact on Windows 10 in-place upgrade task sequences.

This client setting provides the following options:

- **Not Configured**: Configuration Manager doesn't change the setting. Admins can pre-stage their own setupconfig.ini file. This value is the default.

- **Normal**: Windows Setup uses more system resources and updates faster. It uses more processor time, so the total installation time is shorter, but the user's outage is longer.  

    - Configures the setupconfig.ini file on the device with the `/Priority Normal` [Windows setup command-line option](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-setup-command-line-options).

- **Low**: You can continue to work on the device while it downloads and updates in the background. The total installation time is longer, but the user's outage is shorter. You may need to increase the update max run time to avoid a time out when using this option.  

    - Removes the `/Priority` [Windows setup command-line option](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-setup-command-line-options) from the setupconfig.ini file.


### Enable third party software updates

When you set this option to **Yes**, it sets the policy for **Allow signed updates for an intranet Microsoft update service location** and installs the signing certificate to the Trusted Publisher store on the client. This client setting was added in Configuration Manager version 1802.

## State Messaging

### State message reporting cycle (minutes)

Specifies how often clients report state messages. This setting is 15 minutes by default.



## User and device affinity  

### User device affinity usage threshold (minutes)

Specify the number of minutes before Configuration Manager creates a user device affinity mapping. By default, this value is 2880 minutes (two days).

### User device affinity usage threshold (days)

Specify the number of days over which the client measures the threshold for usage-based device affinity. By default, this value is 30 days.

> [!NOTE]  
> For example, you specify **User device affinity usage threshold (minutes)** as **60** minutes, and **User device affinity usage threshold (days)** as **5** days. Then the user must use the device for 60 minutes over a period of 5 days to create automatic affinity with the device.  

### Automatically configure user device affinity from usage data

Choose **Yes** to create automatic user device affinity based on the usage information that Configuration Manager collects.  

### Allow user to define their primary devices

When this setting is **Yes**, users can identify their own primary devices in Software Center.



## Windows Analytics

For more information on these settings, see [Configure Clients to report data to Windows Analytics](/sccm/core/clients/manage/monitor-windows-analytics#configure-clients-to-report-data-to-windows-analytics).
