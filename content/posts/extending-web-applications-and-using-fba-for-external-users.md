---
title: "Extending Web Applications and Using FBA for External Users"
date: 2011-06-25T03:38:00+0000
draft: true
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

You need to create a Security Token Service service application and start the  Security Token Service on the server. This should be done while you're running  PSConfig, but its good to verify before starting...  check the app in central admin and try to surf to: http://localhost:32843/SecurityTokenServiceApplication/securitytoken.svc**

1) create internal web application with claims and windows auth

2) create internal site collection

3) extend web application

if did not use claims, poweshell Primary site...

[http://technet.microsoft.com/en-us/library/gg251985.aspx](http://technet.microsoft.com/en-us/library/gg251985.aspx)

$App = get-spwebapplication “http://sp1:8000”$app.useclaimsauthNentication = “True”$app.Update()update web configs

    a) disable windows auth

    b) enable FBA

    c) add provider (SqlMember)

    d) add manager (SqlRole)

    e) select zone (internet)

4) surf to each internal and external site

http://msdn.microsoft.com/en-US/library/ms229862(v=VS.80).aspx

Set up SQL on server

C:\Windows\Microsoft.NET\Framework64\v2.0.50727

default db name is aspnetdb

for local creation: aspnet_regsql -S (local) -E -A m -d ****
for remote server: aspnet_regsql -S  -E -A m -d ****

****
aspnet_regsql -S sql1-02 -E -A m -d ****FBA-Auth****

****
Show .UDL file

http://www.sharepointchick.com/archive/2010/05/07/configuring-claims-and-forms-based-authentication-for-use-with-a.aspx

sts web config located at:

****[http://blog.summitcloud.com/2009/11/forms-based-authentication-sharepoint-2010-fb/](http://blog.summitcloud.com/2009/11/forms-based-authentication-sharepoint-2010-fb/)**

http://blogs.technet.com/b/mahesm/archive/2010/04/07/configure-forms-based-authentication-fba-with-sharepoint-2010.aspx

[http://www.devcow.com/blogs/jdattis/archive/2008/03/10/forms-based-authentication-application-pool-account-permissions.aspx](http://www.devcow.com/blogs/jdattis/archive/2008/03/10/forms-based-authentication-application-pool-account-permissions.aspx)

troubleshooting  STS add  before 

The requested service, 'http://localhost:32843/SecurityTokenServiceApplication/securitytoken.svc' could not be activated

goto [http://support.microsoft.com/kb/2520344](http://support.microsoft.com/kb/2520344)
