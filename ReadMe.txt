FLExBridge is an add-on to FieldWorks (http://fieldworks.sil.org; https://github.com/sillsdev/FwDocumentation/wiki)
that supports using Chorus (https://github.com/sillsdev/chorus) to allow multiple users to share data.

***Build notes:***
FLExBridge depends on several assemblies from Chorus and Palaso.
Versions of these assemblies are in the repo, but to avoid bloating it, they are usually not kept up to date.  Therefore,
to build it, you must usually get the latest versions of these DLLs and copy them to appropriate places in the FLExBridge project.

The required DLLs (that are likely to be out of date) are
 IPCFramework.dll, L10NSharp.dll, Chorus.exe, ChorusMerge.exe, LibChorus.dll, and Palaso.dll.
For a Windows debug build the Chorus and Palaso dependencies currently live in lib/Debug (release build: lib/Release,
Mono: lib/DebugMono); IPCFrmework and L10NSharp live in lib/common.  You can download these and various other projects from the
continuous build server at http://build.palaso.org/ or by using running buildupdate.sh.  If necessary, buildupdate.sh can be
updated using the tool at https://github.com/chrisvire/BuildUpdate.

If you plan to work on Chorus,
- clone the Chorus and LibPalaso repos from https://github.com/sillsdev/chorus and https://github.com/sillsdev/libpalaso into the
   same parent directory without changing their repository names
- run GetAndBuildThis.bat to: Download the latest commit on your branch of FLExBridge, GetAndBuild LibPalaso and Chorus
   recursively, copy dependencies from LibPalaso to Chorus to FLExBridge, and build FLExBridge

Special Mono dependencies:
$ cp ../libpalaso/lib/Debug/icu.net.dll* ../libpalaso/lib/DebugMono
$ PATH=/usr/bin:$PATH make [debug|release] #This will prefer the System Mono over fieldworks-mono

To run FLExBridge you must unzip chorus/lib/debug/Mercurial.zip to the root of FLExBridge.  Then, edit the mercurial.ini file
in the Mercurial folder. Add a line like this (with the appropriate path for your FLExBridge folder).
fixutf8 = C:\Dev\FLExBridge\MercurialExtensions\fixutf8\fixutf8.py

***Connecting FieldWorks to FLExBridge:***
Add the following keys to your registry (32-bit OS: omit 'Wow6432Node\', Mono: export env var FLEXBRIDGEDIR):
<code>
[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\SIL\Flex Bridge\8]
"InstallationDir"="C:\Dev\FLExBridge\output\Debug"
</code>
Also, if you are working on Chorus:
- Copy Chorus.exe, LibChorus.dll, and Palaso dll's to C:\fwrepo\fw\output\Debug (or Release).  You can do this using the
   UpdateFLExDependencies.bat script in the flexbridge repo
These steps are required for only those dependencies bound at compile time (e.g. API changes):
- Comment out the corresponding DownloadFiles tags in C:\fwrepo\fw\Build\mkall.targets
- Rebuild FLEx
