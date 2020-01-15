GDB Scripts
-----------

* `Audience:`
   This section is written for those with entry level understanding of GDB.

* `Introduction:`
   This repo contains gdb scripts to peruse memory of restraintd.  It is
   especially useful to derive data from memory as a result of a crash.

*  `Steps if restraintd crashed:`
   If restraintd crashes, you gather the core file and prepare an
   environment similar to that which restraint crashed. So if the crash
   occured in RHEL7, then you install a machine with RHEL7 and same
   version of restraintd. You also need the following installed:

      `yum install gdb`

   Then do
      `gdb restraintd the-core-file-found-from-crash`

   When you execute this the first time it will fail as you are missing
   the symbols.  It will report to you what you need to run to install
   the symbols.  The command given I found to change depending on
   your OS version.  I only worked with RHEL 7&8 and saw different
   results on both.  It would be something similar to the folllowing
   in RHEL OS.

      `yum debuginfo-install restraint-0.1.39-1.el8bkr.x86_64`

   The above command installs the restraint symbols as well as
   symbols for its dependency libraries.

   Restart gdb once the debuginfo is installed.

* `Setup and Use of gdb scripts:`
   Clone this repo containing the gdb scripts.  Scripts can be found
   in directory `scripts`.  Sample output can be found in  `sample_out`.
   In gdb, do the following:

      `source ${your-root-dir}/gdb/scripts/gdb_set_env`
      `source ${your-root-dir}/gdb/scripts/gdb_peruse_recipe_task`
      `source ${your-root-dir}/gdb/scripts/gdb_peruse_glib`

   The first script should always be sourced since the other scripts
   may depend on it.  The environment scripts contains functions
   which allow you to enable debugging to external file.

   You'll likely have to move up/down the stack to get to the
   restraint function where the data structures needed are
   visible to the 'C' function.  So you run gdb 'bt' to
   see the backtrace, then run 'up 10' for example to point
   yourself to the function with visibility.

   From there, you can run the scripts like the following:

      `print_all_app_data app_data`

   Each function has a description of use so you should refer to the
   individual files for details.
