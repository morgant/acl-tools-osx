aclfind
=======
by Morgan Aldridge <morgant@makkintosshu.com>

OVERVIEW
--------

`aclfind` is a Mac OS X-specific wrapper to `find` that adds knowledge of ACLs. In Mac OS X, only `ls`, `chmod`, and `vim` command line utilities know about ACLs, and the latter only enough to preserve them. `find` is an extremely powerful tool, esp. for finding and adding/changing/removing permissions, but it's not helpful at all when it comes to ACLs, so this utility aims to improve that. It is written in `bash`, so it's slow, but I may port it to C at some point (or patch `find` itself?)

USAGE
-----

The usage of `aclfind` is identical to `find` since it's really just a wrapper for it, with the addition of the following _primaries_. See `man find` for details.

### Primaries

	-noacl
	        True if the file has no ACL.
	
	-aceuser uname
	        True file has an ACE for the user 'uname'.
	
	-acegroup gname
	        True if the file has an ACE for the group 'gname'.
	
	-aceperm permission
	        True if the file has an ACE containing 'permission'.
	
	-aceallow
	        True if the file has an 'allow' ACE.
	
	-acedeny
	        True if the file has a 'deny' ACE.
	
	-printace
	        This prints any matching ACE after the filename, indented by a single space and prepended with the ACE number in the ACL (the same format that `ls` outputs ACEs in).
	
### Operators

**Note:** `aclfind` does not add any operators, but it also doesn't support operators at the moment and just ANDs them together. This is planned for a future release.

TO-DO
-----

	[x] Implement `-noacl`.
	[x] Improve `-aceperm` to accept a comma-delimited list of permissions.
	[ ] Build a proper `find` path to support operators.

REFERENCE
---------

* [Mac OS X Hints: Script to list all filesystem objects with ACLs](http://hints.macworld.com/article.php?story=20080816224959309)
* [Stack Overflow: remove an ACL entry for just ONE user in MacOS? oddly difficult](http://stackoverflow.com/questions/637871/remove-an-acl-entry-for-just-one-user-in-macos-oddly-difficult)
* [Apple Source Browser: find](http://www.opensource.apple.com/source/shell_cmds/shell_cmds-162/find/)
