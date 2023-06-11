---
topic: bugs
author: trap
---

Originally I stumbled across this race condition by double clicking the submit button while turning in a flag.  ***This way may not always give an extra flag but it does work.***

<p>&nbsp;</p>
<h1><img src="https://sls-ci-bowtie-houndstooth-root-us-east-1-assets.s3.amazonaws.com/Trapnatized/blog1/1684453836654-duplicate_flag_cropped.png" alt="" width="890" height="821" /></h1>
<p>&nbsp;</p>
<p>&nbsp;</p>

But we want a way to make this more consistant, so lets exploit this race condition using Burp Suite. First, turn foxy proxy on then we can intercept a flag submission, right click and send to intruder.
<p>&nbsp;</p>

While inside the positions tab make sure the attack type is set to a sniper attack, and also clear the payload positions.


<p>&nbsp;</p>
<h1><img src="/assets/images/flag_submission_intruder_edited_1.png"></h1>
<p>&nbsp;</p>

Next we will head over to the the payload tab and set the payload type to Null payloads. In the payload options section click Generate payloads and enter 5. This will allow us to send the original flag submission request 5 times repeatedly. Now we can finally start attack.

<p>&nbsp;</p>
<h1><img src="/assets/images/race_intruder_payload.png"></h1>
<p>&nbsp;</p>

***Note all five requests are not guaranteed to go through. But the best chance for multiple duplicate flags are when you recieve all reponses with a word length of 721.***

<p>&nbsp;</p>
<h1><img src="/assets/images/721_response_code.png"></h1>
<p>&nbsp;</p>

Exit out of the attack and discard the results. Go back to Intruder's positions tab and change the flag value to the next flag on the list. You can now hit start attack again and rinse and repeat for all the remainder flags.

<p>&nbsp;</p>
<h1><img src="/assets/images/flag_submission_intruder_edited_2.png"></h1>
<p>&nbsp;</p>

<p>&nbsp;</p>
<p>&nbsp;</p>
<h1><img src="https://sls-ci-bowtie-houndstooth-root-us-east-1-assets.s3.amazonaws.com/Trapnatized/blog1/1684454759972-dupe_flag_cropped2match.png" alt="" width="891" height="793" /></h1>





