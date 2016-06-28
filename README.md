# Boilerplate code for compiled functions and classes linked to ROOT

## Step 1

Clone the repository:

<code>
git clone https://github.com/Jollyhrothgar/root_project_generator
</code>

## Step 2

Run the script (example):

<code> ./make_project.py --build_dir build --source_dir source --macros_dir
macros -classes OneClass AnotherClass ThirdClass --functions int,FirstFunction
double,SecondFunction --lib_name MyLibrary --install_dir $MYINSTALL</code>

Note, you can use shorter versions of each CLI flag - just type "make_project.py
-h"

This generates your project area with separated build, source and macros
directories. Run it, and see how your project is structured. You also get a
Makefile and a cleaning script as well. 

Note you can call the classes and directories whatever you want. You can
overlap directories as well. Some people prefer to build in their source
directory. In that case just do: -s src -b src, this will put everything in a
directory called 'src'. This only effects where the fullClean.sh script looks
to delete unnecessary build files, if you want to clean up your work area or
re-compile.

## Step 3 

Go to your build directory, and generate the makefiles for your project with:

<code>
../source/autogen.sh --prefix=$MYINSTALL
</code>

## Step 4
Compile and install your project with:

<code>
make install
</code>

## Step 5
Now, go to the macros directory, and test the project with:

<code>
root -l Run_Tests.C
</code>

You should see a series of outputs confirming that each one of your functions
and classes was instantiated.

## Next Steps

You can now modify your classes and functions as you wish - add member functions
to classes to abstract data types, add arguments to functions, etc. Modify as you wish.

## Requirements

You should have the path variable $MYINSTALL defined. It points to a directory
on your computer containing "$MYINSTALL/lib". This is where all your compiled
libraries should be. $MYINSTALL should be added to your $PATH variable, so that
you can use your root libraries from any location on your system.

In bash, (i.e. ~/.bashrc), this would look like:

<code>
export MYINSTALL=/direct/phenix+u/workarea/beaumim/install
</code>

<code>
export PATH=$PATH:$MYINSTALL
</code>

In cshell, (i.e. ~/.cshrc), this would look like:

<code>
setenv MYINSTALL /direct/phenix+u/beaumim/workarea/install
</code>

<code>
set path = ($MYINSTALL/bin $path)
</code>

If you use this script on a cluster, you will need to modify the autogen.sh file
to point to wherever libtoolize is installed (for PHENIX, its in $OFFLINE_MAIN).

You also need to have ROOT 5.34 installed. Running an earlier version should
also work, assuming that it supports CINT, and uses the Dict system of managing
building streamers for interactive use.

This will not work for ROOT 6, it will need to be modified. The C++ stuff will
be just fine, but ROOT 6 does not handle dictionary generation (needed to run
your functions and classes interactively in CINT) the same way as ROOT 5.34.

## Documentation

Lists of classes and functions are white-space separated (i.e.):

* --classes foo bar baz
* --functions int,foo float,bar bool,baz

The only optional argument is --overwrite. Add this to the argument list if and
only if you wish to overwrite classes and functions with the blank template used
in this program. This way, you can add new classes or functions to your project
and get an automatically generated makefile and LinkDef for free.

<pre>
  -h, --help            show this help message and exit
  -c [CLASSES [CLASSES ...]], --classes [CLASSES [CLASSES ...]]
                        List the names of the classes to add to the project
  -f [FUNCTIONS [FUNCTIONS ...]], --functions [FUNCTIONS [FUNCTIONS ...]]
                        List the function and type as:<TYPE>,<FUNCTION NAME>
  -l LIB_NAME, --lib_name LIB_NAME
                        Give a name for the library
  -s SOURCE_DIR, --source_dir SOURCE_DIR
                        Give a path for the source directory
  -m MACROS_DIR, --macros_dir MACROS_DIR
                        Give a path for the macros directory
  -b BUILD_DIR, --build_dir BUILD_DIR
                        Give a path for the build directory
  -i INSTALL_DIR, --install_dir INSTALL_DIR
                        Give a path for your library installation directory
  -o, --overwrite       Forces all generated files to overwrite existing
                        files. Otherwise, only new classes and functions are
                        written. Other boilerplate code that is not generally
                        modified by the user is also regenerated (i.e.
                        autogen.sh, Makefile.am, etc.)
</pre>

## Standard Disclaimer

Any project you're working on should implement a frequent back up strategy. You
should commit your code edits frequently to a repository like github. If you
overwrite your whole project by using this tool, and you don't have a recent
back up, that's your fault.  I've tried to make sure this can't happen unless
you force it, but there may be bugs, so please proceed with caution.
