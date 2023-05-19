---
topic: bugs
author: trap
---

Originally I stumbled across this race condition by double clicking the submit button while turning in a flag.  ***This way may not always give an extra flag but it does work.***

<p>&nbsp;</p>
<h1><img src="https://sls-ci-bowtie-houndstooth-root-us-east-1-assets.s3.amazonaws.com/Trapnatized/blog1/1684453836654-duplicate_flag_cropped.png" alt="" width="890" height="821" /></h1>
<p>&nbsp;</p>
<p>&nbsp;</p>
I needed a way to make this more consistant so I thought lets test for a race condition using Burp Suite.



<p>&nbsp;</p>
<p>&nbsp;</p>
<h1><img src="https://sls-ci-bowtie-houndstooth-root-us-east-1-assets.s3.amazonaws.com/Trapnatized/blog1/1684454759972-dupe_flag_cropped2match.png" alt="" width="891" height="793" /></h1>





