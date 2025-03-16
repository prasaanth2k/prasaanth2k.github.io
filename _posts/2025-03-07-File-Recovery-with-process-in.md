---
title: File Recovery with process in proc
author: prasaanth
date: 2025-03-07 11:33:00 +0800
tags: [linux]
categories: [Linux, Filesystem]
---


# Recovering a Deleted Binary Using `/proc`
<br>
#### Lets dig the Power of `/proc`
<br>
Question : Even after deleting a running binary on Linux, it can still be recovered! ?
<br>

<br>
Yes today we are going to see about that only when ever we run a process in linux all come in the folder called `/proc` <br>
basically in linx "Every thing is an file" what ever it is what ever hardware even process when ever we create an process mean run and process
the linux will create an folder with respective `pid` of that process and in that folder we can able to see all the things about that file and what is
what of that process by using this feature we can able to recover the binary file <br>
<br>

## Example proc folder strcture of htop
<br>
<img class="source_images" src="/assets/img/htopprocfolder.png" alt="Screenshot of the htop process folder in Linux terminal">

<br>

So here you can see ther is file called exe so that is our executable of that htop binary when cat that file and pipe <br>
into an file 
<br>
<br>
```
cat /proc/<pid>/exe > recoveryfile 
```
<br>
we can able get the file and it work this is how the linx system process and files even file discriptor and other things
<br>
<br>
### Here is the practical video
<br>
<br>

<iframe src="https://www.linkedin.com/embed/feed/update/urn:li:ugcPost:7244366010898640896?compact=1" height="600" width="800" frameborder="0" allowfullscreen="" title="Embedded post" alt="Screenshot of the htop process folder in Linux terminal"></iframe>