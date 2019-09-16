---
title: "How This Site Was Built"
date: "2019-09-16T04:00:00+01:00"
draft: false
---

(This post may change over time if the stack around this site changes.)

I thought I'd share how I built this site because it was pretty painless. The short of it is that I just followed what [Julia Evans](https://twitter.com/b0rk) said to do in her blog post on [How to put an HTML page on the internet](https://jvns.ca/blog/2019/09/06/how-to-put-an-html-page-on-the-internet/).

Julia does a really great job of explaining a lot of things and in this post she really boils it all down to a nice easy How to on how to get a site up and running without having to use a blogging site like Medium.

I followed everything pretty much to T. So to sum up what is running the site:

 - New [GitHub](https://github.com) repo
 - Created a site using [Hugo](https://gohugo.io/) 
 - Committed Hugo site to GitHub repo
 - Created a [Netifly](https://www.netlify.com/) account
 - Netifly is configured to fetch content from my GitHub repo
 - I have a build command setup in Netifly to build the site
 - I created a domain through [Google domains](https://domains.google.com) (this part is not free).
 - I setup the DNS forwarding for my google domain to point to the netfily site.

I do have a few things to add on all of this:

1. Do not use your main gmail email (if you use gmail) to register your domain if you go through Google.
   1. If one gets suspended for whatever reason or even hacked, you could lose it all. It's always good to have backups.
   2. Also good to have your whois info not point to your main personal email, IMO.
2. Keep your domain registration and site hosting seperate.
   1. Sort of same reasoning as above. Basically the idiom of "don't keep all your eggs in one basket". Basically, diversify the pieces of your site (domain name, site hosting, code repo) so that if one or even two things go down, you still have pieces of the whole to build back up around.
3. For getting the DNS setting setup in Google domains, I ended up using a CNAME. Netifly says to use an ANAME, but Google does not have that so the CNAME worked fine. Not sure if I needed both the A and CNAME types setup, but is what I ended up doing:
![CNAME settings.](/images/GoogleDNS.png)
4. The build command I ended up using in the Netifly Continuous Deployment settings is "hugo --gc --minify".

And that's it. Pretty simple stuff. I did find that there was a learning curve for me getting the DNS settings setup right and also with getting the Hugo theme to work nicely, but nothing that was too bad.
