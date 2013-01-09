---
layout: post
title: "How to get external IP address"
date: 2013-01-09 9:23
comments: true
categories: csharp
---

For this we will use <http://checkip.dyndns.org> service. 

{% codeblock lang:csharp %}
                        WebClient req = new WebClient();
                        var addr = req.DownloadString("http://checkip.dyndns.org");
                        ip = Regex.Match(addr, @"\b(?:[0-9]{1,3}\.){3}[0-9]{1,3}\b").Value;
{% endcodeblock %}
