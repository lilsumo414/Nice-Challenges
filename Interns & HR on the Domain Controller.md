# CHALLENGE #T0144 - INTERNS & HR ON THE DOMAIN CONTROLLER [NG]
Author: Malcolm Reed Jr.
Framework Category: Operate and Maintain
Specialty Area: Systems Administration
Work Role: System Administrator
Task Description: Manage accounts, network rights, and access to systems and equipment.

SCENARIO
Recently we concluded our first intern program here at DASWebs. While we expected it to be valuable to the intern, Rob, after working with our techs for a while on our network he brought to our attention that he could access critical systems with his basic domain credentials. After which we reviewed how things were setup we found that many domain users have access to servers and shares they shouldn't. The techs and I have already outlines the basic start to fixing these issues and we want you to handle it.

Additional Information
More details and objectives about this challenge will be introduced during the challenge meeting, which will start once you begin deploying the challenge.

# MEETING BRIEFING
Gilly Bates @gbates: Maybe we should have internships more often... not sure how no one noticed these glaring configuration issues before.

Sergio Chanel @schanel: Yup, yup! Clearly you guys have got plenty to do now!

Richard LeGrand @rlegrand: THAT WILEY INTERN ROB! WHAT HE DID TO OUR COMPANY WAS JUST UNSPEAKABLE. LETS MAKE SURE HE HASN'T FOUND A WAY TO OUR MONEY. SENT FROM MY PHONE- DICTATED BUT NOT READ.

Brimlock Stones @bstones: @rlegrand everything is fine (nothing wrong with the bank accounts), Rob did not DO anything to the company... From my understanding he merely pointed the issues out.

Gary Thatcher @gthatcher: Correct @bstones, Rob just found issues already there. Did us more of a service than anything. Anywho... @playerone let me get you caught up. Turns out anyone with a domain account can log into any domain controlled servers and access the HR and accounting shares. We need to correct this asap.

Gilly Bates @gbates: @playerone, based on what we (Gary and Myself) have discussed, we want you to fix this by going onto the Domain Controller server and creating some basic security groups for our non-technical users. AKA Brimlock and Sergio. These security groups should be named HRSec (for HR) and AccountingSec (for Accounting). Once you've made them, you'll need to put Brimlock in the Accounting Security Group and Sergio in the HR Security Group. While you're at it, also make another security group named Miscellaneous and place all other non-Admin users in that group.

Gary Thatcher @gthatcher: Indeed! And after that you will use the security groups to create limitations on the users therein! Restrict both security groups from logging in to anything but the Workstation. Then limit the security groups access to the special file shares. Only HR should have read/write/list permissions on the domain network HR share and only Accounting should have read/write/list permissions on the domain network Accounting share. You should explicity deny all permissions on the Accounting share to the HRSec and Miscellaneous groups and explicity deny all permissions on the HR share to the AccountingSec and Miscellaneous groups. These shares are hosted from the domain but they reside on the Fileshare server in case you did not know. If they're not already mapped, you can use a file browser to navigate to \BSDFILESHARE\ to access the shares.

Gilly Bates @gbates: Don't forget domain admins! We will need access to both of those shares so we can manage them!

Gary Thatcher @gthatcher: Thanks @gbates, almost forgot. Yeah, along with HRSec and AccountingSec respectively, both shares will need to give full access to the domain admin group. Oh, and now that I'm looking at it... @playerone do a little house keeping for me and disable Rob's domain account. I'd do it myself, but I somehow got removed from the Domain Admins group. Could you also add me back in?

Gilly Bates @gbates: Yeah, if you could do that it'd save me some time. Just add @gthatcher back to the Domain Admins group, as he needs full access to the network shares as well.

Gary Thatcher @gthatcher: Thanks. You'll need to cut access to both of those shares for all users that don't need access.

Now that I think about it, that Miscellaneous group that @gbates asked you to make should work for that. Once you're done making the changes to all of the groups, you'll want to log into Fileshare and restart the smbd and nmbd services to upadte the proper group members. Or you can just restart the Fileshare box. That'll work as well. This will allow us to properly access the files we need access to. If you don't do this, it could take up to an hour afterward before I can get access to those shares. I need that access as soon as possible..

Richard LeGrand @rlegrand: GUESS HE WAS FINE. THAT CRAZY ROB. WAIT. SOMEONE ELSE HAS TO GET ME COFFEE NOW. I AM NOT GOING TO BACK TO GETTING IT FOR MYSELF. SENT FROM MY PHONE- DICTATED BUT NOT READ.

# SUBMITTED DOCUMENTATION
I first logged into the VM using the given username and password. After logging in, opened Server Manager, clicked Tools and opened the Active Directory Users and Computers. On the left side of the new window, I selected daswebs.com and within it I created three new groups (HRSec, AccountingSec, and Miscellaneous) using Action -> New -> Group. Once that was completed, I right-clicked on AccountingSec -> Properties -> Members Tab -> Add in order to add Brimlock Stones as a member of the group. I then right-clicked on HRSec -> Properties -> Members Tab -> Add in order to add Sergio Chanel as a member of the group. Afterwards on the left side of the Active Directory window, I went int other Users folder under daswebs.com, right clicked Domain Admins -> Properties -> Members Tab -> Add in order to add Gary Thatcher to the group. Then I selected and right-clicked on Guest, Ione Levenetis, Richard LeGrand, Rob, sshd, sshd_server and Thanh Akasaka to add them to the group Miscellaneous. I also followed this by right-clicking Robs account and disabling the account as well as right-clicked on Sergio Chanel's and Brimlock Stones's accounts -> Properties -> Account Tab -> Log On To -> clicked the following computers radio button and inputed Workstation so now that they can only log on to Workstation.

Now I went into File Explorer, selected Network, right clicked the yellow notification and turned on network discovery and filesharing and clicked FILESHARE. I went in to the Properties of the Accounting Share -> Security Tab -> Edit and Added AccountingSec, HRSec and Miscellaneous. For the AccountingSec I only checked permissions for List/Read/Write and for HRSec and Miscellarous I denied all permissions. Then I went in to the Properties of the HR Share -> Security Tab -> Edit and Added HRSec, AccountingSec and Miscellaneous. For the HRSec I only checked permissions for List/Read/Write and for AccountingSec and Miscellarous I denied all permissions. 

Once all tasks were completed, I reviewed and verified that everything looked good and submitted the challenge.
