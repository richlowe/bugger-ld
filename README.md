# bugger-ld

This is a wrapper around the illumos link-editor such that all (well, *most*)
inputs are gathered along with diagnostic output and the command line.

The resulting tarball(s) can then be provided to greatly ease the debugging of
the link editor since they reproduce the build environment as faithfully as
possible.

Unfortunately, a script such as this will always have various flaws in unusual
cases, and may perhaps behave badly.

## Usage

set `LD_ALTEXEC` to point to the bugger-ld script, set `BUG_DIR` to the path
to a directory in which tarballs (one per invocation of `ld`) should be
created.  Build your software.

```
LD_ALTEXEC=bugger-ld BUG_DIR=$PWD/bugs make
```

If the link-editor you are really running is not `/bin/ld` (perhaps you want
to test an alternate linker _and_ use this, for some reason), set `REAL_LD` to
the path to the link-editor to actually invoke.

When your build fails (or whatever), you will find in `$BUG_DIR` a series of
tarballs with hexadecimal names, and an `index` file matching each name up
with the (full) output path the link-editor was creating at the time.  Some
(or all) of these contain the data needed to debug the link-editor on an
arbitrary system.

## Contents

- .../build.sh  

  A shell script which should reproduce the build precisely (modulo bits we
  can't save)
  
- .../command-line

  The pristine command line with which the link-editor was invoked
  
- .../current-directory

  The (absolute) directory from which the linker was invoked
  
- .../environment

  The full process environment
  
- .../objects/

  Every file used as input to the link-editor, stored with its absolute path
  below `objects/`, i.e. `objects/usr/lib/libc.so.1`
  
- .../output

  The output of the link editor, which we explicitly invoke with
  `-Dall,detail,long`
  
- .../version

   The output of `ld -V`  
