First login to the Mac mini via ssh.

Then navigate to: /Library/StartupItems/  

Note about the StartupItems directory:  The StartupItems directory is actually a legacy directory. Apple promotes using the creating a launch daemon in the LaunchDaemons folder instead of using the StartupItems directory.  Apple however has not depreciated the directory and as far as I can tell they have no plans to do so in the near future.

So then you will want to login to root, either by sudo su or just logging into the root account right off the bat.

Okay now what you do is make a directory in the StartupItems directory.  For sake of making my life easy I called my directory Firewall.  For the sake of this email I will address the directory within the StartupItems as the Firewall directory.

Verify that the Firewall directory has the owner and group of: root:wheel. 

Also make sure that the directory permissions are: drwxr-xr-x 

Okay next change to the Firewall directory.

Now touch two files. The first needs to be named the same as the directory. So in the case I named the file Firewall.  The second needs to be named StartupParameters.plist.

In the StartupItems.plist file you will insert the code from the provided plist file.

You will need/want to change a few things in the plist code.
Under the description key you will want to change the text in the string to what ever you want. 
The description string is a human readable description of the daemon. So you can name it however you want as far as I know.

Then you might want to change the Provides string.  Now this is the unique name for the process. As far as I know it cannot have spaces, or special characters, in the name.

Once you have edited that file to your liking save it and close the file.

Now for the second file.  This is where you actually set the rules to maintain.

Use the provided file as your start point. 

All you have to do is add/delete your own rules to the list that is already there following the format.

Do not remove the /sbin/ipfw -f -q flush line however.  That is the command that flushes the rules out of ipfw before setting the new rules. 

Also both the -f -q arguments are needed for the ipfw command.  If either of those are missing it could cause the launch daemon to fail.

Once you have made the changes then save that file.

Now verify that both files have the owner group of root:wheel.
Change the mode of the Firewall file to: -rwxr-xr-x 
Change the mode of the StartupParameters.plist file to: -rw-r--r--

Then you are done.

Now you can start/stop/restart the daemon by typing: /sbin/SystemStarter <start/stop/restart> <name of daemon provided within the Provides String within the plist file>

You may have to sudo start it, but if it works right you should see messages after the command is executed.

Example:
sudo /sbin/SystemStarter restart IPFWFirewall
IPFW Stop
IPFW Startup

If you see responses like above then you are good to go.  Reboot the machine to verify.

You will know it worked by doing one of two things.  You can either look at the system.log file and you should see the "IPFW Startup" message. This only confirms that the daemon started though. The real test is to type: sudo ipfw list