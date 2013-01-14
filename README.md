Ghost Trap - Ghostscript trapped in a sandbox
======

Ghost Trap is a modified distribution of the 
[GPL Ghostscript PDL interpreter](http://www.ghostscript.com/) secured and 
sandboxed using 
[Google Chrome sandbox technology](http://dev.chromium.org/developers/design-documents/sandbox).  
The objective of the project is to bring best-of-breed security to 
Ghostscript's command-line applications.

At a deep technical level Ghost Trap was designed with the help of 
[Peter Venkman, Egon Spengler, and Raymond Stantz](http://en.wikipedia.org/wiki/Ghostbusters). 
It's a portable Ecto Containment Unit which securely holds Ghostscripts in a 
laser containment field.

<a href="http://www.gbfans.com/equipment/ghost-trap/" 
    title="Ghostbusters Fan - used as a parody - love the '80's!">
    <img src="https://github.com/codedance/GhostTrap/raw/master/images/ghostbusters-ghost-trap-sized.jpg">
</a>


##Download

*Windows:* [ghost-trap-installer.exe](https://github.com/codedance/...)  (version 1.0)

##Motivation
PDL interpreters are large complex native code solutions. Adobe Reader is also a PDL viewer and as evident
by the number of security updates seen over the years, maintaining a secure solution in this space is a
difficult and ongoing exercise.  

Security is one of the most important goals at [PaperCut Software](http://www.papercut.com/).  Many
PaperCut users are large organizations printing sensitive data and have high security requirements.
In 2012 we started on a project to bring one of our most requested features, the ability to view and 
archive print jobs, into our print management software. To support viewing of PostScript jobs we needed to reply on a PostScript interpreter.

Rather than risk PaperCut users being susceptible to zero-day attacks we decided to take a more proactive approach and have used sandboxing.  This was made possible because both Ghostscript and Chromium are 
open source software projects. Ghost Trap brings Chromium's best of breed sandboxing security to 
Ghostscript.  Rendering print jobs into images is done within the sandbox.


##Usage

Install Ghost Trap (download link above).  Here are some examples using the example PostScript files
supplied with Ghostscript.

To convert the Escher PostScript example into a PNG image *WITHOUT* sandboxing:

    C:\Users\me> "C:\Program Files (x86)\GhostTrap\bin\gswin32c.exe" -q -dNOPAUSE -dBATCH -sDEVICE=png16m -sOutputFile=escher.png "C:\Program Files (x86)\GhostTrap\examples\escher.ps"


To convert th same Escher PostScript example into a PNG image *WITH* sandboxing:

    C:\Users\me> "C:\Program Files (x86)\GhostTrap\bin\gswin32c-trapped.exe" -q -dSAFER -dNOPAUSE -dBATCH -sDEVICE=png16m -sOutputFile=escher.png "C:\Program Files (x86)\GhostTrap\examples\escher.ps"

To convert a multi-page PDF file into a JPEG images *WITH* sandboxing:

    C:\Users\me> "C:\Program Files (x86)\GhostTrap\bin\gswin32c-trapped.exe" -q -dSAFER -dNOPAUSE -dBATCH -sDEVICE=jpeg "-sOutputFile=annots page %d.jpg" "C:\Program Files (x86)\GhostTrap\examples\annots.pdf"

```gswin32c-trapped.exe``` is the sandboxed version of ``gswin32c.exe``.  It should behave the same
was as standard Ghostscript console command as [documented](http://ghostscript.com/doc/9.06/Use.htm),
with the following known exceptions:

 * The input and output files must be local files.
 * Defining custom/extra FONT or LIB paths are not supported.

##How it works

```gswin32c-trapped.exe``` sets up a whitelist of resources required to perform the conversion.  It then 
execs a child process within a sandbox to perform the conversion. The whitelist of resources is dynamically constructed by determining the input file and output file/directory from the supplied command-line arguments. Ghostscript is provided access only to:

 * Read only access to the Windows Fonts directory.
 * Read only access to application-level registry keys (HKLM\Software\GPL Ghostscript).
 * Read only access to Ghostscript's lib folder resources.
 * Read only access to the input file.
 * Write access to the output directory.


##Future

The following future refinements are planned:

 * Sandbox other executable in the GhostPDL project (e.g ```pcl6.exe```).
 * Support custom FONT and LIB paths?

##Authors

Ghost Trap is open source software supported by [PaperCut Software](http://www.papercut.com/).

<img src="http://www.papercut.com/images/logo_papercut.png">

##Developers

If you with to build Ghost Trap form source, here is a brief flow:

 1. Clone the GIT repo
 1. Download Google Chromium source into the third-party directory as documented in
    [ghost-trap]/third-party/README.txt
 1. Download GhostPDL source into the third-party directory as documented in
    [ghost-trap]/third-party/README.txt
 1. Perform a 32bit Release compile on each dependency (follow the project's documentation). 
    Note, building Chromium is very involved! You will not need to compile whole Chromium source.
    Just the "sandbox" sub project will be enough to generate the required dependencies.
 1. Install [INNO setup](http://www.jrsoftware.org/isinfo.php)
 1. Run ```build.bat```

##License

    Copyright (c) 2011-2012 PaperCut Software Int. Pty. Ltd. http://www.papercut.com/

    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program.  If not, see <http://www.gnu.org/licenses/>.
    
