# genman
Shell script wrapper around help2man

This is a script for Debian packagers which uses help2man to automate
the generation of simple man pages from the output of the `--help` option
of executables in a package.

It is easily configured in the package source;
the man page files can be generated at build time
and will be installed to the correct directories.

### Options:
    -h --help   Show a help message.
    --clean     Return everything in the package debian/ directory
                to the state it was in before running the script.
    --test <file>
                Display the manpage that would be generated for this file.
                Nothing is changed in the source directory.

### Configuration:

The script will look for files in the debian/ directory:<br>
*packagename*.*section*.genman-list<br>
*packagename*.genman-list<br>
or<br>
genman-list

If *section* ( 1~8 ) is missing, 1 is assumed.<br>
If *packagename* is also missing, it is got from dpkg-parsechangelog.<br>
The file should contain a list of the executables
(paths relative to the package root) whose manpages are to be built.
Shell globs may be used.
Multiple genman-list files can be used, for source building
multiple packages, or for different manual sections.

Built manpages will be put in debian/genman-pages/, and
their paths will be appended to an existing debian/\*manpages file,
or put in a new manpages or *packagename*.manpages file.

The script should be run from the package source root directory.
Run it manually before building the package,
or to auto-run, add this to debian/rules:<br>
(adjust the path to genman.sh if necessary)

	override_dh_installman:
		debian/genman.sh
		dh_installman

	override_dh_clean:
		dh_clean
		debian/genman.sh --clean

Also add **help2man** to the source package's **Build-Depends** in debian/control.


