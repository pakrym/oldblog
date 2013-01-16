---
layout: post
title: "Do not resume bookmark from an activity"
date: 2013-01-16 08:38
comments: true
categories: wf
---

In one of my applications that uses Workflow Foundation we had a `ResumeBookmark` call that always resulted in `TimeoutException`. The cause was that Bookmark resumed immediately after it was created, in the `Excecute` method of activity. This resulted in `WorkflowApplication` waiting for workflow to become idle but it couln't because the activity never ended executing. The problem was solved by moving ResumeBookmark out of activity.