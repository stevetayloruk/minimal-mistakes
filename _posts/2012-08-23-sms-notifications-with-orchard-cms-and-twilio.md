---
title: "SMS Notifications with Orchard CMS and Twilio"
tags:
  - Orchard CMS
  - SMS
  - Twilio
---

In this post I'm going to run through a quick example of how to set up SMS notifications in Orchard CMS. 

## Install the SMS Messaging Module ##
First of all install the SMS Messaging Module from the <a href="http://bit.ly/Pw1V8E" target="_blank">Orchard CMS Gallery.</a> 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/sms-module.gif" alt="SMS Messaging Module">

## Sign Up for a free Twilio Account ##
Once the module is installed and enabled, go to <a href="http://bit.ly/RrxF4j" target="_blank">Twilio</a> and sign up for a free account. 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/twilio-signup.jpg" alt="Twilio Signup">

When you sign up, Twilio will give you some credit so you can try out the service; with this credit purchase a number. This number will be used to send the SMS's and will appear when the end user receives the message.

## Configure the Settings ##
Go into the SMS Settings area and enter the information found in Twilio. You'll need the following information:

 - Account SID
 - Auth Token
 - From Number

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/sms-settings.jpg" alt="SMS Settings">

When entering the Auth Token, the textbox will appear empty. This is because the Auth Token is treated like a password and is encrypted in the database.

## Set Up a Rule ##
Now we have everything configured we need an Event to happen so we can fire off a message. Under Rules, create an event. 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/sms-rule1.jpg" alt="SMS Rule">

In this example, the event will be when a new blog comment has been created.
 
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/sms-comment.jpg" alt="SMS Comment">

Next we set up the Action. 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/sms-action.jpg" alt="SMS Action">

The Action will fire an SMS to the administrator of the website but again, this could be an arbitrary number or even a collection of numbers using a comma to separate them. Next fill in the message that will be sent. The message can contain up to 160 characters and can contain tokens.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/send-sms.jpg" alt="Send SMS">

Now that the Event and Action are set up all that's left to do is to do is ensure that the default messaging is set to SMS in the general settings and do a test. 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/sms-messaging.jpg" alt="SMS Messaging Settings">

Go onto a blog post and submit a comment. You should shortly receive an SMS with the message you wrote.


<img src="{{ site.url }}{{ site.baseurl }}/assets/images/sms-iphone.jpg" alt="SMS iPhone">