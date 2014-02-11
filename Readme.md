#Siri Proxy for SAP Netweaver Gateway
=========
After hearing about folks hacking Apple’s Siri to do things like starting their Viper and controlling their thermostat, I thought about utilizing voice based integration with SAP to retrieve data. By default, Apples technology is pretty well locked down, but it didn’t take long for a developer to introduce a small workaround using a ruby app and a DNS filter. The filter intercepts the call to apple and allows you to inject your own questions, responses and data. If you are interested in learning more, this summary can guide you through the process of setting up and actually communicating with SAP from Siri.

Version
----

1.0

Install Requirements
----

What you will need:
-          iPhone 4S – (SiriProxy has a test console, if you don’t have one)
-          Ubuntu (Virtual Box Virtual Machine) – this is quick and easy to download, and get up and running quickly with Oracle’s VirtualBox. (Download)
-          SiriProxy – You can use this guide from within your Ubuntu box to get this up and running in less than a hour. (Download & Install Instructions)
-          SiriSAP Plugin – You can download this from Github


How does SiriProxy work?
----

SiriProxy intercepts the iPhones DNS calls to Apple (specifically guzzoni.apple.com) and reviews your request, in the event a loaded plugin contains the requested text, it will process the request “locally” rather than sending it to Apple.

Below is the general network path a Siri requests traverses.

![Siri proxy Integration](http://scn.sap.com/servlet/JiveServlet/downloadImage/38-61257-76112/640-242/Capture2.jpg)

With SiriProxy and SiriSAP Plugin, the calls are routed through your Ubuntu server and essentially "preprocessed":

![Siri proxy Integration](http://scn.sap.com/servlet/JiveServlet/downloadImage/38-61257-76113/640-390/Capture.jpg)

How does SiriSAP Plugin work?
----

The plugin gives you  the ability to write your own search text and pulls data from SAP's  Gateway system. As an example, you could say something like: "Show  account name", the plugin will process the request, pull data from SAP CRM (Using Gateway) and present the results to the user.

The plugin is written in Ruby as a gem and utilizes a HTML/XML Parser called NokoGiri which makes processing very quick and easy. One limitation I found with SiriProxy is the inability to have your own custom response types, for example, SiriProxy only supports "Answer snippets", maps, and answers. Below is an example of how a simple interaction would occur:

![Siri proxy Integration](http://scn.sap.com/servlet/JiveServlet/downloadImage/38-61257-76114/ScreenShot2012-01-16at11.27.51AM.png)

In this example we:

1. Intercept the call
2. Let the user know we are processing their request
3. Remove any spaces
4. Pass the account number to a method (show_account_name).
5. The method Opens the SAP Gateway CRM request "Account Collection"
6. We use NokoGiri to parse the HTML/XML
7. We search the HTML/XML for "orginizationname" and return the contents
  
![Siri proxy Integration](http://scn.sap.com/servlet/JiveServlet/downloadImage/38-61257-76116/photo2.jpg) ![Siri proxy Integration](http://scn.sap.com/servlet/JiveServlet/downloadImage/38-61257-76117/photo1.jpg)

[Click here for a short demo video on youtube.](http://www.youtube.com/watch?v=_-ovLb8DgaM&rel=0)