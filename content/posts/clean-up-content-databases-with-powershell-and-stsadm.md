---
title: "Clean Up Content Databases with PowerShell and STSADM"
date: 2011-07-21T14:26:00+0000
draft: true
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

```

```

```
[http://technet.microsoft.com/en-us/library/ff607764.aspx](http://technet.microsoft.com/en-us/library/ff607764.aspx)
```

```
Get-SPContentDatabase -WebApplication http://sitename | Dismount-SPContentDatabase -WhatIf
```

```
[http://technet.microsoft.com/en-us/library/ff628582.aspx](http://technet.microsoft.com/en-us/library/ff628582.aspx)
```

```
Mount-SPContentDatabase "**" -DatabaseServer "**" -WebApplication *http://SiteName*
```

```
*[http://technet.microsoft.com/en-us/library/ff607941.aspx](http://technet.microsoft.com/en-us/library/ff607941.aspx)*
```

```
Test database
```
