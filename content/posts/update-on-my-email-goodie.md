---
title: "Update on My Email Goodie"
date: 2025-08-07T12:27:48Z
draft: false
toc: false
images:
tags:
  - email finds
---

So after posting my last post. I got another email from "JageX". This time it was about account migration, and it was being sent to a random Gmail. The email itself is from support[at]goamiles[dot]com which is like trivago[.]com site based in Panjim-Goa(aka India).

The link to the "signin" page first when to a linkly[.]link (I reported and got taken down) shorten url. That when takes us to a new url **This is a scamsite don't enter real info** no-limitsolutions[.]com. This one is interesting because it takes your ip, city, ISP, country, and user agent. Seen in screenshot:

![Vistor_ip_reporting_on_load](/pictures/Vistitorip_on_load.png)

---

The other site appears to be taken down sadly. The "on load function" that reports IP is reported to a different channel then the login details, but the login also reports IP now.

![the_new_on_send_func](/pictures/updated_send_func.png)

---

## The Way It's Getting The IP

It's using ipify[.]org pull that info. Has seen in this screenshot

![Func_fetch_ip_and_geo](/pictures/fetechip_and_geo_no-limit.png)


This is a `Cross-Origin Request`, and most browsers like Brave will block that by default. You can also add ipify to your blacklist of URLs because that is the only thing it does is return IP info.

---

## This Is The End Sadly

I didn't really have much to say. I have been reporting the infomantion to the right places, and jumping the hoops to get this taken care of. Other than that it's the same has my pervious post, just the new domain and *vibe coded* attempt to get IPs and geo data. Remember to wear protection, block Cross-Origin Request, and don't open random links. Until next time!