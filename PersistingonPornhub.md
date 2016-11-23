# Persisting on Pornhub
This guy again? More [Pornhub](https://hackerone.com/pornhub) bounty things? 
Well yeah, sort of. Recently I found a stored cross-site scripting vulnerability in Pornhub core(i.e pornhub.com). Allowing any user to post malicious links to their 'steam' aka profile. This post will explain how the attack was possible and show some examples of execution of said attack.

Difficulty: **Low** 
Risk: **High**
Affected URLs: **pornhub.com** 
Report Link: https://hackerone.com/reports/138075
Date Reported: **May 11th, 2016**
Date Report Made Public: **September 24th, 2016**
Bounty Paid: **$1500**

This issue has been marked as high risk in this scenario based upon the affected demographic and the impact of the payloads.


####Timeline of Events


- Reported on Hackerone to Pornhub: 11th May 2016
- Issue Marked as Duplicate: 12th May 2016
- Issue Reopened as closed in error: 20th July 2016
- Issue Resolved: 17th August 2016
- Pornhub Award $1500 Bounty: 12th September 2016
- Issue Disclosed: 24th September 2016

I identified that it was possible to craft malicious [BBCode](https://www.phpbb.com/community/faq.php?mode=bbcode) URLs on the stream function of pornhub, resulting in persistent/stored cross-site scripting occurring. Before this issue was patched it was possible for a user to post a link similar to that shown below which has javascript embedded in the URL.

`[url=http://www.pornhub.com/"onmouseover="alert(document.domain)" ]
target="_blank">http://[url=http://www.pornhub.com/"/onmouseover="alert(document.domain)"/]http://a/"[/url]`

Essentially this would present a link on the user's profile that when moused over would produce an alert box similar to that displayed below:


As can be seen clearly in the screenshot, the link is valid in the user's stream and the alert box is also validly displayed. The following screen captures will demonstrate how this issue was produced.
- Step 1: Navigate to the user profile stream page and select "post to your stream"
- Step 2: Select "Post"
- Step 3: Inject payload string similar to that demonstrated above.
- Step 4: Click post/submit, the screenshot below shows a snippet of the POST request that is made, notice the payload is included twice.
- Step 5 & 6: The payload URL can be seen lying dormant until an unsuspecting user mouses over the link and the payload pops!


Pornhub have since patched this issue and as a result, this is no longer a working method of exploit delivery. An alert box was used to demonstrate that the attack was possible however, more weaponized payloads could have been used such as a [BEeF hook](https://forums.kali.org/showthread.php?23861-Tutorial-Easy-Beef-XSS-hook).

Thanks to Pornhub for the bounty and for fixing the issue. Everyone is that little bit safer now!
