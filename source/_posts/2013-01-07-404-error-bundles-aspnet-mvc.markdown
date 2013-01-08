---
layout: post
title: "Bundles 404 error when running ASP.NET MVC application on IIS"
date: 2013-01-07 20:43
comments: true
categories: aspnet-mvc
---

If you are getting 404 error on some mvc script bundle links when other are working fine check if they have dot symbol in bundle name. Strangely enough bundles with dot are not serverd by IIS but in development server everything is ok. 