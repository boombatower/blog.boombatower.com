---
created: 1185074580
layout: post
redirect_from:
- article/8/
- node/8/
- microsoft-iis-5-windows-2000-server-asp/
title: Microsoft IIS 5 - Windows 2000 Server - ASP .NET
---
Although I dislike IIS I and .NET I was forced to setup a server running the two due to an application that required them.

I installed Windows 2000 Server, Service Pack 4, various components, and locking down the directory permissions (<http://support.microsoft.com/kb/271071>). Running the application resulting in the famous Metabase issue.

After messing around for a couple of days with no luck I reinstalled Windows 2000 Server and the various things mentioned above. This resulted in the same problem. I reviewed the system event log and noticed that it was failing to load the profile for the SYSTEM user. Just for kicks I set the directory permission on Documents and Settings to allow EVERYONE access. This fixed the problem but created another. This time it was having trouble "mapping" the App_GlobalResources folder.

I searched the web for this issue and found the following article: <http://www.thejoyofcode.com/Failed_to_map_the_path_App_GlobalResources.aspx>. The fix was to changed the permissions on `C:\Documents and Settings\All Users\Application Data\Microsoft\Crypto\RSA\MachineKeys` to allow the ASPNET user to have read and modify permissions.

IIS and .NET were finally satisfied and I just had to setup SQL Server and the remaining application settings.

I rebooted the machine and wouldn't you figure I received the Metabase issue again. Frustrated I just set the Documents and Settings permission to EVERYONE read access. Shockingly enough that fixed the problem.

It is interesting that Microsoft puts such necessary files in Documents and Settings since that is used for user data not libraries etc.

Another one of the wonderful experiences you get to enjoy when using Microsoft products.

