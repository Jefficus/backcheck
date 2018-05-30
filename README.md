Backcheck is a simple regression testing tool for command line scripts and apps. It's designed for use with VERY simple programs that have no interactive behavior and dump all of its output to STDOUT. I use it to make sure that new changes to a tool don't break previously working features.

It works on a simple principle: when a command works correctly for a particular combination of arguments, take a snapshot of the command invokation and its output and store those in a vault. At any time in the future, all snapshots in the vault can be retested to be sure they still produce the correct output.

Usage: 
	backcheck [options]  
	backcheck -h | --help  
	backcheck -v | --verbose  
	backcheck -V | --version  

Options:  
	-c DIR       # Confirm all recorded snapshots are still correct  
	-l DIR       # List all snapshotted command lines  
	-r CMDSTR    # Record snapshot of CMDSTR into the vault  
	-f           # Force -r to overwrite the existing snapshot of CMDSTR  
	-t CMDSTR    # Verify snapshot of given CMDSTR, if one exists  
	-v           # Display verbose output  
	-V           # Display version information  

There are three basic operations:

	 Record a new command snapshot with:  
			backcheck -r "mycommand arg1 arg2..."

	 Confirm correct behavior of a specific command with:  
		  backcheck -t "mycommand arg1 arg2..."

	 Confirm correct behavior of all snapshots with:  
		  backcheck -c VAULTDIR

By default, the vault directory is stored in ./test-mycommand, and
that vault is created automatically if it does not already exist.

Snapshot files in the vault are named with an MD5 hash of the command
line being recorded, and the contents of the file are as follows:  

	 Line 1: the command line that generated the output
    
	 Line 2+: the generated output

Whether invoked individually or for all snapshots, verification is
performed by running the command line again, capturing its output,
and comparing to the snapshot content. If they are identical, the
test passes. If they differ in any way, the command fails.

Any data files required for testing can be stored in the vault
directory and will be ignored by the -c command so long as the
filename begins with 'data'. Files that include non-hexadecimal
characters will also be ignored, although they will be listed as
unknown files in the -l listing.


Created 2018 by Jefferson Smith
Check out my tools, videos, and novels at http://creativityhacker.ca
