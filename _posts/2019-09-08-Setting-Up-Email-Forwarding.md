---
layout: post
title: "Setting Up Email Forwarding"
date: 2019-09-08 02:08:22 -0400
categories: [software, tutorial]
tags: [email, gmail, email forwarding, dns, custom domain, comcast sucks, forwardmail]
---
### Third Party Email Forwarders
In looking for a solution to utilize email forwarding, I found that Comcast (and most other internet service providers) block port 22 on resident networks. To receive or send any sort of email on a resident network there are two options: upgrade to business class internet or use a third party service for email.

So, that brings me to finding such a service.

The one I landed on was [ForwardMail][forwardmail]. It's free, open source and has a small following already. Now, there are some pros and cons to using a third party, specifically forwardmail, I'll list a few of them below.

#### Pros
* Relies only on DNS (could be a con, depends on the person)
* Don't have to deal with any networking/hosting
* Privacy, Forwardmail claims they don't log anything

#### Cons
* Privacy, up to you to trust them
* Forwardmail hosts the service, if they go down there's not much to be done

### Setup
If you'd like to read through the documentation yourself, I'll link it [here][forwardmail_setup]. Otherwise, feel free to continue reading.

#### How does it work?
Like normal MX DNS entries, they point to which servers handle the domain's mail. We'll set these to forwardemail's so they can handle the mail for us.

|Name/Host/Alias|TTL|Record Type|Priority|Value/Answer/Destination|
|---|---|---|---|---|
|@ or leave blank|3600|MX|10|`mx1.forwardemail.net`|

<br />

Next, we need to tell forwardemail what addresses to handle and where to forward them to. Forwardmail will lookup the txt entry and configure their service depending on what is entered. This will email all emails to the forwarded address `example_email@gmail.com`.

|Name/Host/Alias|TTL|Record Type|Value/Answer/Destination|
|---|---|---|---|
|@ or leave blank|3600|TXT|`forward-email=example_email@gmail.com`|

<br />

If you'd only like to send emails to say contact@customdomain.com use the setting below.

|Name/Host/Alias|TTL|Record Type|Value/Answer/Destination|
|---|---|---|---|
|@ or leave blank|3600|TXT|`forward-email=contact:example_email@gmail.com`|

<br />

Finally, we need to setup SPF verification or Sender Policy Framework verification. Basically, we use this to verify that the email is origination from our domain and that it's legitimate.

|Name/Host/Alias|TTL|Record Type|Value/Answer/Destination|
|---|---|---|---|
|@ or leave blank|3600|TXT|`v=spf1 a mx include:spf.forwardemail.net -all`|

### Using Gmail
With that, email forwarding is setup and sending emails to `example_email@gmail.com`. Now, if you'd like to respond to the emails from gmail and use the custom domain's email address you can follow this [guide][forwardmail_gmail].

[forwardmail]: https://forwardemail.net/
[forwardmail_setup]: https://forwardemail.net/#/?id=how-it-works
[forwardmail_gmail]:https://forwardemail.net/#/?id=send-mail-as-using-gmail