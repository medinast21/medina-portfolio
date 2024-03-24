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

