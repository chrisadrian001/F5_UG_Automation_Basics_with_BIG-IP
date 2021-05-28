Lab 1 - Imperative Automation with iControl
===========================================

Objectives
----------

The intention of this lab will be to show how to work with imperative API calls.


Setup Lab Environment
-----------------------------------

To access your dedicated student lab environment, you will need a web browser and Remote Desktop Protocol (RDP) client software. The web browser will be used to access the Unified Demo Framework (UDF) Training Portal. The RDP client will be used to connect to the jumphost, where you will be able to access the BIG-IP management interfaces (HTTPS, SSH).

#. Click DEPLOYMENT located on the top left corner to display the environment

#. Click ACCESS next to jumphost.f5lab.local

    |accessjh|

#. Select your RDP resolution.

#. The RDP client on your local host establishes a RDP connection to the Jump Host.

#. Login with the following credentials:

    +------------+--------------+
    | User:      | f5lab\user1  |
    +------------+--------------+
    | Password:  | user1        |
    +------------+--------------+

#. After successful logon you can launch the Chrome browser and use the bookmark "bigip1" to launch the WebUI to the BIG-IP.

    |chrome|
    |bookmark|

#. Login in to the BIG-IP with the following credentials:

    +------------+--------------+
    | Username:  | admin        |
    +------------+--------------+
    | Password:  | admin        |
    +------------+--------------+

..Note::  You can take a look around at the BIG-IP.  At this time we do not have any application configuration.

Task 0: Postman
----------------------------
For this lab we are going to use a tool called Postman.  This is a tool that can be used to send API calls.  We use it to craft and submit calls to the REST API of the BIG-IP.  `_Postman Products <https://www.postman.com/products/`_

#. Locate the icon on the Desktop for Postman

    |postman|

#. Double click on the icon and click "Yes" you want to allow this app to make changes.

#. A collection has been loaded along with an environment file:

    +-----------------+-----------------+
    |  |collection|   |  |environment   |
    +-----------------+-----------------+

#. Click on the carrot next to expand the collection and expand **Lab 1 - Imperative Automation with iControl**

    |expand|


Task 1.1: Create
-----------------------------
For this task we will run an API call to our BIG-IP, examine that the call was processed and then run the rest of the commands to create a full configuration for our application app.acme.com.

#. Expand **1.1 Create** and click on **1.1.1 Create - LTM Node**.  Under the POST locate the **Body** and click on it.

    |task1_1|

#. From the Postman tool we are going to send a POST command to the BIG-IP REST API.  In our lab we are leveraging an Environment file to warehouse some variables. Click on the **eye** icon at the top right corner near the **environment file** name. We can see the {{big-ip}} equals the FQDN of our BIG-IP.

    |view_enviro|

    |bigip_var|

    .. Note:: You will also see that we are storing user and password.  These are the credentials used to login to the BIG-IP.  For this lab we are using Basic Auth which allows us to send the username and password with each POST, GET or PATCH.  This may not work in your own environment and we do have other ways to generate token authorization but that is an advanced skill we will cover in a later class.

#. Each POST, GET, or PATCH is sent to the BIG-IP (FQDN or IP Address) and then //mgmt//tm// to identify the namespace for traffic management.  Next we must identify the **organizing collection** which is equivalent to the modules in tmsh.  In our case we are just dealing with the LTM module.  Last we want to direct this POST to Node a collection within the LTM module.

    |post|

#. Now let's examine the JSON body where we plug in our configuration information for the collection we have identified. The body is in JSON format so we can identify the entity we want to configure.  In this case we have directed our call to **Node** and the two must have pieces of information to configure for a **Node** are the **name** and **dddress**.

    |json_node|

#. Return to the Chrome browser and access your BIG-IP.  Navigate to **Local Traffic --> Nodes --> Nodes List**.  We should not have any nodes configured at this time.

#. Go back to your Postman tool. You should still have the **POST** up to create our first **Node** on BIG-IP.  Click on **Send**

    |create_node|

#. At the bottom of the Postman screen you will see the response from the BIG-IP.  We want to see a **200 OK** response to know that the BIG-IP accepted our call and processed our request.  Looking at the return boy we can see that our name and address were transmitted.

    |200_ok_node|

#. Return to Chrome and the BIG-IP.  Navigate back to **Local Traffic --> Nodes --> Nodes List**

    ..Note:: If you are still on the screen click on **Nodes --> Nodes List** one more time to refresh the screen and make the newly created node visible.

    |node1|

#. Return to Postman and locate **1.1.2 Create - LTM Monitor**.  Click on it verify you see **https://{{big-ip}}/mgmt/tm/ltm/monitor/http** as the **POST**.  Check the **Body** to see the configuration and click **Send**. Verify **200 OK**.

    |http_monitor|

#. Repeat for steps **1.1.3 to 1.1.9**

    |repeat|

#. Return to Chrome and the BIG-IP.  Navigate to **Local Traffic --> Virtual Servers --> Virtual Server List**.  You should now see two new Virtual Servers for **app.acme.com** on port 443 and 80.

#. Click on **app.acme.com_vs_80**.  Note that it has an IP Address.  Click on the **Resources** tab at the top. Note that the **_sys_https_redirect** iRule has been attached.

#. Navigate back to **Local Traffic --> Virtual Servers --> Virtual Server List** and click on **app.acme.com_vs_443**

#. Note that we have the same IP Address assigned.  Scroll down and see that we have an **acme_https** HTTP profile attached and a client SSL profile called **app.acme.com_client-ssl**.  Continue scrolling and find that we have Source Address Translation set to **Automap**.

    |vs_app|
    |attach_profile|
    |automap|

#. Click **Resources** at the top.  See a pool has been set and a persistence profile.

    |resources|

#. Navigate to **Local Traffic --> Pools --> Pool List**.  Click on **app.acme.com_pool**.  Notice that the **app.acme.com_monitor** is attached.

    |app_pool|

#. Click on **Members**.  Notice that the node created at the beginning has been added to the pool.

    |member|

Task 1.2: Read
-----------------------------

#. 

Task 1.3: Update
-----------------------------

Task 1.4: Delete
-----------------------------

.. |chrome| image:: ./docs/media/lab01/chrome.png
.. |bookmark| image:: ./docs/media/lab01/bookmark.png
.. |postman| image:: ./docs/media/lab01/postman.png
.. |collection| image:: ./docs/media/lab01/collection.png
.. |environment| image:: ./docs/media/lab01/environment.png
.. |expand| image:: ./docs/media/lab01/expand.png
.. |task1_1| image:: ./docs/media/lab01/task1_1.png
.. |view_enviro| image:: ./docs/media/lab01/view_enviro.png
.. |bigip_var| image:: ./docs/media/lab01/bigip_var.png
.. |post| image:: ./docs/media/lab01/post.png
.. |json_node| image:: ./docs/media/lab01/json_node.png
.. |create_node| image:: ./docs/media/lab01/create_node.png
.. |200_oknode| image:: ./docs/media/lab01/200_ok_node.png
.. |node1| image:: ./docs/media/lab01/node1.png
.. |http_monitor| image:: ./docs/media/lab01/http_monitor.png
.. |repeat| image:: ./docs/media/lab01/repeat.png
.. |vs_app| image:: ./docs/media/lab01/vs_app.png
.. |attach_profile| image:: ./docs/media/lab01/attach_profile.png
.. |automap| image:: ./docs/media/lab01/automap.png
.. |resources| image:: ./docs/media/lab01/resources.png
.. |app_pool| image:: ./docs/media/lab01/app_pool.png
.. |member| image:: ./docs/media/lab01/member.png
