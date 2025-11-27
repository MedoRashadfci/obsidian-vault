We can test out the URL parameter by adding payloads to see how the web application behaves. Path traversal attacks, also known as the dot-dot-slash attack, take advantage of moving the directory one step up using the double dots ../. If the attacker finds the entry point, which in this case get.php?file=, then the attacker may send something as follows, http://webapp.thm/get.php?file=../../../../etc/passwd

Suppose there isn't input validation, and instead of accessing the PDF files at /var/www/app/CVs location, the web application retrieves files from other directories, which in this case /etc/passwd. Each .. entry moves one directory until it reaches the root directory /. Then it changes the directory to /etc, and from there, it read the passwd file.
The same concept applies here as with Linux operating systems, where we climb up directories until it reaches the root directory, which is usually .

Sometimes, developers will add filters to limit access to only certain files or directories. Below are some common OS files you could use when testing.

 
Location 	Description

/etc/issue
	

contains a message or system identification to be printed before the login prompt.

/etc/profile
	

controls system-wide default variables, such as Export variables, File creation mask (umask), Terminal types, Mail messages to indicate when new mail has arrived

/proc/version
	

specifies the version of the Linux kernel

etc/passwd
	

has all registered users that have access to a system

/etc/shadow
	

contains information about the system's users' passwords

/root/.bash_history
	

contains the history commands for root user

/var/log/dmessage
	

contains global system messages, including the messages that are logged during system startup

/var/mail/root
	

all emails for root user

/root/.ssh/id_rsa
	Private SSH keys for a root or any known valid user on the server

/var/log/apache2/access.log
	

the accessed requests for Apache web server

C:\boot.ini
	contains the boot options for computers with BIOS firmware
