This file explains some details about developing the Augeas C library.

# Check out the sources

The sources are in a git repo; to check it out run

```
  git clone git://github.com/hercules-team/augeas
```

# Building from git

Besides the usual build tools (gcc, autoconf, automake etc.) you need the
following tools and libraries to build Augeas:

* Bison
* Flex
* readline-devel
* libxml2-devel
* libselinux-devel (optional)

On Raspbian Jessie:

* sudo apt-get install autoconf

* sudo apt-get install bison
* sudo apt-get install flex
* sudo apt-get install libreadline-dev
* sudo apt-get install libxml2-dev

Augeas uses gnulib, and you need a checkout of gnulib. The build scripts
can create a checkout for you behind the scenes - though if you already
have a gnulib checkout, you can pass its location to autogen.sh with the
--gnulib-srcdir option.

At its simplest, you build Augeas from git by running the following
commands in the toplevel directory of your Augeas checkout:

    ./autogen.sh [--gnulib-srcdir=$GNULIB_CHECKOUT]
    make && make install

It is recommended though to turn on a few development features when
building; in particular, stricter compiler warnings and some debug
logging. You can pass these options either to autogen.sh or to
configure. You'd then run autogen like this:

    ./autogen.sh --enable-compile-warnings=error --enable-debug=yes

# Running augtool

The script ./src/try can be used to run ./src/augtool against a fresh
filesystem root. It copies the files from tests/root/ to build/try/ and
starts augtool against that root, using the lenses from lenses/ (and none
of the ones that might be installed on your system)

The script can be used for the following; OPTS are options that are passed
to augtool verbatim

* `./src/try OPTS`: run the commands from `build/augcmds.txt`
* `./src/try cli OPTS`: start an interactive session with augtool
* `./src/try gdb`: start gdb and set it up from `build/gdbcmds.txt` for
  debugging augtool with commands in `build/augcmds.txt`
* `./src/try valgrind`: run the commands from `build/augcmds.txt` through
  augtool under valgrind to check for memory leaks

# Platform specific notes

## Mac OSX

OSX comes with a crippled reimplementation of readline, `libedit`; while
Augeas will build against `libedit`, you can get full readline
functionality by installing the `readline` package from
[Homebrew](http://brew.sh/) and setting the following:
```sh
> export CPPFLAGS=-I/usr/local/opt/readline/include
> export LDFLAGS=-L/usr/local/opt/readline/lib
```
