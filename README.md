# KernelHowto

1) Install git tools and setup e-mail

	apt-get install git git-email gitk

To use git send-email to send your patches through the Gmail SMTP server, edit ~/.gitconfig to specify your account settings:

	[sendemail]
        smtpEncryption = tls
        smtpServer = smtp.gmail.com
        smtpUser = yourname@gmail.com
        smtpServerPort = 587

Or use the following commands:

	git config --global sendemail.smtpuser user
	git config --global sendemail.smtpserver smtp.googlemail.com
	git config --global sendemail.smtpencryption tls
	git config --global sendemail.smtpserverport 587

Must turn on Gmail "Access for less secure apps" settings:
https://www.google.com/settings/security/lesssecureapps

2) Obtain a current source tree

	git clone git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git

Note, however, that you may not want to develop against the mainline tree directly. Most subsystem maintainers run their own trees and want to see patches prepared against those trees. See the "T:" entry for the subsystem in the MAINTAINERS file( http://lxr.free-electrons.com/source/MAINTAINERS) to find that tree.

3) Create a local branch, make your changes and commit your changes

Create a local branch:

	git checkout -b first-patch

Make your changes....

Finally, you can commit your staged changes:

	git commit -s -v

-s: Signed-off-by line that is needed at the end of your patch description. The sign-off
        is a simple line at the end of the explanation for the patch, which certifies that you
        wrote it or otherwise have the right to pass it on as an open-source patch.
-v: It will show you the diff that you're committing. This is very useful to decide whether
        you are committing the correct code.

4) Create a patch

We want to create a patch for the first commit in our history (the HEAD commit). To specify the commit before the HEAD commit, you can use either "HEAD^" or "HEAD~".

	git format-patch -o /tmp/ HEAD^

-o: specifies where to put the patch. If you've run the command correctly, you should see the command output a filename in /tmp/.

5) Style-check your changes

Check your patch for basic style violations, details of which can be found in Documentation/CodingStyle.

	scripts/checkpatch.pl /tmp/xxxxx.patch

6) Test your patch

http://lxr.free-electrons.com/source/Documentation/SubmitChecklist

7) Select the recipients for your patch

By scripts/get_maintainer.pl:

	scripts/get_maintainer.pl /tmp/xxxxx.patch

Example output of HID patch:

	Jiri Kosina jikos@kernel.org (maintainer:HID CORE LAYER)
    linux-kernel@vger.kernel.org (open list)
    linux-input@vger.kernel.org (open list:HID CORE LAYER)

8) Email your changes

No MIME, no links, no compression, no attachments. Just plain text. Once your commits are ready to be sent to the maintainer, run the following commands:

	git send-email --to sarah.a.sharp@linux.intel.com --cc gregkh@linuxfoundation.org --cc linux-usb@vger.kernel.org --cc linux-kernel@vger.kernel.org ~/patches/*.patch --annotate

--annotate:to edit the subject header and mail before it is sent.

	git send email will prompt you with who to send the message to, and other odd questions:

It will then show you the resulting email header, and ask you to confirm:

	Send this email? ([y]es|[n]o|[q]uit|[a]ll):
	Send with 'y' (or 'a': git send-email can be used to send multiple commits at once).

9) Linux Kernel Performance Test - 0-DAY kernel test infrastructure

An infrastructure Intel developed to test the Linux kernel is called 0-Day(kbuild test robot). https://01.org/lkp
It is a service and test framework for automated regression-testing that intercepts kernel development at its earliest stages, and is available to the worldwide Linux kernel community.
