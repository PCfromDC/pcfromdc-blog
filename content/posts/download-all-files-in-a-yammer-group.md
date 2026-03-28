---
title: "Download All Files in a Yammer Group!"
date: 2016-06-21T01:31:00+0000
tags: ["Download All Files", "Chrome", "File Download", "Yammer", "JavaScript"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

I was lucky enough to take a week long Azure Cloud Solution Architect training class hosted by Microsoft. Unluckily, they uploaded all the documents for the class into Yammer, apporoximately 80 files. Now, I could just go to each file and individually download each file, but where is the fun with that? So, I decided to do a quick search and found a couple of blog posts.**
I found a GitHub post *[Download all files in a Yammer.com group](https://gist.github.com/Saturate/9388053) *and a blog post from Sahil Malik ([https://twitter.com/sahilmalik](https://twitter.com/sahilmalik)) [Download Multiple Files from Yammer - easily](http://blah.winsmarts.com/2014-3-Download_Multiple_Files_from_Yammer_-_easily.aspx).

The first problem that I ran into was that neither code worked any longer due to the url structure change, as Sahil predicted. The second problem was that the code opened up a new tab for each file downloaded. I figured there had to be a better way.

Now, I am fortunate enough to be able to reach out to someone I consider to be one of the best JavaScript developers around, Matthew Bramer ([https://twitter.com/iOnline247](https://twitter.com/iOnline247)) for a bit of help.

In this post, I am going to show you a couple of ways to download your files. First is the full length developer version, while the other is a quick and easily repeatable version.

We decided to test this in Chrome only. The developer version should work in Edge, but was only tested in Chrome. If you want to complain that you cannot get it to work, TRY CHROME FIRST!****

Do This First****
1) Within Chrome, open up Settings**, and select **Show advanced settings...****
2) Under Downloads**, set a download location and make sure that the **Ask where to save each file before downloading** check box is **NOT **selected.**

Full Length Developer Version****
1) Open up Yammer, and go to the files location.

     a) Make sure that you scroll down and click the More** button to show all of the files.**
2) Hit F12** to open the Developer Tools**
3) Under Sources**, select **Snippets**.**
4) Insert the following code into the Script snippet window:

5) Click the run snippet button (Ctrl + Enter) to start your downloads

Easily Repeatable Version****
1) Within Chrome, open the Bookmark Manager** (Ctrl + Shift + O)**
2) Under Folders, select (or create) the appropriate folder

3) Under Organize, click the Organize** drop-down and select **Add page...****
4) Give the Page an appropriate name like, Download All Yammer Files on Page

5) For the URL, paste the following code.

6) Open up Yammer, and go to the files location.

     a) Make sure that you scroll down and click the More** button to show all of the files.

7) Open the bookmark that you just created to start downloading all the files.

Thank you Matthew for helping me get this up and running in time for Ignite...
