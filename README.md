ACL Tools for Mac OS X
======================
by Morgan Aldridge <morgant@makkintosshu.com>

OVERVIEW
--------

This is a collection of command line tools for working with file ACLs to complement the support built into `ls` & `chmod` on Mac OS X, incl.:

* `findacl` -- A wrapper for `find` which adds ACL-related primaries.
* `chgrpacl` -- Change ACEs from one group to another.
* `chusracl` -- Change ACEs from one user to another.

Development was sponsored by [Small Dog Electronics, Inc.](http://www.smalldog.com/)

FINDACL
-------

`aclfind` is a Mac OS X-specific wrapper to `find` that adds knowledge of ACLs. In Mac OS X, only `ls`, `chmod`, and `vim` command line utilities know about ACLs, and the latter only enough to preserve them. `find` is an extremely powerful tool, esp. for finding and adding/changing/removing permissions, but it's not helpful at all when it comes to ACLs, so this utility aims to improve that. It is written in `bash`, so it's slow, but I may port it to C at some point (or patch `find` itself?)

### USAGE

The usage of `aclfind` is identical to `find` since it's really just a wrapper for it, with the addition of the following _primaries_. See [`man find`](https://developer.apple.com/library/mac/#documentation/Darwin/Reference/ManPages/man1/find.1.html) for details.

#### Primaries

	-noacl
	        True if the file has no ACL.
	
	-aceuser uname
	        True if the file has an ACE for the user 'uname'.
	
	-acegroup gname
	        True if the file has an ACE for the group 'gname'.
	
	-aceperm permission
	        True if the file has an ACE containing 'permission'. Accepts a comma-delimited list of permissions.
	
	-acelocal
	        True if the file has a 'local', non-inherited, ACE.
	
	-aceinherited
	        True if the file has an 'inherited' ACE.
	
	-aceallow
	        True if the file has an 'allow' ACE.
	
	-acedeny
	        True if the file has a 'deny' ACE.
	
	-printace
	        This prints any matching ACE after the filename, indented by a single space and prepended with the ACE number in the ACL (the same format that `ls` outputs ACEs in).
	
#### Operators

**Note:** `aclfind` does not add any operators, but it also doesn't support operators at the moment and just ANDs them together. This is planned for a future release.

### TO-DO

	[x] Implement `-noacl`.
	[x] Improve `-aceperm` to accept a comma-delimited list of permissions.
	[x] Support inherited ACEs.
	[ ] Add `-aceallowuser`, `-acedenyuser`, `-aceallowgroup`, `-acedenygroup`, `-aceallowperm`, `-acedenyperm` primaries?
	[ ] Override `-exec` so we're performing it _after_ we get results back from `find` (it should never be passed through to `find`).
	[ ] Add unit tests.
	[ ] Build a proper `find` path to support operators.

CHGRPACL
--------

`chgrpacl` is wrapper for `findacl` which allows you to change ACEs on files & directories from one group to another, much like `chgrp` lets you change the group for POSIX permissions. This is ideal if you need to replace group ACEs in a complex structure, for example you've had to create a new group in order to change the group's short name.

#### USAGE

The usage of `chgrpacl` is very similar to `chgrp` with the exception that you must provide the old group name as well as the new group name:

	chgrpacl [-R] oldgroup newgroup file

By default, `chgrpacl` will only change the ACEs for the specified `oldgroup` for the specified `file`, but one can use the `-R` option to change them recursively for all children as well.

CHUSRACL
--------

`chusracl` is also a wrapper for `findacl` which allows you to change ACEs on files & directories from one user to another, much like `chown` lets you change the owner for POSIX permissions. This is ideal if you need to replace group ACEs in a complex structure, for example you have one user taking on the role (and therefore the permissions of) another user.

#### USAGE

The usage of `chusracl` is very similar to `chown` with the exception that you must provide the old user name as well as the new user name:

	chusracl [-R] olduser newuser file

By default, `chusracl` will only change the ACEs for the specified `olduser` for the specified `file`, but one can use the `-R` option to change them recursively for all children as well.

REFERENCE
---------

* [Mac OS X Hints: Script to list all filesystem objects with ACLs](http://hints.macworld.com/article.php?story=20080816224959309)
* [Stack Overflow: remove an ACL entry for just ONE user in MacOS? oddly difficult](http://stackoverflow.com/questions/637871/remove-an-acl-entry-for-just-one-user-in-macos-oddly-difficult)
* [Apple Source Browser: find](http://www.opensource.apple.com/source/shell_cmds/shell_cmds-162/find/)
