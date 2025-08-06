---
title: "Looking at My Email and Found Some Goodies"
date: 2025-08-06T12:24:59Z
draft: false
toc: false
images:
tags:
  - email finds
---

When I began my journey into reverse engineering, I would often sift through my spam folder and open links in a virtual machine. While this wasn't the smartest approach, my desire to learn drove me to explore. I discovered that many scams, particularly phishing attacks, are often simple in design. Contrary to the sophisticated zero-day exploits we hear about in the news or see in Hollywood films, these scams are typically straightforward.

More often than not, they involve recreating a trusted website, such as Google or JageX, with the goal of tricking users into entering their account information. This data is then sent to a platform like Discord for later use, enabling scammers to steal accounts or items to sell on sites like [PlayerAuctions](https://www.playerauctions.com/runescape-items/). Obtaining rare items or high-level accounts can translate to thousands of dollars in real life.

Today, I will delve into this topic further. For legal reasons, I will need to redact some information.

## The Email

The email sent to me looks like it’s from JageX, the creators of OSRS and RuneScape. I used to play OSRS a lot when I was younger, and I was always interested in protecting my account (of course).

The email claims that my account's email has been changed from my address to a random Hotmail account. It instructs me that if I didn’t authorize this change, I should click a link to report and secure my account. If you click the link, you are taken to the following URL: **This is a scam site; do not enter any information into it** `www[.]quantify[.]pe/loginform_aSnv39lf4n3[.]html`. This, along with the email, looks very much like JageX.

At the time of my investigation, the site didn’t have email verification, so you can input random characters into each box and proceed to the next step, which then asks for a 2FA code for the account. This caught my interest because these codes are only valid for a minute, meaning they would need to log into the account in less than that time. You can't store it in a database for later use. I wanted to understand how they were capturing real-time data and using it.

---

## Finding How They Are Storing The Info

When you perform the "a and a" trick and monitor the network activity, you'll notice that the data is sent to a Discord webhook, which includes an embed.

![The post function on quantify](/pictures/func_quantify.png)

### Observations on the Data Transmission

- Odd Behavior: One peculiar aspect is that the webhook sends data to the Discord channel even if no email or password is provided. This raises questions about the effectiveness of the scam.

- Vibe Coded: I would describe this as vibe coded—it's potentially deadly for a victim. While it doesn't work well, it still manages to capture some data. The logic of the flow of execution makes no sense, as there are some safety measures in place, but they are not comprehensive.

### Logic and Execution Flow

- JavaScript Structure: The JavaScript code is contained within a single HTML file, which is a common tactic in phishing sites. This structure can make it easier for attackers to obfuscate their code and hide malicious functions(They didn't do this). Notably, the only fields that seem to work are the email and password inputs.

### Implications for Victims

- Potential Risks: The fact that the webhook captures data even without valid credentials indicates that the attackers may be collecting information for other purposes, such as identifying potential victims or testing the effectiveness of their phishing scheme.

- User Awareness: It's essential to inform readers about the risks associated with such phishing attempts and encourage them to be vigilant when encountering suspicious emails or websites.


---

## Why This Attack Makes No Sense

If someone changed your email, how are you going to log in with the old email? The goal, like with all attacks, is to get someone worried or scared enough that they don't ask that question. I am 100% sure, like all other attacks, it works on some victims. It almost worked on me, even with my email and password.

That really is the key to keeping yourself safe from things like this: ask yourself questions. For example, if they changed my email, how can I log in with my old email?

Always remember to think critically and question the legitimacy of unexpected communications. This mindset can help protect you from falling victim to phishing attacks.