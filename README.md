## Zlib for use with Hercules

This repository contains an unmodified copy of Zlib 1.2.11, retrieved
October 2017 from http://www.zlib.net/ and stored in the `pkg_src`
subdirectory.  This should allow a complete replacement of the `pkg_src`
directory contents when an uplevel upstream Zlib distribution becomes
available.

A CMake build script has been added.  The CMake script, while based on
makefile and and makefile.msc in the upstream Zlib distribution, is
specific to Hercules' requirements and only builds a z shared library
(.so or .dll). The build script exports targets for the build
directory, and for the installation directory if the z shared
library is installed.  The CMake build for Hercules can import either
target.  If Zlib is built by the CMake build for Hercules, the build
directory target is used.

File Zlib_version.txt line one contains the version of Zlib available
in `pkg_src`.

The CMake build for Hercules will clone and build this repository if
Zlib is not installed on the target system, or if the version installed
on the target system is older than the version in this repository.

The CMakeLists.txt in the top level directory is a modified version of
the original CMakeLists.txt in pkg_src; modifications are licensed using
the same BSD license as the unmodified version.  Other programming in the 
top level is copyright by Stephen Orso and is licensed under the Boost 
license.   Documentation and other writing is licensed using the Creative 
Commons By-SA 4.0 license.

Each file contains specific copyright and license information.

The Zlib package retains its original copyright and license, which can be found at `pkg_src/LICENSE'

---
&nbsp;
### CMake -D Configuration Option Variables


Except for `BUILD_TESTING`, the configuration options used by the CMake
build for Hercules ensure that the Zlib library is built in a manner
consistent with the options and target for which Hercules is being
built.

- `BUILD_TESTING  ON | OFF`, default is `OFF`
    When `ON`, include the `example` and `example64` executables in the 
    build tree and generate test cases to verify the operation of Zlib
- `DEBUG ON | OFF`, default is `OFF`
    Applicable to single-configuration generators like Makefiles and
    Ninja.  When ON, generate a Debug configuration build script.
    When OFF, generate a Release script.  (The configuration used
    for multi-configuration generators like Microsoft Visual Studio
    or Apple Xcode is determined at build time.)
- `WINTARGET`  blank | `HOST` | `DIST` | windows-version
    Applicable when building on Windows.  Option windows-version may be
    any of `WinXP364`, `WinVista`, `Win7`, `Win8`, `Win10` and is case
    insensitive.  When blank or `HOST`, build for the version of Windows
    on the host.  When `DIST`, build for the earliest version of Windows
    supported by Hercules, Windows XP SP3 64-bit.  Otherwise build for
    the Windows version specified.
    
---
&nbsp;
### Differences from the original CMakeLists.txt

- Normally, only the zlib library is built, as a shared library
  (libhercz.so or libz1.dll).  The static library is not built, as
  Hercules has no need of it.

- On Windows, a DLL import library is also built.

- On Windows, warning C4267 "'var' : conversion from 'size_t' to 'type',
  possible loss of data is" suppressed to eliminate noise messages when
  Hercules builds Zlib.

- If the option BUILD\_TESTING is ON (-DBUILD_TESTING=ON), then the
  example and example64 executables are built and tests added for them.
  The executable is not included in the package installation.

- A WINTARGET option is added to allow generation for specific versions
  of the Windows API, with a range from WINXP364 to WIN10.

- The options to build assembly-language versions of selected routines
  are eliminated.  A single attempt to use these routines generated
  warnings that they were "use at your own risk," and a segment fault
  upon execution of the sample program.

- The entire unmodified Zlib distribution is stored in the pkg\_src
  directory, and this script reflects that.  This should allow a
  complete replacement the pkg_src directory contents when an uplevel
  upstream Zlib distribution becomes available.

- On UNIX-like systems, the shared library is not versioned.  There is
  no need, as this build of the package is intended to be used with
  exactly one application, Hercules, and versioning issues are expected
  to be addressed as part of updating pkg_src with new upstream code.

- The library installation target is exported to the zlib-targets
  sub-directory for inclusion by Hercules.

- The library target is also exported from the build tree to allow the
  Zlib library to be included in a Hercules build by referencing the 
  build directory.  If a Hercules builder elects to build this Zlib 
  package, there is no need to install it; the build tree target 
  can be imported.

- The two public headers are copied to the build tree so that the
  exported build tree target does not make any references to the source
  tree.

- For targets that support configurations (Windows and mac OS Xcode),
  only the Release and Debug configurations are supported.  The
  MinSizeRel and RelWithDebInfo configurations are removed if they are
  present in CMAKE_CONFIGURATION_TYPES.

- On Windows, for both Release and Debug configurations, linker .pdb
  files are created and included in the install of the library.
  
CMake 3.4 is the minimum version required to build Zlib because it is
the first version to include the boolean WINDOWS\_EXPORT\_ALL\_SYMBOLS,
which creates the exports file (.def) needed to build DLLs from source
that lacks specific `__declspec( dllexport )` declarations.

&nbsp;
&nbsp;

-----------

This README.md file Copyright © 2017 by Stephen R. Orso.

This work is licensed under the Creative Commons Attribution-ShareAlike
4.0 International License.

To view a copy of this license, visit http://creativecommons.org/licenses/by-sa/4.0/
or send a letter to Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.
