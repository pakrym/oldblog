---
layout: post
title: "How to enable SQL Authentication without using Sql Server Managment Studio"
date: 2013-01-07 13:26
comments: true
categories: sqlserver
---

To enable SQL Authentication navigate to registry key `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL.1\MSSQLServer` and set value of `LoginMode` key to `2` or save the folowing code to `.reg` file and double-click it.

{% codeblock lang:ini %}
Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL.1\MSSQLServer]
"LoginMode"=dword:00000002
{% endcodeblock %}