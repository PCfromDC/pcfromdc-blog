---
title: "Office Web App 2013 Server Error"
date: 2018-01-23T23:07:00+0000
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

I ran into an issue the other day, after deploying a new WAC Farm, where SharePoint stopped displaying documents in the wopi frame.

I ran the same New-OfficeWebAppsFarm script that I have been running for the installation/upgrades for years, so at first I thought it was the latest CU.

After installing the upgrade, I ran the following script to get the new WAC Server Version to make sure I was on the right version:

However, the version number did not return. From within SharePoint, I received a Server Error message, saying "We're sorry. An error has occurred. We've logged the error for the server administrator" This message was in the wopi frame, but would be seen within the WAC farm by going to https://wac.domain.com/op/servicebusy.htm

[*](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhBeKET3f7iPYSLCVxX61cnHPdox0dsxjS_mBujQ1h1_r7QWJbyVaFg1KvIQc_ea0BziwvYNUvJmZnnLOnhLnpWXNn9N5v9_HVHJXKtZsM-NCcDnR9OyqrARb8vMdb8an8TAyHUn6OjIg8d/s1600/wopi+error.jpg)
Now the interesting part was that I was able to get content back from going to wopi-discovery: (http://wac.domain.com/hosting/discovery).

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEguk-jJ7h4PuXMRj6p26oxjjBsBqrJ6hSq8bJYIYfRmTqyMASSwi89r95PhSo6KNmBO4JGFWBmveXJzZyOf5afXrouURAuF9u_5pfRiGNM5sOg3-5dYK1lCd_b6N8DkeDyvFZk6UXoq621k/s320/wopi-discovery.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEguk-jJ7h4PuXMRj6p26oxjjBsBqrJ6hSq8bJYIYfRmTqyMASSwi89r95PhSo6KNmBO4JGFWBmveXJzZyOf5afXrouURAuF9u_5pfRiGNM5sOg3-5dYK1lCd_b6N8DkeDyvFZk6UXoq621k/s1600/wopi-discovery.jpg)
Looking at the Event Viewer, I was getting Application Error and .NET Runtime errors for the different application Watchdog.exe. At this point, I enabled verbose logging on the server.

I once again tried to open up the servicebusy.htm page, then opened up the ULS logs. I was able to find an Unexpected Error:

ServiceInstanceFinderAdapter.FindAllServiceInstances() threw an exception: System.Reflection.TargetInvocationException: Exception has been thrown by the target of an invocation. ---> System.Collections.Generic.KeyNotFoundException: The given key was not present in the dictionary.     at Microsoft.Office.Web.Apps.Environment.WacServer.AFarmTopology.GetMachine(String machineName)     at Microsoft.Office.Web.Apps.Environment.WacServer.WSServiceInstanceFinderAdapter..ctor()     --- End of inner exception stack trace ---     at System.RuntimeTypeHandle.CreateInstance(RuntimeType type, Boolean publicOnly, Boolean noCheck, Boolean& canBeCached, RuntimeMethodHandleInternal& ctor, Boolean& bNeedSecurityCheck)     at System.RuntimeType.CreateInstanceSlow(Boolean publicOnly, Boolean skipCheckThis, Boolean fillCache, StackCrawlMark& stackMark)     at System.RuntimeType.CreateInstanceDefaultCtor(Boolean publicOnly, Boolean skipCheckThis, Boolean fillCache, StackCrawlMark& stackMark)     at System.Activator.CreateInstance(Type type, Boolean nonPublic)  at Microsoft.Office.Web.Common.EnvironmentAdapters.HostEnvironment.LoadAdapterInstance(AdapterLoadInformation adapterInfo, Boolean readAppConfigOnly)     at Microsoft.Office.Web.Common.EnvironmentAdapters.HostEnvironment.AdapterLoadInformation`1.<>c__DisplayClass17.b__16()     at System.Lazy`1.CreateValue()     at System.Lazy`1.LazyInitValue()     at Microsoft.Office.Web.Common.EnvironmentAdapters.HostEnvironment.get_ServiceInstanceFinderAdapter()     at Microsoft.Office.Web.Common.ServiceInstanceFinder.RefreshList(Object state)  *

*ServiceInstanceFinder has no data because of an adapter exception, throwing exception to terminate process*

*
*
I did a quick search and found a blog post on OOS which had the same issue ([https://social.technet.microsoft.com/Forums/office/en-US/96dd1ac1-1173-47c3-bdc5-29ac0fd9f722/oos-2016-server-error-were-sorry-an-error-has-occurred-weve-logged-the-error-for-the-server?forum=OfficeOnlineServer](https://social.technet.microsoft.com/Forums/office/en-US/96dd1ac1-1173-47c3-bdc5-29ac0fd9f722/oos-2016-server-error-were-sorry-an-error-has-occurred-weve-logged-the-error-for-the-server?forum=OfficeOnlineServer)). Basically, if the server name is not in all capitol letters, it doesn't work. To fix this, I ran a a script to update 3 registry values to update the Computer Name to all CAPS.

I opened up the servicebusy.htm page again and the expected error was returned:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiZpKLWlzGgcmI6KhStg_MAzsFFOsPoh-TpBH5LZxyvN66Achvx433_e00tt3faaB6JJSgXrxUIysRlCGPlOId9Y5CeASWbb1ZfprG4oQB6HtPbP7t1LkbI-TeB_ZTF7tkQB0QF0QdejB0S/s320/wopi-busy.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiZpKLWlzGgcmI6KhStg_MAzsFFOsPoh-TpBH5LZxyvN66Achvx433_e00tt3faaB6JJSgXrxUIysRlCGPlOId9Y5CeASWbb1ZfprG4oQB6HtPbP7t1LkbI-TeB_ZTF7tkQB0QF0QdejB0S/s1600/wopi-busy.jpg)
I then stop my verbose logging by running the following:

Now, when I run the following script:

I get the expected results:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjZbJPpwbtJBCWZtZ_UTjGPENOgoP80Ux7adPIF9ljKCsVmYeXYIwgaBhee0YmNDs-loNnSFCrhxodY2Yon-ZN8vqn4Q6RsSI3URm5Br9BhKxCfjfxNtnntJgG1vJoASJieX4WBp5I-4c5_/s640/wac-version.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjZbJPpwbtJBCWZtZ_UTjGPENOgoP80Ux7adPIF9ljKCsVmYeXYIwgaBhee0YmNDs-loNnSFCrhxodY2Yon-ZN8vqn4Q6RsSI3URm5Br9BhKxCfjfxNtnntJgG1vJoASJieX4WBp5I-4c5_/s1600/wac-version.jpg)

At least it is an easy fix...

I also tested this on a base image of WAC plus Service Pack 1, and same issue persisted.
