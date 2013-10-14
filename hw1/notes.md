
Copyright © 2013 Ladinu Chandrasinghe
Revision 2 (2013-10-13)

###Coreutils "seq"

1. Notes on findings and progress
	* Visited "GNU Core Utilities Frequently Asked Questions" page section "Where can I get the latest version of GNU coreutils?".

	* Downloaded Ubuntu 12.04 and created a virtual machine using VirtualBox. Installed `git-core` on the Ubuntu machine and cloned the repository `git://git.savannah.gnu.org/coreutils` which was mentioned in "Where can I get the latest version of GNU coreutils"

	* Read the "README-hacking" text file in `coreutils` repo and followed its directions. Ran into the follwoing issue: Ran `./bootstrap` and gave me error about not finding autoconf. Read the "README-prereq" and installed the missing dependencies.

	* After installing the dependencies was able to run `./bootstrap` fine without any errors. 

	* Ran the fowllowing commands:
		`$ git submodule foreach git pull origin master`
		` $ git commit -m 'build: update gnulib submodule to latest' gnulib`

	* Some warnings were shown after running the following command:
		` ./configure --quiet`
	
	* The abouve warnings were resolved by installing the libraries that it complained about. Then ran ` $ make` and gave the following errror: `error: token @ is not valid in preprocessor expressions`.

	* After googling the error, came up with no answers. Decided to restart the whole proccess from the begening and this time everything worked. I was able to run `make` and `make check` without any major problems.

	* There were executable coreutil files in `src/` such as `ls`, `rmdir`, `touch` etc.


2. Try building the source. Does it work for you? Who is the author of GNU `seq`? What license is it under?

	* I was able to build all of coreutils which include `seq`
	
	* When the built `seq` was executed with `--version` flag. It says the follwoing at the end "Written by Ulrich Drepper." Also in the `seq.c` file it says "Written by Ulrich Drepper" in the comments.
	
	* License GPLv3+: GNU GPL version 3 or later

3. What version of C is it? s it C++ compatible? Has a coding standard been followed? If so, what standard? Comment on the general readability of the C source code.

	* In the `config.status` file it has the following line: `$["CC"]="gcc -std=gnu99"`. So I'm guessing the C version is gnu99.
	
	* I dont know. In the `config.status` it had a line like the following: `$["CPP"]="gcc -std=gnu99 -E"`. So it may be compatible.
	
	* GNU Coding Standards has been followed.
	
	* I was able to read few function and understand most of it. In most of the places there 
were some really good comments.
	

4. What purposes does the Coreutils library serve?

	* From what I read in the gnulib manual, gnulib extend and in some cases implement certain	   system functions to increase portability.
	
	
	
5. How does the program decide whether `seq_fast` is applicable? Why is seq_fast supposed to be faster? Can you think of a way to check how much speedup seq_fast actually gives?

	* `seq_fast` function is mainly decided if the following conditions meet: 
		 * If there is no format string
		 * If there is a start and end integer
		 * If the increment is 1 or not specified
		 
	* The comments says that it buffers the integers. Rather than call `fwrite` for each  number, it computes the numbers first and store it in a buffer. Then this buffer is written using less `fwrite` calls.
	
	* Time the execution times of the `seq` command. First run the command with `--format` option. The run `seq` without the format option. For example:
		* `$ time seq --format "aa%0g" 1e5 > /dev/null`
		* `$ time seq --format 1e5 > /dev/null`

6. How might you improve upon this implementation?
	* Try to make `print_numbers` buffer the output like `seq_fast`. Also, perhaps you could have 	   two or more threads compute the output in parallel and write to stdout in chunks.
