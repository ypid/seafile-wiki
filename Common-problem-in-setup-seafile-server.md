## Failed to upload/download file online

* Make sure you firewall for seafile httpserver is opened.
* Using chrome/firefox debug mode to find which link is given when click download button and what's wrong with this link

## When downloading a library, the client hangs at "connecting server"

First, you can check the ccnet.log in client (~/.ccnet/logs/ccnet.log for Linux, C:/users/your_name/ccnet/logs/ccnet.log for Windows) to see what's wrong.

Possible reasons:

* Miss config of  <code>SERVICE_URL</code>: Check whether the value of is set correctly in server's <code>ccnet.conf</code>.
* Firewall: Ensure the firewall is configured properly. See [[Firewall Settings for Seafile Server ]]

Trouble shooting:

* Manually telnet to see if you can connect: <code>telnet your-server-IP-or-domain 10001</code> 
