---
title: "Microsoft Azure- Getting Started"
date: 2014-12-31T21:33:00+0000
categories: ["Azure"]
tags: ["Infrastructure", "Getting Started", "Introduction"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

This is part 1 of the blog series on getting Started with Microsoft Azure.

Part 1: Microsoft Azure- Getting Started (this post)

Part 2: [PowerShell for Microsoft Azure- Getting Started ](http://pcfromdc.blogspot.com/2015/01/powershell-for-microsoft-azure-getting.html) 

Part 3: [Microsoft Azure- Create Geo Redundancy and Virtual Networks](http://pcfromdc.blogspot.com/2015/01/microsoft-azure-create-geo-redundancy.html)

Part 4: [PowerShell for Microsoft Azure- Creating Storage](http://pcfromdc.blogspot.com/2015/01/powershell-for-microsoft-azure-creating.html) 

Part 5: PowerShell for Microsoft Azure- Upload VHD Images (In Progress)

Part 6: PowerShell for Microsoft Azure- Create Machines (Coming soon)

Part 7: Active Directory and DNS in the Cloud and Azure AD (Coming Soon)

----------
Microsoft's cloud offering Azure, pronounced [ˈaZHər](http://www.oxforddictionaries.com/us/definition/american_english/azure?searchDictCode=all), is a viable competitor to Amazon's Amazon Web Services (AWS). Azure's offerings and differences change way to quickly to actually keep this blog up to date. You can find out more about Azure [here](http://azure.microsoft.com/en-us/overview/what-is-azure/), and AWS' product offerings [here](https://aws.amazon.com/products/). Microsoft also has a nice read on why they think you should use Azure over AWS, [here](http://azure.microsoft.com/en-us/campaigns/azure-vs-aws/).

There are many blog posts on getting started with Azure, but I figure one more cannot hurt, and hopefully you will have an easier time working within Azure after learning from my trials and tribulations.

This series of blog posts on Azure will be taken from the perspective of Contoso and Tail Spin Toys merging to form Contoyso, who, in the desire to ease the merge of the two IT networks, wants to move into a Hybrid environment.

This first post will be about setting up your Azure account and getting things set up so that managing your environment will be as easy as possible as your environment(s) grow.

#### 
Let's Get Started

The first thing you need to do have is either an Organizational account or a Microsoft account so that you can log into Azure. Try not to use an email address that is the same for both an Organizational Account and a Microsoft Account (eg: OU= jsmith@contoyso.com and Microsoft= jsmith@contoyso.com). You will need this account to create your subscription, and having an account that is in an OU and MS will make management through PowerShell a bit trickier (we will discuss this in a later blog post). To can create a free subscription within Azure go to [http://azure.microsoft.com](http://azure.microsoft.com/) and click on the Free Trial link.

To sign up for the free trial, Microsoft does need a credit card, but will not bill your card unless you remove the preset spending limit (currently $200). You will also need to agree to the Windows Azure Agreement, Offer Details, and Privacy Statement. Once you click the Agree button, Microsoft will provision your subscription. It could take around 15 minutes to provision Azure subscription, and when your subscription has been created your subscription statement page will load:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhxq7rSEdUCa1FC0paBHfFm-PcVY_SdQx1LixxBBlKuUivYQx99l86M9jl9_XF5rF0aU_Wqp6XPXUw5gx_nQeenRNZP6VcS9pygUST8uk7CrP4pD6NE5e7j0guq7ZjJ_LJKJbn5fKalsGtJ/s1600/1-+Subscription.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhxq7rSEdUCa1FC0paBHfFm-PcVY_SdQx1LixxBBlKuUivYQx99l86M9jl9_XF5rF0aU_Wqp6XPXUw5gx_nQeenRNZP6VcS9pygUST8uk7CrP4pD6NE5e7j0guq7ZjJ_LJKJbn5fKalsGtJ/s1600/1-+Subscription.png)

#### 
Change Your Subscription Name

At this point, it is helpful to give your subscription a useful name. The reason being, that if you are handling more than one subscription with the same credential, then there is a good chance that you will have subscription names that are the same. For example, Enterprise subscriptions are all called Enterprise by default. To change the subscription name, click on the Edit subscription detail link and give your subscription a more descriptive name.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7Slr5p-GwnWoOMWQECU0JTaJU6GLjHU2CMMxsFP5iAo1VSGtLqRvdhTuDnPrmGQMGgiQbrEPMJTWLm7GUE3-DXJzjF17Z21mhlpVnSv8Tijwd8YAc9hoV2BDgCNUO4j_94Q7OWKUe92io/s1600/2-+Change+subscription+name.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7Slr5p-GwnWoOMWQECU0JTaJU6GLjHU2CMMxsFP5iAo1VSGtLqRvdhTuDnPrmGQMGgiQbrEPMJTWLm7GUE3-DXJzjF17Z21mhlpVnSv8Tijwd8YAc9hoV2BDgCNUO4j_94Q7OWKUe92io/s1600/2-+Change+subscription+name.png)

Notice that we are using a corporation name for our subscription name, but a Microsoft account as the service administrator. At this time Azure has no idea about the Contoyso corporate infrastructure, however, once the Contoyso on-premises AD is tied into Azure AD (later blog post) you can change the Service Administrator to a more appropriate user such as azureAdmin@contoyso.com.

#### 
Welcome to Azure

Once your account has been provisioned, your [https://manage.windowsazure.com](https://manage.windowsazure.com/) site should look like this:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhtGjJP0RWHjQ6pX4fpFtZk5C6wJghCgkERzFbvqbPZ58uVueKiOvDBooKjh0SVJf6Yazf8cSrGzfnck8sazRhB3YL7Qcz0dHPcpK37PQQ62roZc5YfusKBMvae02Np8ffxGfHWoLBHEEWu/s1600/5-+New+Tenant.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhtGjJP0RWHjQ6pX4fpFtZk5C6wJghCgkERzFbvqbPZ58uVueKiOvDBooKjh0SVJf6Yazf8cSrGzfnck8sazRhB3YL7Qcz0dHPcpK37PQQ62roZc5YfusKBMvae02Np8ffxGfHWoLBHEEWu/s1600/5-+New+Tenant.png)

By selecting the ALL ITEMS in the left menu, it will show you your Default Directory for your Azure Tenant. This is your Azure Active Directory, and is not to be confused with your domain active directory as they are 2 different directories at this point.

#### 
Validate Subscription Name

At the bottom of the left hand menu is the SETTINGS menu. By selecting the settings menu, you will be able to see all the subscriptions that you have available to the currently logged in account.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi077DMPbzSIAkAeqQ-qF7CB6RFBBVpfTmSq-iyJeJoz_kKse9-ROEZw-sUgZPnMZXisjwuLekgWkAlI9BpNkG_zoDusFNmKjPpGwWMrtbjDvrjbbn93Kr4GGMefWC8h1DDrAvftLON3nn0/s1600/6-+Subscription+Check.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi077DMPbzSIAkAeqQ-qF7CB6RFBBVpfTmSq-iyJeJoz_kKse9-ROEZw-sUgZPnMZXisjwuLekgWkAlI9BpNkG_zoDusFNmKjPpGwWMrtbjDvrjbbn93Kr4GGMefWC8h1DDrAvftLON3nn0/s1600/6-+Subscription+Check.png)

If your subscription name is still not what you are want, click on the current user account in the upper right hand corner and select, View my bill,

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhpxkGxZtEw409qLthl4Z8ZSY76ETjmMsbtn5aPuj65Bx8ii503EmGqR573LK6UOkSfFu5lJowm2SDNqdUbSSZOcAEV1L6J3CJ2qwLTJb4YNa1dQv02K4Lm1IYRYmZbS_maj398EmBp2_uM/s1600/7-+Change+Name+1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhpxkGxZtEw409qLthl4Z8ZSY76ETjmMsbtn5aPuj65Bx8ii503EmGqR573LK6UOkSfFu5lJowm2SDNqdUbSSZOcAEV1L6J3CJ2qwLTJb4YNa1dQv02K4Lm1IYRYmZbS_maj398EmBp2_uM/s1600/7-+Change+Name+1.png)

or you can go to [https://account.windowsazure.com/Subscriptions](https://account.windowsazure.com/Subscriptions). Once you are within Subscriptions page, click on the subscription name you wish to change. In this case, click on the Free Trial subscription.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg7aupBB8knqVnhIicYNHimQdOgNRG06lIJIUeDU7F4dkwQg3nh9PvDKZiKNCrwIlxGhpS84Mes5F_5g2befzGijBp7ZfOLwajxbpNaRU0k8ODmNPEx8pQjBkq-IBr_3rYok5yj_CvG-_vt/s1600/8-+Change+Name+2.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg7aupBB8knqVnhIicYNHimQdOgNRG06lIJIUeDU7F4dkwQg3nh9PvDKZiKNCrwIlxGhpS84Mes5F_5g2befzGijBp7ZfOLwajxbpNaRU0k8ODmNPEx8pQjBkq-IBr_3rYok5yj_CvG-_vt/s1600/8-+Change+Name+2.png)

This will pull up the subscription statement page, the same one from earlier in the blog. Click the Edit subscription details link, and modify your Subscription name, as described earlier. If you take a look at your subscription setting page, the summary title should have changed.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi0pJkGPY9sk72UDmM3I0aiGbzHbN-JedXQgWWsvvGI5iXCGHkWQD1oRwZJpZ39kMCoqmdx2x2j-zPaR2CEMqlg2Uh55K-JF7ijsOEwl21dY7Sa8Dtejit3grjq8ID3rk692ZuEVM5_AXIH/s1600/9-+Change+Name+3.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi0pJkGPY9sk72UDmM3I0aiGbzHbN-JedXQgWWsvvGI5iXCGHkWQD1oRwZJpZ39kMCoqmdx2x2j-zPaR2CEMqlg2Uh55K-JF7ijsOEwl21dY7Sa8Dtejit3grjq8ID3rk692ZuEVM5_AXIH/s1600/9-+Change+Name+3.png)

Once you have validated that the subscription name has change on the billing side, then your subscription name should have changed on the Azure Settings side as well.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiq1FkaZJICb4n_taTDxFv0ZVAsseeBs97hJEh80pTMlFhxJrH7swMcj1O_JHFVjftrNKw8s806NIz22Wwy6K-bVPFlVEnB4LgiGvl8OQKIN5g8mc7lnBA3Cj8QTg0vUZ8R8e984sfuLWaC/s1600/10-+Change+Name+4.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiq1FkaZJICb4n_taTDxFv0ZVAsseeBs97hJEh80pTMlFhxJrH7swMcj1O_JHFVjftrNKw8s806NIz22Wwy6K-bVPFlVEnB4LgiGvl8OQKIN5g8mc7lnBA3Cj8QTg0vUZ8R8e984sfuLWaC/s1600/10-+Change+Name+4.png)

#### 
What's Next

Before creating anything or setting up anything in Azure, the next step is to get the PowerShell module for Azure installed on a local machine so that you can log in to your Azure Tenant and get work done.

Please continue to the next blog post, [PowerShell for Microsoft Azure- Getting Started](http://pcfromdc.blogspot.com/2015/01/powershell-for-microsoft-azure-getting.html) to see how to install the PowerShell module for Azure and review a couple of ways to log in to Azure through PowerShell.
