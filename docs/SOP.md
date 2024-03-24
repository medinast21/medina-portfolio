# SOP

## Installing and Configuring the Act! Web API Database Connection

### Overview
When a client has requested to have the Act! Web API configured for use with Act! Marketing Automation, Act!
Premium accessed via web, or 3rd-party connections, you will need to create a secure connection from their Act! database to the cloud API endpoint.

You have two options for creating this connection for an Act! database:

* Installing the Act! Connect Link software on the client's server or machine hosting the Act! database
* Installing Act! Premium for Web on the client's server / host machine and then installing and configuring an SSL
certificate on that same machine

This document helps the Act! consultant understand how to achieve both of these configurations.

!!! note
        The API connections for the Microsoft Outlook and Word add-ins (v22.1+) use a local service and do not
        require the endpoint to be configured to work properly. However, users do need to have the Web API Access
        permission added to their permissions in the Tools > Manage Users menu.

### The differences between Act! Connect Link and an SSL certificate
Things to consider when deciding which method to use:

* Act! Connect Link is easier to configure, as you simply install a piece of software and open any necessary firewall
ports.
* Using an SSL certificate requires more work to configure, as you need to obtain and then install the certificate
before you can set the API endpoint URL using the appropriate batch file.
* Both options can be used in conjunction with Act! Premium (Web) to set up web access to an Act! database.
* Act! Support officially does not recommend nor support Act! Premium for Web configurations using Act! Connect
Link to create an API endpoint URL (only recommended for use with non-web Act! Premium).
* SSL certificate configurations tend to be more stable and provide smoother functionality across services that
require the API endpoint connection (especially when dealing with databases that are large or have large data sets).

### Installing and configuring Act! Connect Link
Installing Act! Connect Link on a client's machine will create a secure ground-to-cloud connection from that machine
to the cloud server being hosted by the company providing this service.
To install Connect Link, download the installer from the Act! website and run the .exe file, accepting all of the default
options.
Once the software has been installed, a unique URL is generated for use as the database's API endpoint.
To view this unique URL that has been generated, open the Act! software.
Then, click the Act! Connect button in the column of options seen in the lower-left corner of the program window.
If the connection is working properly, you should see the URL displayed along the top of the program window.
This link can be copied to your clipboard using the button under the URL.

![Image place holder]()

!!! note
        The API URL should be updated in the configuration file in the Act! program folders automatically,
        but occasionally it does not get set properly. If this happens, you will need to use the GetSetCloudAPIURL.bat file
        found in the JWT token error ![KB article](https://help.act.com/hc/en-us/articles/360024432273-Error-Invalid-JWT-Token-when-accessing-Act-Marketing-Automation) to set the value manually.

#### Using ConnectLink with APFW
If the machine you have installed Connect Link on is using Act! Premium for Web, ensure the **Website Administration** settings have all been configured and tested successfully. You can find these settings in **Tools > Website Administration...**

To test a successful API connection to the Act! database, you can copy and paste the API URL into a web browser. If the connection is working properly, the Act! Web API Documentation webpage will display. If the client is using Act! Marketing Automation, you can also attempt to open the AMA module in Act!.

To verify a successful connection for clients to see, create a blank test campaign in AMA and attempt to select an Act! Group for the campaign. If the groups in the database appear for selection, you have a working API connection.

![Image place holder]()

#### Troubleshooting the Act! Connect Link connection
The Connect Link software operates over Port 80 (http), which is typically blocked by most organizations.
You can find a list of other ports to try opening if the connection issues persist by opening the Connect Link menu.
To do this, click the Windows Start button and search for "Act!".