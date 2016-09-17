# POOFITEE 

Linux Scripting "Packet filtering Owner Only, adaptive, pseudo-application layer linux Firewall - the system is Invisible To Everyone Else"

-- as of 2016-09-16 operational - tested with Ubuntu server CLI, Linux Mint, Raspberry Pi, and Pine A64 (for which you'll have to manually load the ipset module for iptables).  We expect success with many or most other Linux distros.  Future expansion still planned including source code cleanup and screen layout enhancement.  

Linux iptables plus scripting firewall  "NOT COMPATIBLE WITH BSD NOR MAC SYSTEMS"

BULLET-PROOF YOUR “OWNER-ACCESS-ONLY” LINUX SERVER FROM HACKERS WITH IPTABLES AND ROBUST SHELL SCRIPTING.

Bare minimum packages and whitelist-only access to your Linux home surveillance &amp;/or automation system such as NEST or WeMo,  or your WiFi router (if port or other forwarding, or external management are in use).  ENTERPRISES: Let POOFITEE create the blacklists you need! Run POOFITEE on an external ip address that should have no incoming traffic, then share with your firewalls the ipset blacklist it creates.

No hackers can get any data or communication at all out of your system when this firewalling solution is installed on it:

1. Only you know how to get your remote IP address permitted by whitelist to become an authorized source address.  This will be by port knocking or email.  Source addresses not whitelisted are ignored as effectively as them being explicitly blacklisted.  All 65536 of your ports are invisible to them.
2. Only you know the IP address of your system – it's emailed to you fresh every time your systems gets it changed by DHCP.  
3. You will be notified immediately by email/text of any and all source addresses that your system places on its whitelist or removes from its blacklist.  
4. You would still continue to use passwords and ssh keys as is prudent even for systems without firewall protection.

All this PLUS the simplicity and robustness inherent in a bare iptables and scripting-only Linux solution, PLUS the option of building an explicit blacklist of first-time hacking packets in real-time for your reference and curiosity, if your system has enough RAM.

INTENDED AUDIENCE

 -- Owners/admins of Internet-connected systems who want to protect them from hackers and reclaim the bandwidth hackers steal.  Surveillance, automation, and normal Internet access systems are otherwise subject to hacking attacks trying to break in from Russia, China, you name it.  

 -- Non-BSD LINUX server Command Line Interface (CLI) users connected with IPv4 (IPv6 maybe later).  IPv6 access INTO your system will always work fine because the IPv6 address will get converted into IPv4 before arrival. This setup can co-exist with your favorite Linux GUI and will also run on Linux workstation.   NOT COMPATIBLE WITH BSD NOR MAC SYSTEMS

 -- Plus anyone who doesn't even care about the firewalling aspect but just wants the ability to email commands to their Linux-based automation or other purpose server.  Examine the file "email_fetch_parse" to see the default set of keywords and what actions each keyword will perform.  Some examples from that file are: "allow (IP address)" to whitelist a specific IP address, "end blacklist (IP address)" to remove from blacklist a specific IP address, "report" to have the tail of /var/log/kern.log emailed to you, "log" to insert an iptables rule that starts logging every IP packet that makes it down in the iptables INPUT chain ruleset to the rule that sends it to s_privateIPs chain (for if ever the default level of logging didn't log your first attempt), "undo logging" to undo that high level of logging, and a few more.  Make up more for your own use or for automation via usb (echo someword > /dev/[automation's usb channel]).

 -- You, as owner, want Internet access to your system from anywhere without it providing TCP/IP services to John Q. Public.  I assume your system is already be online.  This project only adds firewalling to your system.  If you do intend to provide certain TCP/IP services to Mr. Public, you'll want to modify these scriptings

 -- You've discovered innumerable security-flaw seekers and other type opportunistic thrill-seekers worldwide are hammering YOUR system, stealing YOUR bandwidth, and you take offense to that, never knowing when they might succeed

 -- You may have discovered that Fail2Ban, UFW and the like iptables front-ends are too complicated, not persistent enough, not intuitive, or otherwise too wimpy to be secure firewalls for owner-access-only systems.  This firewalling solution is NOT a front-end at all; you may edit these rules in place without fear that some iptables front-end will trash your edits.  You're allowed to continue direct or programmed control of iptables via CLI or other scripting without fear of interaction consequences.  These are helper scriptings to give you some very useful remote or unattended iptables rules right alongside current rules you may need for other purposes.
 
 -- You may have used knockd for port knocking and found it to be limited in knock port sequence, and horrifyingly it tends to stop running for no apparent reason.

 -- You understand the advantages of having your system TOTALLY invisible to anyone but you, your friends, and allies.  (Visibility MIGHT be required in IPv6, but IPv6 is not yet protected in the current state of POOFITEE development)  

 -- You want to minimize the number of application packages installed in order to maximize your system's robustness.

 -- You don't mind learning iptables and making the initial ruleset unique to your configuration.
 
 -- You are knowledgeable, willing, and able enough to lightly modify one or two of the short scripts presented here to suit your environment.  Modifications to these scripts will be necessary for the system to use your ISP-provided email account to send system-generated email/texts from and to know the email addresses you want to receive those IP address changes and iptables-relaxing changes alerts in.  You should also expect a certain amount of script troubleshooting to be needed during installation, Muphy's Law.  This 'feature' helps me get this published sooner.

 -- You can spare a dedicated email account not requiring access via SSL, for back- or front-door communications to your system via the Internet.  In my case, my local ISP allows non-SSL access to the email accounts they provide their customers.  Perfect for me, because I would have absolutely no use otherwise for an ISP-provided email account, given how often I seem to change or drop ISPs due to oft-changing circumstances.

 -- You understand the advantages of this set up not using or having to configure any sort of polling to check for email messages or IP address changes.  When you send an email to the email account dedicated to communicating to your system, just knock a port afterwards to cause your system to read that email

 -- “Owner-access-only” use is by virtue of heavy-handed “drop” rules affecting all ports equally.  If your interest is to provide some public services, simply modify the scripting presented to use iptables drop rules less strictly on those ports. For example, you could create an INPUT chain rule placed just before the LOG and DROP rules ACCEPTing port 80 and if your “recent” iptables module rules aren't violated.  In other words, the general technique presented here will work for non-owner-access-only system, assuming you are skilled to any extent with iptables rules and with modifying shell scripts. The only points I'm making in highlighting “owner-access-only” are:  1) fail2ban, the prevalent product in use in that niche, is notoriously ineffective in the hands of non-gurus (I.e, “owners of a home-based systems who have better ways to spend a few hundred hours than learning the non-intuitive fail2ban nuances”),  2) the source computers of incoming IP packets are either friend or foe, but most companies offering public web sites are very reluctant to slap a “FOE” label on probers to blacklist them, in large part because IP addresses are often dynamic, and 3) this scripting as provided allows no public services

 -- You understand that a large share of internet IP addresses are dynamically allocated.  That is, any specific IP address may be foe one minute, then friend the next, or vice versa

 -- In this presentation, I deliberately don't explain the technology at a raw novice level so that I could publish this sooner.  In fact, I make you adapt and save the scripts the hard way, although I certainly could have written a helper script for that.  So if you're not able to follow this presentation, you probably need to gain a higher level of understanding than you have right now...like getting someone's help who understands all this or learning it yourself, which will be better on your friendships


Intuitive

You just know that your firewall should only allow friend to pass and only after said friend proves friendship, at least on all IP ports other than those you offer for public use.  And you just know that the firewall rules should be persistent between reboots of your firewall computer.  Other firewall software I've tried should, but doesn't, implement these two concepts by default nor force the installer/system owner to acknowledge otherwise during the installation step, well before production use.  I consider that NON-INTUITIVE.
You just know that you shouldn't have to predict and flawlessly characterize any and all perpetrators' future activity and encode said activity perfectly into config files in order for your firewall to protect you from said activity.  If you've encountered this NON-INTUITIVE behavior of fail2ban, you've realized why its name is sadly appropriate for that package.    


Function and Capabilities of this Firewalling Script Solution as if used in a Home Surveillance System


At startup with a new IP address and at every IP address change thereafter, your system will email you telling you its new IP address and whatever other information you have configured that scripting to tell you.  You can easily modify the script to notify recipients, in case you also want to be additionally texted, for example, or if you have numerous allies who need to know.  The afore-mentioned “dedicated email account” is not necessarily used in this sending step unless the script is simply using it for authentication.  Said another way, to send this email out does require an SMTP email client such as mailutils.  The destination of said outgoing email will be the email account you want to read the inbox of when you get to your hotel room.


From your hotel room in Dyubai, let's just say you find that you have IPv6 Internet access while your home system is on IPv4.  You could choose the port knocking method to get your hotel IP address whitelisted, because you don't know offhand while you sit in Dyubai what is the specific translated IPv4 address that your home system will see you come in from.  In your email inbox, you've received your home's IP address with the helpful “nc -w 1” knock command.  Your system sent that email to you the last time it detected its email changed.  The port knocking method will whitelist your translated IP address if you're careful not to have a browser open too soon throwing extraneous packets at your system on port 80 or 443.  Will lots of other IPv6 address get translated into this same IPv4 address?  Absolutely, especially from computers on your same ISP in Dyubai, so send an undo email and “knock it through” when you're done with your session.  Actually, send and knock  it through immediately once you connect if you want to, because the whitelist/blacklist rules only apply to new connections - established traffic continues to be allowed.  Suppose the port knocks aren't getting through, as might happen with overly restrictive wifi APs.  You could still use email, its just that you'll have to send an email asking your system to log ALL dropped traffic (“log”), knock that email through, knock a port if you want to, make, send, and knock a new email telling your system two commands, each on a separate line. The commands will be “undo log” and “report”.  Then just wait for your system to send you the log tail, and decide if your translated IP address is shown in it. Just be sure you've throttled back that amped-up logging when you're done so as to not waste excessive CPU time and disk space (that's the “undo log” I mentioned).  In case you can't even port knock your email through, before leaving town you had uncommented the hourly crontab job that will fetch and parse emails without needing the port knock, right?


Now, instead of being in Dyubai, let's say your hotel is Stateside and connects you with the same IP version native to your home system.  You'll then have two ways to choose from to whitelist yourself – port knocking or email.  You decide to use email, so you compose an email according to your pre-designed rules and send that email to the email account dedicated to your home system.   Use “whats my ip” in a web search to find your hotel's IP address for composing this email.  Then “knock it through” by following up the email with a port knock sequence against your home system's IP address which causes the system to read/parse/delete and execute the commands from all the email in that dedicated email account since it was last read/deleted.  This email communication technique can additionally be used to exert control over your system in any way you choose to script, a sort of “remote control” if you have any home automation that your surveillance system can interact with.  In Linux, this ability is very nicely trivial to implement by virtue of the CLI and shell scripting feature to read to and write from external devices.  All this email parsing without any email client software being installed - shell scripting alone in Linux is perfectly capable of, and arguably best suited for, performing this email fetch, parse, and execute function.  This by virtue of Linux' shell script controllability of system sockets. Be aware that your email signature could contain keywords, so you may want to delete it in ALL emails back to your system.


This whitelisting action also triggers your home system to send you a text and email just so you'll know every time a firewall-relaxing action happens.


I found that by avoiding the use of a formal email client for fetching and parsing incoming emails, the default conflicts between the outbound and the totally unnecessary (for this firewall setup) inbound email clients simply vanished.  Since knockd was also found to be unreliable both in operational stability and knock sequence progression if port numbers were reused/repeated, buggy [inter]operability of others' apps is minimized by having much of the functionality in scripting and iptables' “recent” module.


By virtue of the “drop” action being taken by iptables on IP communication packets from Internet computers you haven't authorized, they cannot even know of your system's existence.  So rest assured – once you install iptables and make the INPUT and FORWARD chains end with a “drop” rule, there is simply no way for probers to establish communications with your system for any of their nefarious purposes whatsoever.


By virtue of whitelist entries in iptables being allowed exclusively, ONLY whitelisted IP addresses can establish communications with your system for the purposes of viewing camera video, ssh control, or any other purpose, even simple pinging – you have no reason for your system to admit its existence to strangers, do you?


Although not strictly necessary for dropping packets, a blacklist is built up for your own information.  You might be curious where those probing packets are coming from and what ports they are probing.  By default this blacklist is made of only first-time probers in order to prevent CPU-time wasting logging a single IP address multiple times in event of DoS attacks.  You'll be notified whenever blacklist entries are removed by [your] email instructions.


Unless you've already gone to the effort of dealing with the dynamic IP address dilemma, you'll appreciate that this setup sends you [an] email[s] every time your system's IP address changes, assuming that the IP address that makes it down to your Linux system is the public one.  You configure said notification email[s] with correct links or whatever you'd like.  If your ISP prevents the public IP address from coming down onto your system, and it is a dynamic one, you would have to resort to polling methods that I suggest later, or get a static IP address.


No config files means directory names, etc. are hard-coded in the scripts.  As of this date, it will be your own project to find and change the values you feel you need to change and convert to config files, if desired.


Notice that for this system to send outbound emails, you will need an email client like mailutils, but that same email client was not easily configurable in my case to also fetch incoming emails in a way that I could parse them in scripting.  Nor was I able to easily get any other email clients I tried to work in that capacity without ruining mailutils configuration.  For email receiving, sockets shell scripting was the simplest solution for me and is the single most compelling reason to use Linux, besides it being free.  The downside with socket scripting for email retrieval is that it is non-tls (non-ssl).


IMPORTANT CAVEAT:  I cannot guarantee your ISP will allow non-ssl fetching of emails.  Cox communications, for example, verbally stated to me that they will not.  This means that I would've needed an email account from someone else if my system had been on a Cox network and if I understood them correctly.  Unless, of course, you are able to script a tls connection yourself.


Not every Linux distribution has the same set of packages by default, so you might have to install an extra package or two besides  iptables.  I'm thinking especially of mailutils and inotify-tools.


The Components

Packages to install on your server

 -- iptables (any Linux firewalling needs this)

 -- iptables-persistent (firewall rules persistence)

 -- mailutils (to alert owner by email or text)

 -- inotify-tools (good alternative to polling for changes)

 -- this set of scripts installed while your are root, including crontab.   This root requirement stems from the iptables commands in all these scripts needing it.  The scripts also need chmod 7nn rather than 5nn because a couple of them get modified to prevent their execution when they might produce conflicts if they executed
 
 Configuration scripting: crontab
 I strongly advise that you proceed with utmost caution in changing the crontab SHELL line as shown! If you don't use crontab for anything, don't worry about it.  If you DO have crontab entries, you really need to make sure they won't break when running under the new shell, if applicable.  If this new SHELL line breaks them, consider yourself in for some troubleshooting work to iron out quote mark, comparison operation, shell parameter, etc. differences between/with another shell.  Some of these entries will fail a step or two downline with the wrong SHELL line making troubleshooting even more challenging.  In other words, /bin/bash was not the default for my crontab, but all this scripting as is requires it.  (I think my original crontab shell default was something like /bin/sh – not the one you'll need for this.)
The paths are default ones from my Ubuntu 15.10 server.  Adjust them for your installation!! 
Line wrapping is employed in these pages, so copy/paste transfer will work better than looking/typing.

SHELL=/bin/bash

*/30 * * * * /etc/init.d/iptables-persistent save >/dev/null 2>&1

@reboot nice -n19 inotifywait --quiet --monitor --event modify /var/lib/dhcp/dhclient.eth0.leases|while read;do /home/homeowner/newip_no_mysql.sh;done >/dev/null 2>&1

@reboot /home/homeowner/newip_no_mysql.sh >/dev/null 2>&1

@reboot nice -n15 /usr/bin/tail -F -n 0 /var/log/kern.log|/bin/grep --line-buffered ' SRC='|stdbuf -o0 /bin/grep -v 'SRC=10\.'|stdbuf -o0 grep -v 'SRC=0\.0\.0\.0'|stdbuf -o0 grep -v 'SRC=127.0\.0\.1'|stdbuf -o0 /bin/grep -v 'SRC=192\.168\.'|stdbuf -o0 awk '{for (i=4;i<=NF;i++) {if ($i ~ "^SRC=") {{gsub("SRC=","",$i); printf $i" \""} printf "kern.log "$1" "$2" "$3; for (i=i;i<=NF;i++) {if ($i ~ "^PROTO=" || $i ~ "^SPT=" || $i ~ "^DPT") {printf " "$i}} print "\""}}}'|xargs -l1 /home/homeowner/blacklistme.sh >/dev/null 2>&1

 # 10 * * * * nice -n19 /home/homeowner/email_fetch_parse >/dev/null 2>&1

Explaining the crontab entries

iptables-persistent: used to best keep iptables rules between reboots of the system.  If this would ever fail to execute, the entire firewall functionality vaporizes permanently into nothingness, which I have had happen on very rare occasion.  Therefore, I strongly recommend you familiarize yourself with the backup aspect of this scripting called “buildiptables”.  It is a script in the home directory which is real-time updated from iptables rules changes.  Systemd compatibility coming soon.


reboot nice -n19 inotifywait...  … /var/lib/dhcp/dhclient.eth0.leases... For dynamic IP systems: Did I promise something for DHCP client systems?  This is one way to “trap” an IP address change of the system.  NOTE: I DON'T FULLY UNDERSTAND IF/HOW/WHEN/WHY ALL THE DIFFERENT ISPs/MODEMS PASS THE NEW IP ADDRESS DIFFERENTLY THROUGH THEIR CABLE MODEMS TO YOUR SYSTEM!  My DOCSIS 3 cable modems pass the routable/public IP address to my systems, but I don't know if that will be the case with everyone or all modem/ISP combinations.  If you really want the feature of being alerted when your system's routable IP address changes but don't have the public IP address coming coming all the way down to your system, you'll probably have to figure out a polling method using “dig +short myip.opendns.com @resolver1.opendns.com” or “echo -en '\x00\x01\x00\x08\xc0\x0c\xee\x42\x7c\x20\x25\xa3\x3f\x0f\xa1\x7f\xfd\x7f\x00\x00\x00\x03\x00\x04\x00\x00\x00\x00' |nc -u -w 2 stun.services.mozilla.com 3478 |dd bs=1 count=4 skip=28 2>/dev/null |hexdump -e '1/1 "%u."' |sed 's/\.$/\n/'” or something.  Maybe your ISP offers static IPs – you could ask about that.  Don't forget to adjust the “eth0” in this line as needed with the name of your ethernet interface that faces the internet.


@reboot /home/homeowner….   For dynamic IP systems: At every power-up/reboot this just launches the same script that the previous command line did.  The script, “newip_no_mysql.sh” in /home/homeowner checks whether your system's IP address changed since last check.  That script also has intact but hashed out script lines that I use for Zoneminder update via mySQL of the system IP address sent out in email/text alerts.  If you want to, modify it, un-hash it, and put it on your Zoneminder box if you have Zoneminder.  Just be ready for the “warnings” you might find after doing so telling you it is unsafe or insecure to script your mySQL password - you'll be the judge of that.


@reboot nice -n15 /usr/bin/tail -F…..This line is THE heartbeat of both the dynamic building of the optional blacklist and the detection of the port knock that triggers the email reading.  You might find it very useful to know the port numbers that probers are hitting, especially if you decide to craft a port-knocking sequence and want to avoid the popularly-hit ports, so you would allow blacklistme.sh to create a running blacklist in iptables.  However, if you decide against using the dynamically-built blacklist, make that change in blacklistme.sh so that the email-reader knocks still get detected in blacklistme.sh when this command sends the log entry there.  IF YOU HATE ZOMBIES, YOU'LL BE ANNOYED THAT THIS LINE PRODUCES ONE.  I have tried very hard to stop this line from producing a zombie but to no avail.  Thankfully, it will only produce one, not adding any more on subsequent runs.  It happens the first time after each reboot and then this `tail -F` command getting caught up, so to speak.  The zombie will appear then the next probe that comes in when this line launches the blacklistme.sh script.  Even developing a workaround (to make the zombies go away after their creation) was hardly worth my time.  I am very interested in any scripting technique you could suggest that would be a true solution here, not just a workaround nor just your guess.  You might check if the answer lies somewhere in my use of `stdbuf -o0`, but if you remove that, this line stops running in the background.  Zombies in low quantities aren't harmful.


 # 10 * * * * nice -n19 /home/homeowner/email_fetch_parse…  In case port knocking is totally unsuccessful including the knock to get your email fetched and parsed, you'll still know the system will read its emails every hour:10.  By default this is not enabled, but you might want it to be if you travel.

Shell script files, adjust details for your situation.  All go in home directory

.mailrc -- too large for inclusion in readme

Only needed for sending emails from the scripts.  If you have your system doing that to your satisfaction already, you might be better off not even installing this because your system might be using the procmail/sendmail combo instead of the postfix/mail combo I'm using.  Test for whether you need this configuration file with the command line test: echo “content”|mail -s “subject line” “youremail@gmail.com”.  If your account receives the email straightaway (not getting it discriminated against by spam filters) then you don't need nor want this file – it reveals your password.

What this file does:

More and more often, email MDAs discriminate against receiving unauthenticated emails.  This file is part of making outbound emails from your system get authenticated so they are more reliably sent to addresses controlled by discriminating MDAs.  The email account it refers to is the one your system will use in order to authenticate into for the sole purpose of sending authenticated emails out, and yet it can be the same one you want your emails to be sent to.  Basically, any normal email account works.  I actually have this being the same account that my system sends the notices to me to.

This file comes into play when the postfix-provided mail command is executed from CLI or scripts openall.sh, blacklistme.sh, and newip_no_mysql.sh.


remakebuildiptablesfromiptables script -- too large for inclusion in readme.  What this file does: see comments within it

email_fetch_parse script -- too large for inclusion in readme.  What this file does: see comments within it

openall.sh script -- too large for inclusion in readme.  What this file does: see comments within it

blacklistme.sh script -- too large for inclusion in readme.  What this file does: see comments within it


FINALLY:
See the wiki.


In Conclusion...

Be sure to chmod the scripts no less than 744.  “w” access is needed for at least one because it employs a run-time self-modifying technique to prevent multiple instances running. (email_fetch_parse)
iptables requirements means root level execution for all scripts
If you end up seeking an alternative from what I presented to you here, I would heartily recommend shorewall over ufw or fail2ban.  Ufw and fail2ban both suffer from disastrous lack of persistence; shorewall might not.
(That lack of persistence is addressed in my scripting methodology by using a backup shell script, buildiptables.sh, which can always restore the iptables ruleset and will do so automatically when needed.  In my opinion, this backup script system is only possible with iptables by not using an iptables front-end.)

Still TODO:

– Do a better job on this documentation.

--  Integrate with fwknop capability
 
– Develop IP address consolidation for iptables rules.  At the rate of my blacklist formation, without address consolidation I estimate that my blacklist would grow to 150,000 entries in 2-3 years.  Even as robust as iptables is, I don't know that it was designed for that large of rulesets.  Integrate the services of ipset and geoip if beneficial.

– Add IPv6 functionality.

– Make code look better
