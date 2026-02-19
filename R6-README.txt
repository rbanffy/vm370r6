VM/370 R6 Installation Instructions

Before you begin, you will need to have the following:
	* Hercules installed on your Linux system. I'm using 1.69, your
	  mileage may vary with older versions.
	* About 400 MB of free disk space. Once the installation is
	  complete, the VM system occupies about 200 MB.
	* telnet to access the VM console
	* x3270 to access VM 3270 devices
	* VM/370 R6 documentation or a really good memory
	* The DDR tape image files: VMREL6.ddr.aws and CPR6L0.ddr.aws

If you're reading this, you've probably already unloaded the "essentials"
tarball. I find it convenient to keep all this stuff in one directory, but
hey, that's just a suggestion. Here's what you should have:
	dmkddr.aws	IPLable DDR tape
	vm370r6.cnf	Hercules config file for VM/370
	dmkrio.assemble	The real I/O config for this VM system
	dmksys.assemble	The system config for this VM system
	release6.direct	The directory for this VM system

By the way, I'm running Redhat Linux 6.2 on a Pentium MMX 200MHz 32MB box.
I find that it performs like a 370/148 running VM/370.

OK, let's get to work:

1. dasdinit CPR6L0.3330-1 3330 CPR6L0 404 (those 0 are all zeroes)
   dasdinit VMREL6.3330-1 3330 VMREL6 404

2. hercules-370 -f vm370r6.cnf

3. in another shell window:
	telnet localhost 3270

4. back on Hercules:
	ipl 580

5. in the telnet window you should see the prompt "ENTER CARD READER..."
	input 581 3420
	output 131 3330 VMREL6
	restore all
	(restore now takes about 2 minutes)
	input 582 3420
	output 132 3330 CPR6L0
	(restore takes another 2 minutes)

6. on Hercules:
	quit

7. do step 2 again

8. do step 3 again

9. on Hercules:
	ipl 131

10. in telnet window you should see "CHANGE TOD CLOCK..."
	NO
    in telnet window you should see "START ((COLD WARM..."
	COLD
    up comes VM/370. Enable terminals like this:
	enable all

11. in another shell window:
	x3270 -model 3278-2
    after a bit of grinding, a 3270 window will appear, from its Connect
    menu select "Other...", then type
	localhost 3270
    and press the connect button

You should now see the VM/370 logo. Further instructions are outside of the
scope of this document.


NOTES
-----

There is a known problem in Hercules that causes intermittant DSP003 abends.
I'm looking into it.

Cheers to all,

Bob Abeles
