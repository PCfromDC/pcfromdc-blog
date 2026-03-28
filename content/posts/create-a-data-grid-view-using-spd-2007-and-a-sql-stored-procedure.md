---
title: "Create a Data Grid View Using SPD 2007 and a SQL Stored Procedure"
date: 2011-03-02T03:04:00+0000
categories: ["SQL"]
tags: ["Custom Page", "Connections", "SharePoint Designer"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

Let's say that we have a SQL table of Resources (people), and in another SQL table we have their schedules.  How can we view a list of people who are available to work certain dates?  Basically, we want the user to enter a "Start Date" and an "End Date" and retrieve a Data Grid list of all people in the company who are available to work within those dates.

1) Create the sample databases and data.    [Get SQL script here](http://public.bay.livefilestore.com/y1pZ-OHzNTV9IOYIDMDFKdmTEQtZY4N2v1J1gOleaOWIhKcfQhigPhxWcF5nI_JhxaWfl3i4UeKB6gjhd9Pht_lyw/DemoTables.sql?download&psid=1)

2) Create the Stored Procedure

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgT_cwlGPcLEbwLT5zWbQUp9jUt7N4olkI3AI14W6eulRjupkOxeUD-TtX__VYRzBnzfErCwkHpZtWcBbE1flfsFV7qe0Bi5BWccbcBRLwDyRSJxEqf0qu2lt5XiboMH0TxfTZk23kb9nbe/s640/Stored+Procedure.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgT_cwlGPcLEbwLT5zWbQUp9jUt7N4olkI3AI14W6eulRjupkOxeUD-TtX__VYRzBnzfErCwkHpZtWcBbE1flfsFV7qe0Bi5BWccbcBRLwDyRSJxEqf0qu2lt5XiboMH0TxfTZk23kb9nbe/s1600/Stored+Procedure.jpg)[Get Stored Procedure script here](http://hrrqga.bay.livefilestore.com/y1pJEVN3wU4IwDrCu_yWprjENFP8WBHgOQjvASD-nmMOrdayWi1ybzf1wzpclff7y5un5XgJhJOBNQTG5DyfC5Nv_0Ul3SVRhu3/sp_GetAvailableResources.sql?download&psid=1)

3) Next, we will open up SharePoint Designer 2007, and go to the top level of the site.  For example, http://pc2007.local.  We are now going to create a blank page based off the default,master page.http://pc2007.local  > _catalogs > masterpage > default.master.  Right click the default.master and select "New from Master Page".  This will open up a blank web page.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgSAtHdWPQklzztSyTVBMQfSwXKI_r-Nyz0Zv8HM9398BtoYZ17BfmHAmbPaZ7KiuPxuB3vrF3GjCz2JO-KQ2PSIO2bvyVVE_JgVDLijAH-Ifuzfcc9yj9-_KXcycbLwXW5HkqZpR4zz1D2/s400/Select+Default+Master.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgSAtHdWPQklzztSyTVBMQfSwXKI_r-Nyz0Zv8HM9398BtoYZ17BfmHAmbPaZ7KiuPxuB3vrF3GjCz2JO-KQ2PSIO2bvyVVE_JgVDLijAH-Ifuzfcc9yj9-_KXcycbLwXW5HkqZpR4zz1D2/s1600/Select+Default+Master.jpg) [![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg7zN4jDPmvHTdwRKih8-C1EM5RSt29DFWj5X1iFoRV2D9L7XJ8HnLhDEMvps02rGlO5do9BUroHtG2M_vynbQJDVqFc_Y3yVIl4UjWEiAODpRHdCgi8SGxjEwdGkNbi-NU5S5B3xVpjPkh/s320/Select+New+From+Master.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg7zN4jDPmvHTdwRKih8-C1EM5RSt29DFWj5X1iFoRV2D9L7XJ8HnLhDEMvps02rGlO5do9BUroHtG2M_vynbQJDVqFc_Y3yVIl4UjWEiAODpRHdCgi8SGxjEwdGkNbi-NU5S5B3xVpjPkh/s1600/Select+New+From+Master.jpg)4) To incorporate out own ideas, we will need to go to the Common Content Tasks for PlaceHolderMain and select Create Custom Content.[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjO8RLU2xCeRnJM94GWw4rAfnlF7pn-RFplbHTWNl1dcW9i9KrWuuGG77XYFAr0d-ujC7PA8xZA8tmFTTLTHDet7i9bKqswCVTA4akO3z6ngwfHgTByHjQd3fPcAoqQZJRRb9svXlCB8J36/s640/Create+Custom+Content.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjO8RLU2xCeRnJM94GWw4rAfnlF7pn-RFplbHTWNl1dcW9i9KrWuuGG77XYFAr0d-ujC7PA8xZA8tmFTTLTHDet7i9bKqswCVTA4akO3z6ngwfHgTByHjQd3fPcAoqQZJRRb9svXlCB8J36/s1600/Create+Custom+Content.jpg)5) Within the PlaceHolderMain, I am going to add a 4x4 table.  Table > Insert Table 

6) I am going to label my columns and insert 2 Calendar Date Pickers.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjRjYGcqoEQmw_iX4JFV_26jBQBV5pZaGHa1euNdqKoom0lP1zsBjrsHtp5aNFVuTnDIAUsdpdGHxlDhu-_GXmuDWoMiRRkgSzCAsLaK41OuJyAzpRpckGLSgDfyyiOqcDMh6G3bRGMJ1AN/s640/Calanders+Inserted.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjRjYGcqoEQmw_iX4JFV_26jBQBV5pZaGHa1euNdqKoom0lP1zsBjrsHtp5aNFVuTnDIAUsdpdGHxlDhu-_GXmuDWoMiRRkgSzCAsLaK41OuJyAzpRpckGLSgDfyyiOqcDMh6G3bRGMJ1AN/s1600/Calanders+Inserted.jpg)7) We now want to change the ids from "Calendar1" to "indate" and from "Calendar2" to "enddate"

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgvQ0ZV3VsAcArOOOdQn3jgmElEWp9Np6NTU8LqtRfb1_jnVA8owNJPGvfXW6SpTXL3AQdio_pkJMGOPiBJEE9TMB5iSfuJNimifnT8SXufGbeikj7Nb9zk8MqhOBAPR0J3aeOZnjjq5-yZ/s640/change+calendar+names.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgvQ0ZV3VsAcArOOOdQn3jgmElEWp9Np6NTU8LqtRfb1_jnVA8owNJPGvfXW6SpTXL3AQdio_pkJMGOPiBJEE9TMB5iSfuJNimifnT8SXufGbeikj7Nb9zk8MqhOBAPR0J3aeOZnjjq5-yZ/s1600/change+calendar+names.jpg)8) Within the Data Source Library tab, click "Connect to a database..."

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgrlcKa1rhpT4zBrWFhVKfqL63MbTc0mno7uTOMUOd24H4QrEBlt2nbMSszMTR-IV5PXaf5FoLcPFQvYF4nHhHFKgmZa3ucvEXzxq3Nhfx0ciAYj_vOpF0qCIV-nUmC7g3g1yWlctxNXJ0l/s400/Database+Connection.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgrlcKa1rhpT4zBrWFhVKfqL63MbTc0mno7uTOMUOd24H4QrEBlt2nbMSszMTR-IV5PXaf5FoLcPFQvYF4nHhHFKgmZa3ucvEXzxq3Nhfx0ciAYj_vOpF0qCIV-nUmC7g3g1yWlctxNXJ0l/s1600/Database+Connection.jpg)

9) Enter your SQL server information, click Next.  And you will receive a warning about passwords being saved as plain text.

10) Select the database where your tables are stored, and we are going to select the Stored Procedure radial button.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhw5sDcPkn5WneS_sWY1tZxNR4q2jr298ToOBcMNeljZoZWhLIqCFXDF-bv34BZzaj_jtqUKv64rUS6_macIiJPXTRX-SBYv77cvKlv71nQ0HYvgH11wjsGimWtgzIIm5gkR1Ywtg35wyfO/s400/Select+Database.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhw5sDcPkn5WneS_sWY1tZxNR4q2jr298ToOBcMNeljZoZWhLIqCFXDF-bv34BZzaj_jtqUKv64rUS6_macIiJPXTRX-SBYv77cvKlv71nQ0HYvgH11wjsGimWtgzIIm5gkR1Ywtg35wyfO/s1600/Select+Database.jpg)11) Click Finish!  A new window should pop-up to Edit Custom SQL Commands.

12) Under the Select tab, select the Stored Procedure radial button (again) and select the appropriate stored procedure, and click the "OK" button.

13) Select the "General Tab" and give your database connection a useful name... and save it!

14) This is just my preference, but I will select the bottom left cell of my table to insert my source control...

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjn4OIFVvflMzK1GezTReO2169Qj_DElcBrK0ALpcpE14XhNk2UhbkBvk0LlIzXq76Gb9AOnsqjUjprTL-FojcKpeB2QEnU32w1W5NGfKy6HKif4Kk6JziMHQlpQkLSu-DUraX9h5nR3T9E/s400/Source+Control.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjn4OIFVvflMzK1GezTReO2169Qj_DElcBrK0ALpcpE14XhNk2UhbkBvk0LlIzXq76Gb9AOnsqjUjprTL-FojcKpeB2QEnU32w1W5NGfKy6HKif4Kk6JziMHQlpQkLSu-DUraX9h5nR3T9E/s1600/Source+Control.jpg)15) Hover your mouse over the newly created database connection and select "Insert Data Source Control"  A Refresh Data Source Schema will pop-up, and you will want to click the "Ok" button.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEimA8PCL9NFslGj9v3hyxmJTVsbKLJ7-87BtRAlLQ97OqifOpzyft3ZWK4g2Bm-2sZfOp6CZK9cg9pYFWpUGe3f2UUwpfQvADo9xyTnTelYvo5tPDtKWmeMRXE_dZZjAMXXpdZaWLyaI6PK/s400/Insert+Data+Source+Control.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEimA8PCL9NFslGj9v3hyxmJTVsbKLJ7-87BtRAlLQ97OqifOpzyft3ZWK4g2Bm-2sZfOp6CZK9cg9pYFWpUGe3f2UUwpfQvADo9xyTnTelYvo5tPDtKWmeMRXE_dZZjAMXXpdZaWLyaI6PK/s1600/Insert+Data+Source+Control.jpg)16) You will want to open the Common SqlDataSource Tasks and select "Configure Data Source..."

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiCRwHoSP5V5nIx3nocEfpUXE2OU2sEuYIU_vC17eD-QaJ9UaCa6O7oZSGt7VK_43lL5pSyIayy86zc3LH35vEXtS0EnLQDEC4wAO2ER3R6PT9mICeM3JffKM2PGRgtJS_lyPNJSsxOFNPH/s400/Select+Configure+Data+Source.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiCRwHoSP5V5nIx3nocEfpUXE2OU2sEuYIU_vC17eD-QaJ9UaCa6O7oZSGt7VK_43lL5pSyIayy86zc3LH35vEXtS0EnLQDEC4wAO2ER3R6PT9mICeM3JffKM2PGRgtJS_lyPNJSsxOFNPH/s1600/Select+Configure+Data+Source.jpg)17) Click Next as we have already created the connection

18) However, we want to save the connection string locally, so give it a friendly name, and click Next.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjtegQLvipjiBYlKCRj7JZzqs7pnDyMtIwu8F2DBOwFRNl3Ac8oHHkT2OpdxJAZDEhtleDipZYwQkZfTB5on9gK6MHP-z8puaCHCqfY1PABqLlfqYFORF1okLl9gY4EmCzBFFnWD-5sYmLl/s400/Save+Connection+String.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjtegQLvipjiBYlKCRj7JZzqs7pnDyMtIwu8F2DBOwFRNl3Ac8oHHkT2OpdxJAZDEhtleDipZYwQkZfTB5on9gK6MHP-z8puaCHCqfY1PABqLlfqYFORF1okLl9gY4EmCzBFFnWD-5sYmLl/s1600/Save+Connection+String.jpg)19) Verify the "Specify a custom SQL statement of stored procedure" radial button is selected, and click Next.

20)  Under the Select tab, select the Stored Procedure radial button, find your Stored Procedure, and click Next.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgjvRyzAd40gE2RAGI4JRR_0cIy5dtv3hlldG9B1j3-_Ueo2Pej5FoDm5kDGwm5b2EsIRbIvKj8S4NTflmv04q_W-WV-56nhf1vvfgIdxJH7srvaNKXoIUhceJbJ_dkURMCcNRDDP7DosJ1/s400/Select+Stored+Procedure.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgjvRyzAd40gE2RAGI4JRR_0cIy5dtv3hlldG9B1j3-_Ueo2Pej5FoDm5kDGwm5b2EsIRbIvKj8S4NTflmv04q_W-WV-56nhf1vvfgIdxJH7srvaNKXoIUhceJbJ_dkURMCcNRDDP7DosJ1/s1600/Select+Stored+Procedure.jpg)21)  We now want to tell the connection what variables we want to pass to the Stored Procedure.  Our sources are both "Control" types.  However for the "enddate", make sure that your ControlID is the name of the control that has the End Date...  We had named the calendar "enddate".

22)  Set up the In Date control and press the Next button, and then the Finish button.  You will get a warning about where the configuration for the connection is being saved.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjFS-M6liGcE09Xk756jQEsw5svcxgV6ufYKjL5nopvSH2PmZ6ce8zkNeAZ6OwoP5xqZSjnySHpIBfUkVEY2nvfabzoL1U7sO9bFlG7ZE0moTlb_xIIN8_IVYj7lN5LnRCq11jpEppK8vZC/s640/Controls+Selected.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjFS-M6liGcE09Xk756jQEsw5svcxgV6ufYKjL5nopvSH2PmZ6ce8zkNeAZ6OwoP5xqZSjnySHpIBfUkVEY2nvfabzoL1U7sO9bFlG7ZE0moTlb_xIIN8_IVYj7lN5LnRCq11jpEppK8vZC/s1600/Controls+Selected.jpg)23)  Merge the rows under the calendar.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgHA-dbccSZ6mKhDkPk2JJOP6x_ft_Alcl2ERgRh9HKpqxeZujy2bNBLPDP2tDfz_69iMaRzXe6Fl82XHpI_eed20NmIYIpG9in0nFRzFe-dWCa6Lbh9L31ENzJOa4Ia3IxUy_Q7Wpujz93/s640/Merge+Cells.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgHA-dbccSZ6mKhDkPk2JJOP6x_ft_Alcl2ERgRh9HKpqxeZujy2bNBLPDP2tDfz_69iMaRzXe6Fl82XHpI_eed20NmIYIpG9in0nFRzFe-dWCa6Lbh9L31ENzJOa4Ia3IxUy_Q7Wpujz93/s1600/Merge+Cells.jpg)24) Under the Toolbox tab, we are going to expand the Data Section, grab the GridView control and drop it into the row under the calendar...  The one where we just merged the cells...

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjH7zMoUzGTjbgPGcK1kh5MLABFCrsXM9_GuGv9Asugzm-FljbXqiz1E2ZcB9O9Wkj9bh9c0n8DLF0Knq3jGExDUMVxrbmFagkq2mH44NSfdYS7UDrBAxL4Ea27bG28CaEKHrK1YPP8NzN3/s400/Datagrid+View.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjH7zMoUzGTjbgPGcK1kh5MLABFCrsXM9_GuGv9Asugzm-FljbXqiz1E2ZcB9O9Wkj9bh9c0n8DLF0Knq3jGExDUMVxrbmFagkq2mH44NSfdYS7UDrBAxL4Ea27bG28CaEKHrK1YPP8NzN3/s1600/Datagrid+View.jpg)25) Open up the Common GridView Tasks and under the Choose Data Source, select your Data Source.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgsK45AsABqho4aF5swzS6Mh4LVT4q7CKwi1VNSl4ZtG5g6RK0xeLegFFT18qg-UAh3qRWOMbUj3PfHa4QZs80Gmg8v3qRyiNPi7hcU__1hDsQwLpDHwH7Cyt30EW-jSgTNiOHfpf89o38A/s400/Common+GridView+Tasks.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgsK45AsABqho4aF5swzS6Mh4LVT4q7CKwi1VNSl4ZtG5g6RK0xeLegFFT18qg-UAh3qRWOMbUj3PfHa4QZs80Gmg8v3qRyiNPi7hcU__1hDsQwLpDHwH7Cyt30EW-jSgTNiOHfpf89o38A/s1600/Common+GridView+Tasks.jpg)26)  Under the Common GridView Tasks, enable Paging and Enable Sorting.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiYMUNK9mTZD2SsfEcGm2U-CAlsq7I_CXPWxbwVB_5r5uO220yisHd5wxS95CETDX7lnZpX34TZcsbaBsxUS2ONnRV3ZmHjJdtPVw7XGTu6qTIRc6IiUDuAFiPtKV2ru4PoF7js6VC2cD8R/s400/Enable+Sorting+and+Paging.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiYMUNK9mTZD2SsfEcGm2U-CAlsq7I_CXPWxbwVB_5r5uO220yisHd5wxS95CETDX7lnZpX34TZcsbaBsxUS2ONnRV3ZmHjJdtPVw7XGTu6qTIRc6IiUDuAFiPtKV2ru4PoF7js6VC2cD8R/s1600/Enable+Sorting+and+Paging.jpg)27)  Save your work, check in your document, navigate to your new page, select an In and Out Date, and look at who is available!

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg3NM7BCKeP5aTBI-5qp97W7X-dzeEfaAQxFKex4nePbW3E-HvKEES-_okFZLyvqqmIvnfQHICk4KEgXLRfwGlVdIVwLUZFKwOpc5k16iu4X15LMHD8_EyhzPAkR09tDOVUppsckhxiZufB/s640/My+Finished+Product.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg3NM7BCKeP5aTBI-5qp97W7X-dzeEfaAQxFKex4nePbW3E-HvKEES-_okFZLyvqqmIvnfQHICk4KEgXLRfwGlVdIVwLUZFKwOpc5k16iu4X15LMHD8_EyhzPAkR09tDOVUppsckhxiZufB/s1600/My+Finished+Product.jpg)
