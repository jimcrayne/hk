Reminder: It is not especially secure to put sensitive information such as passwords on the command line regardless of the tool. If you need to input such things to your scripts, the secure way it is to use stdin.

hk
==

Bash script for running some quick haskell on the command line


USAGE: hk [OPTIONS] [CODE]

hk facilitates running some quick haskell at your shell prompt

The type of CODE can be an IO type or anything for which there is
a Show instance. If unsure, try something and then just run hk -e to see
the actual program in vim or the editor indicated by the EDITOR
environment variable. Remember to observe the quoting rules of your shell;
for example, in bash, you probably want to enclose CODE in single quotes.

OPTIONS:
=======
  -e       Edit the last script and then run it in current directory

  -d       Dump last script to stdout

  -p CODE  Pipe mode, apply CODE to contents of stdin

  -h       This help screen

EXAMPLES:
========
  Assuming Bash quoting rules:

  # Use hk as a calculator

	$ hk sum [1..10]/5
	11.0

  # Sum numbers in file 'Nums'

	$ hk 'readFile "Nums" >>= print . sum . map read . lines'
	1123

  # Alternative to above using pipe

	$ cat Nums | hk -p 'sum . map read . lines' 
	1123

  # If getArgs occurs in first parameter, then interpret the rest as command line arguments

	$ hk 'getArgs>>= print . sum . map read' 1000 100 20 3
	1123

  # Note: -p is only for convenience, it may be preferable to call getContents explicitly

	$ echo -e "ok\nthen\nsome" | hk 'liftM2 zip getArgs (fmap lines getContents)' arg1 arg2 arg3
	[("arg1","ok"),("arg2","then"),("arg3","some")]
