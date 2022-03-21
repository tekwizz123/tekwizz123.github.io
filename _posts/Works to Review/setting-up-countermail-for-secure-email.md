---
title: 'Setting Up CounterMail For Secure Email: A Guide'
date: 2013-07-30T16:37:00.001-07:00
draft: false
url: /2013/07/setting-up-countermail-for-secure-email.html
---

Hey again guys,

Today im going to show you how I set up a secure CounterMail email address. When I say secure, keep in mind that what I am saying is that it is very hard for anyone to read the content of your emails. I do not mean that this email will be anonymous in any way. That is potentially possible with this setup, but I won't get into details of it here as that is a sepearte topic (email me if your interested at the email in the last blog post). It is also important to note that no matter which secure email provider you use, email headers and senders/recipients can never be encrypted when dealing with the SMTP protocol that is used by email today. Sorry folks, just the way it is.) With that out of the way, lets get started.

Oh and sorry folks, its going to be Windows this time around. If you want to set this up on Linux, then no problemo. Just follow along, but adapt the instructions so that it works for a Linux OS on a memory stick. Don't forget to also encrypt the entire memory stick while your at it :)


### Encrypt All The Data....With Truecrypt

To start off with, your going to need to set your self up a nice good old Truecrypt encrypted folder.

To do this, install Truecrypt from https://www.truecrypt.org/. Then, open up Truecrypt and select "Create Volume"


[![](https://lh3.ggpht.com/-Xkeg46bBgWY/UfgCC6tsCJI/AAAAAAAAAJ4/Cj5S3NRNefQ/s0/TrueCrypt+Setup+1.PNG)](https://4.bp.blogspot.com/-Xkeg46bBgWY/UfgCC6tsCJI/AAAAAAAAAJ4/Cj5S3NRNefQ/s0/TrueCrypt+Setup+1.PNG)


Then go ahead and click on that next button a few times, making sure that "Create an encrypted file container" is selected.


[![](https://lh3.ggpht.com/-cQQ-uFl-3G0/UfgCCz3sSJI/AAAAAAAAAJ8/AaLg1dX6nyk/s0/TrueCrypt+Setup+2.PNG)](https://2.bp.blogspot.com/-cQQ-uFl-3G0/UfgCCz3sSJI/AAAAAAAAAJ8/AaLg1dX6nyk/s0/TrueCrypt+Setup+2.PNG)


Eventually, you'll be asked to pick a location to store the encrypted file in. Pick a location, make sure "Never save history" is checked, and hit next.


[![](https://lh3.ggpht.com/-g2vsAGhRQIw/UfgC-cC1CXI/AAAAAAAAAKM/_YOBtoYWDzk/s0/TrueCrypt+Setup+3.PNG)](https://3.bp.blogspot.com/-g2vsAGhRQIw/UfgC-cC1CXI/AAAAAAAAAKM/_YOBtoYWDzk/s0/TrueCrypt+Setup+3.PNG)


You will then be asked what encryption you want to use. Its fine to accept the defaults if you have no idea what your doing. Alternatively, feel free to fiddle around here for extra security (example doing triple hashing of the data for maximum security).

You will then be asked how big you want the container to be. Make this big enough that it can fit any future expansion as once created you cannot adjust the size of the container (well, unless you like to watch encrypted data null itself). Be careful here ;)

You then need to set up a password for the encrypted file. What I recommend you do is use a passphase. I don't want to see any of you people using anything under at least 20 characters for this. Im not joking, make it strong. This is what will be protecting your data. If you having trouble creating a passphase, consider the following:

correct horse battery staple

Thats 4 words that you can remember (just an example, don't use this as your password) that when combined together would form your final password:

correcthorsebatterystaple

And thats 25 characters long. See how easy that was? Just pick 4 words that you can memorize, combine them together, and add some numbers that you will remember. Then your all good to go here.

Once this is done it will ask you to move your mouse around randomly. This will introduce some additional entropy into the randomizer that will make the encryption more secure. The longer you do this, the more secure it will be, but don't go overboard on this. A good 2-3 minutes tops will do, 5 minutes if your paranoid. When your ready, click on "Format". Done? Make yourself a cup of coffee, cause this may take a while.

Ok, all good? Great :) Now that thats done, go back to the main TrueCrypt menu, and select a mount point from the selection above. You've got A:\\, B:\\, G:\\, M:\\, D:\\, the list goes on. Pick one :)

Now that you've done that, click on "Select File" and browse to the encrypted partition you created. Select it, hit ok to confirm your selection, and then click the big old "Mount" button on the bottom left. Enter the password for the encrypted partition, and click "Ok". If all went well, the partition should be mounted. You can then use your directory navigator, go to "My Computer", and then select the partition you mounted the encrypted partition on. Congrats, thats TrueCrypt set up :)


[![](https://lh3.ggpht.com/-BH4xPvX2V5I/UfgHQkYFyCI/AAAAAAAAAKc/u5WVKfSa9gI/s0/TrueCrypt+Setup+4.PNG)](https://1.bp.blogspot.com/-BH4xPvX2V5I/UfgHQkYFyCI/AAAAAAAAAKc/u5WVKfSa9gI/s0/TrueCrypt+Setup+4.PNG)

### Setting up the Portable Apps

Ok, now for the fun stuff. Encryption can be tough to set up (oh and there more to come), but this part should be pretty straightforwards.

Download the following to the encrypted partition we just mounted:

*   https://portableapps.com/apps/internet/firefox\_portable
*   https://portableapps.com/apps/internet/thunderbird\_portable
*   The vanilla version of GPG4Win: https://files.gpg4win.org/Beta/gpg4win-vanilla-2.2.0-beta34.exe (if on Linux just install PGP. It does the same thing.)
*   https://www.enigmail.net/download/ <- Download the correct version for the current version of Thunderbird that you have. (Linux users will also need to do the same for their OS).

I'll presume your smart enough to install the first two :P Just make sure all files are installed to the encrypted partition and NOT YOUR NORMAL PC. To install GpG4win install it normally, but make sure all the files are installed to the encrypted partition. Note it will install stuff to your normal PC as well. This is required for the program to operate. Just make sure the main executables are on your encrypted partition.

Next, for Enigmail, open up Thunderbird, go to addons (if you can't find it, look in the upper right for three horizontal lines, and click on that button, then on addons) and click on it.

Find the cog near the "Search all addons" search bar. Click on the cog, and then click on "Install Addon From File". Browse to our encrypted partition, find the Enigmail file you downloaded, select it, and hit ok.When asked to restart Thunderbird, restart it. Enigmail should now be installed.


To finish our Enigmail installation, we need to make one final adjustment. Open up Thunderbird again. Follow the same instructions to get to the Addon's menu, but instead opening the addon's menu, select OpenPGP, then select "Preferences". Click on the checkbox called "Override with" and enter in the location of the gpg.exe executable on your encrypted partition. If you installed it to a folder called GnUPG, then it would be under <encrypted partition>\\GnUPG\\pub\\gpg.exe.


![](https://lh3.ggpht.com/-eNnTHZJeARQ/UfgSEDlE7_I/AAAAAAAAAKw/mcz9c5QpCAo/s0/setup+5.PNG)

Altering GnuPG to use our encrypted copy of gpg.exe


Setting Up A Countermail Account
Ok, so now that we have everything setup for the most part, we need to get a CounterMail account. To do this, go to https://countermail.com/index.php?p=changelog&t=1374961742. If you know how to check your checksums, then download the checksum list at the bottom of the page to our encrypted partition.. Download the offline login client as well to the encrypted parition.

Ok, now we should be good. Close all open Firefox and Thunderbird sessions otherwise the next few steps won't work. Unzip the Offline Login Client zip file and extract the contents to the same location as Firefox.exe (located wherever you installed Firefox Portable). You should now have a Firefox folder that looks something like this:




[![](https://lh3.ggpht.com/-6duybzYzjRY/Ufg9vJX2W-I/AAAAAAAAALM/HkHR19uGOLM/s0/setup+6.PNG)](https://4.bp.blogspot.com/-6duybzYzjRY/Ufg9vJX2W-I/AAAAAAAAALM/HkHR19uGOLM/s0/setup+6.PNG)

Firefox all nice n set up


Double click on FirefoxPortable.exe. You should get a message about the CounterMail app loading, followed by a Java alert. This is because CounterMail uses Java for its web interface to ensure that all of the encryption and decryption of emails is done on the client side rather than the server. Don't worry, for those of you worrying about the obvious security percussions of this, you can check the checksum of each Java applet by looking for a link on the applet (should say "View" and then certificate or something similar) against the hashes you downloaded earlier. If they match, you can be sure that no one is tampering with your connection and that the app is genuine.

However, personally, I don't like Java. Reason why? Just look at: https://istherejava0day.com/ to see if Java is still going unpatched to this day and https://java-0day.com/ to see how many days Java has gone unpatched for from the latest 0day found (there's actually at least 2 0days facing the last version of Java as I write this today).

With that aside, bear with me for a bit while we put up with a bit of Java stuff. Go to https://countermail.com/index.php?p=signup with Firefox Portable and wait for the Java applet to load. Once it has loaded, accept the terms and conditions, and then click continue. Wait for a little bit. You may get some more messages about Java applets. You may check these hashes as well or just accept them off the bat. Once you let them run, a popup box will appear. Here you need to enter the login information and password that you would like to use to log into CounterMail. Make the login password long but memorizeable, as you will need this for the next few steps as well as if we encounter any errors.

After you submit your username and encrypted hash to the server (the Java applet will automatically encrypt your password before it is stored on their servers) you will be asked to move your mouse around randomly for a few seconds. This will generate the PGP keys for the server. Once this is done, you will then be taken back to the main screen where you will have to click on the login button again and log in. Once this is done you will see the main interface:

From here we need to upgrade our account. Look to the lower left. You should see a notification about the number of days you have left running out. Below this there is a button saying something about buying premium services. Click on this. You will then be redirected to a page to buy premium services. Select the number of month slots you want to buy (there are 3 months, 6 months and 1 year slots. Max is 2 years at one time I believe, but I could be wrong). Once you have confirmed how many years you want to buy and if you want to buy a USB key that will be used as a sort of two factor authentication (wanted to try this, but wasn't so sure about it having to be shipped to my address, You can also make your own if you buy a credit and have the appropriate equipment though) you then need to select a method of payment.

**UPDATE 20th August 2014:** A user informed me that Bitcoin is now accepted by CounterMail, therefore the instructions below are no longer valid. Instead it is best to open a Bitcoin account, find an anonymous way of getting coins into it (Google around, there's a number of solutions for this including buying in person) and then pay for your access this way if you want to remain as anonymous as possible. If anonymity is not a huge concern there is always Paypal or better yet a prepaid credit/debit card.

Here is where things get interesting, and the anonymity aspect is lost. Although CounterMail has been trying to accept Bitcoins as a means of payment (and if they do go that way, then this paragraph will disappear), they currently haven't implemented it. As thus you have 3 methods of payment, and only one is a possible option for anonymous payment.

1.  Credit card: Obviously not very anonymous + charges could be high for transfer fees.
2.  Paypal: Not too bad. They delete the transaction from their records after 14 days if your willing to wait that long.
3.  Prepaid Debit/Credit Card: Best option if you can find a way to fund it anonymously. Problem is if you pay for it in cash at a store, the card number could still be linked to you via CTV. If you can find a way to pay for it via Bitcoin and also deal with the constant fees that may occur if you don't use the card, then this is probably the best option for the time being.

For this reason I will not offer any other advice on how to go about setting up anonymous payment etc. Google is you friend for these things.

Ok, so you paid and it went through right? Now for some fiddly stuff. You should be sent back to your account by now. Wait about 5-10 minutes for the payment to go through, then refresh the page. You should see your account has been upgraded.

Ok, next we need to delete the alias that was used for our payment so that it no longer can exist as a link between the payment and any future emails (again Paypal and credit cards will keep records so keep that in mind). To do this, first go to the main interface:






[![](https://lh3.ggpht.com/-ZbsjZgMftOE/UfhITdtqz2I/AAAAAAAAAL4/blT1WUQ52IA/s0/setup+7.PNG)](https://2.bp.blogspot.com/-ZbsjZgMftOE/UfhITdtqz2I/AAAAAAAAAL4/blT1WUQ52IA/s0/setup+7.PNG)


From here, select the "Settings" button on the upper right of the interface.


[![](https://lh3.ggpht.com/-qNdqgebqnTc/UfhIT3BejTI/AAAAAAAAALk/l23DNjKCyUY/s0/setup+8.PNG)](https://3.bp.blogspot.com/-qNdqgebqnTc/UfhIT3BejTI/AAAAAAAAALk/l23DNjKCyUY/s0/setup+8.PNG)


Click on Personal Information/Keys and Passwords


[![](https://lh3.ggpht.com/-tyAK3vxR9S4/UfhIUGHnBzI/AAAAAAAAAL0/7S9VS-rxbwk/s0/setup+9.PNG)](https://2.bp.blogspot.com/-tyAK3vxR9S4/UfhIUGHnBzI/AAAAAAAAAL0/7S9VS-rxbwk/s0/setup+9.PNG)

 Select the alias that you purchased the account with from under "Selected alias" and hit the Delete button under "Alias signature". Good, now thats dealt with, now to deal with our last problem. Ready for some more encryption?


### Attaching Thunderbird to Countermail

See the excellent guide at https://countermail.com/?p=support#12 for how to do this before proceeding. They explain it better than I can ;)  You can choose to save the IMAP password that is used to connect to the server or not. Just keep in mind if you don't your going to have to load up that Java interface again :)


### That Pesky OpenPGP Problem

So here we get to the reason you need a paid account. First off the free account is limited to 7 days, but second off, you can't delete your private OpenPGP key from their server without a paid account. To do this, go to the same page that you had open to delete your alias. Scroll down until you see "Delete Privkey". You will be asked your password, at which point you will then get a copy of the private key to download n store. Keep this is a safe place just incase you need it. The private keys will then be deleted off the CounterMail server.

At this point we now need to create our own PGP keys to allow us to communicate with others without even CounterMail knowing whats going on.

To do this, open up Thunderbird Portable. Go to the Settings menu we used to access Addons, but this time select OpenPGP, and then select "Key Management". On the upper right side of the menu, select "Generate" and then "New Key Pair". Select your CounterMail email address we attached to in the last step. Enter a passphase for the new key (keep it nice n long folks) using the same passphase ideas I layed down for your original password. Make this one especially secure. This is what will be protecting your encrypted emails. Don't forget it though! Next, under advanced, set the key length to 4096.


[![](https://lh3.ggpht.com/-gYLqD4Cb2VY/UfhMKj_C37I/AAAAAAAAAMM/JDkgNzB51a4/s0/setup+10.PNG) ](https://3.bp.blogspot.com/-gYLqD4Cb2VY/UfhMKj_C37I/AAAAAAAAAMM/JDkgNzB51a4/s0/setup+10.PNG) 

After this, hit the "Generate key" button. Do some random intensive stuff. Password cracking works very well when you do it using a CPU :D Just saying. Otherwise browse some websites, open up tabs, do some random keypresses, anything to introduce additional randomness into the system.


Once the key generation is done, we only have one final step to do. We need to upload our public half of the keys to the server so that other people can encrypt messages specifically for our eyes only. This is relatively straightforwards. Simply go back to the Key Management window, select the "Display all keys by default" check button, select the key for your email, right click on it, and select "Upload Public Key To Server". Select any one of the 3 servers available, then repeat the process with the other two.

Congratulations. You made it :) All done. When you are finished working with your email, make sure to dismount the TrueCrypt volume by right clicking on the TrueCrypt icon (should be a dark brown) in the notification area of Windows and selecting "Dismount All".

Thanks for reading, and I hope this helps make a bit of a difference to you. If you spot any errors in this, please forward it on to me as I wrote this in the wee hours of the morning ;)

\-tekwizz123