---
title: "IIS7 and Multiple IP Addresses That Won't Bind"
date: 2010-10-07T14:10:00+0000
tags: ["IP Binding", "Tomcat", "IIS 7"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

Recently one of my clients asked to put a Tomcat Server on the same box as their SharePoint 2007 WFE. 

1) Add the IP address (Local Area Connections --> Properties --> Advanced --> IP Address Add...)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh_iqgoPuCRr3hZAL1JDyTW0Y74AcYi50J1BtU8jE89TR7lzpQ1gtjtzgVNueN6cPqzB0n-WehwkPsSAhPg6Bcs-KY4u0wdyKqV7M0gJjalS-elsMvE3e14l2y7MRkwCXCmnZrA4MQd-6WS/s320/LAC.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh_iqgoPuCRr3hZAL1JDyTW0Y74AcYi50J1BtU8jE89TR7lzpQ1gtjtzgVNueN6cPqzB0n-WehwkPsSAhPg6Bcs-KY4u0wdyKqV7M0gJjalS-elsMvE3e14l2y7MRkwCXCmnZrA4MQd-6WS/s1600/LAC.jpg)[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi2Q-X_oHBupLuEqj7WLD_JIgfRVASJVfG8TXbSvWxwC-9nJI3i4nAhfXcAfUx3e17FxUES8ibR29XQ9nMKl4NRZNoWVwWm9DDDos1y-zvRhEEoHTQotGWgxiuyVOoma7w4G3h423LlsQJm/s320/LAC+Properties.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi2Q-X_oHBupLuEqj7WLD_JIgfRVASJVfG8TXbSvWxwC-9nJI3i4nAhfXcAfUx3e17FxUES8ibR29XQ9nMKl4NRZNoWVwWm9DDDos1y-zvRhEEoHTQotGWgxiuyVOoma7w4G3h423LlsQJm/s1600/LAC+Properties.jpg)[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj6S3XZ2Lu1-S9HruQhHghEzNdunR5UVkc9nnmsG131hlcSeA0Q8lz4aHuo5pn5BcQefmZ0iHKHkMmh_3m-QGgIZCVUxcAd2hh2yeUTKLJMLegRJOqpt0ijBnxx37beHxJalgdpkmyqcYkh/s320/LAC+Properties+Advanced.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj6S3XZ2Lu1-S9HruQhHghEzNdunR5UVkc9nnmsG131hlcSeA0Q8lz4aHuo5pn5BcQefmZ0iHKHkMmh_3m-QGgIZCVUxcAd2hh2yeUTKLJMLegRJOqpt0ijBnxx37beHxJalgdpkmyqcYkh/s1600/LAC+Properties+Advanced.jpg)[](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi2Q-X_oHBupLuEqj7WLD_JIgfRVASJVfG8TXbSvWxwC-9nJI3i4nAhfXcAfUx3e17FxUES8ibR29XQ9nMKl4NRZNoWVwWm9DDDos1y-zvRhEEoHTQotGWgxiuyVOoma7w4G3h423LlsQJm/s1600/LAC+Properties.jpg)

2) Stop IIS  (cmd --> iisreset -stop)3) Set binding information in Tomcat

4) Set binding information in IIS          (IIS Manager --> Right Click Site --> Edit Bindings --> Edit Port Binding --> Select IP from drop down) 

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhneXx36uQLUpTnlveAwwE5q7lFlowNa-LoASjfuHE1kYUyvzwPz0QaOwO3P3luHywS2LowuVW5E1Ac8uav-xWGtubGCWvv1auBA11zCX_0myNdR-8E52KuFEyqCeIIVnXVtFKGB78OYHAd/s320/IIS.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhneXx36uQLUpTnlveAwwE5q7lFlowNa-LoASjfuHE1kYUyvzwPz0QaOwO3P3luHywS2LowuVW5E1Ac8uav-xWGtubGCWvv1auBA11zCX_0myNdR-8E52KuFEyqCeIIVnXVtFKGB78OYHAd/s1600/IIS.jpg)[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEju9VLDpNJbyxOxc_oTXJJVf20mehcoSCGyxKpfDjL_cWBv8C1M2W7e_40CrJj2BxrCrb_z3hoy7WjuZxFMBKXS4vKIy69Mh2_63vQyqU82bhhFrB35HKQ2YYJObBNjLIojTcdyd8EBch0l/s320/IIS+Binding+Edit.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEju9VLDpNJbyxOxc_oTXJJVf20mehcoSCGyxKpfDjL_cWBv8C1M2W7e_40CrJj2BxrCrb_z3hoy7WjuZxFMBKXS4vKIy69Mh2_63vQyqU82bhhFrB35HKQ2YYJObBNjLIojTcdyd8EBch0l/s1600/IIS+Binding+Edit.jpg)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjfXnkcZsdn7YryqoOB1n5ez75rnUbhru0yFlo9CSRidkFYF8_qGWfCQO3N_QAYt7P78EmoStKUnahkz0NnJOKBM_CheLXXwCoywrjwg0bBkpoklwiEAw8kAsNavzs2JmcHvSosDUl1b6JR/s320/IIS+Binding+Edit+Site.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjfXnkcZsdn7YryqoOB1n5ez75rnUbhru0yFlo9CSRidkFYF8_qGWfCQO3N_QAYt7P78EmoStKUnahkz0NnJOKBM_CheLXXwCoywrjwg0bBkpoklwiEAw8kAsNavzs2JmcHvSosDUl1b6JR/s1600/IIS+Binding+Edit+Site.jpg)

5) By default, IIS7 binds to ALL port 80 IPs, so we have to disable this behavior for the IP in IIS       (cmd --> netsh http add iplisten ipaddress=xxx.xxx.xxx.xxx)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi-8ibtTyMcgwYw2oJV9cYXx82tAkN6DR_W9HBspzrPuPOaUOHpfpZgLPobwRvRAQt2gFV7WvXbwzqJqxoGpmjD2tuacwWx6JqPqVWoDdYjsj7yWWzbZe8YTvN19JUhLv-SptAQFKiFngGs/s320/netsh.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi-8ibtTyMcgwYw2oJV9cYXx82tAkN6DR_W9HBspzrPuPOaUOHpfpZgLPobwRvRAQt2gFV7WvXbwzqJqxoGpmjD2tuacwWx6JqPqVWoDdYjsj7yWWzbZe8YTvN19JUhLv-SptAQFKiFngGs/s1600/netsh.jpg)[](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi-8ibtTyMcgwYw2oJV9cYXx82tAkN6DR_W9HBspzrPuPOaUOHpfpZgLPobwRvRAQt2gFV7WvXbwzqJqxoGpmjD2tuacwWx6JqPqVWoDdYjsj7yWWzbZe8YTvN19JUhLv-SptAQFKiFngGs/s1600/netsh.jpg)

6) Restart IIS (cmd -->  iisreset)

Warning!!!

If you have created Custom Web Services, running the Net Shell command will bind your Custom Web Services to the IP address and cause them to stop working.  This will give you an error of:  Not Able To Connect To Remote Server.
