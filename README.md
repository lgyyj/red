Red Programming Language
------------------------
**Red** is a new programming language strongly inspired by [REBOL](http://rebol.com), but with a broader field of usage thanks to its native-code compiler, from system programming to high-level scripting, while providing modern support for concurrency and multi-core CPUs.

The language is in its early bootstrap phase, once complete it will be self-hosted. The Red software stack also contains another language, **Red/System**, which is a low-level dialect of Red. It is a limited C-level language with a Red look'n feel, required to build Red's runtime library and be the target language of Red's compiler. More information at [red-lang.org](http://www.red-lang.org).


Running the Red hello script
------------------------
The compiler and linker are currently written in REBOL and can produce Windows, Linux, Syllable, Android and Mac OS X executables. So, for now, a REBOL/Core binary is required to compile Red and Red/System programs. Please follow the instructions for installing the compiler tool-chain:

1. Clone this git repository or download an archive (`ZIP` button above or from [tagged packages](https://github.com/dockimbel/Red/tags)).

1. Download a REBOL interpreter suitable for your OS: [Windows](http://www.rebol.com/downloads/v278/rebol-core-278-3-1.exe), [Linux](http://www.rebol.com/downloads/v278/rebol-core-278-4-2.tar.gz), [Mac OS X](http://www.rebol.com/downloads/v278/rebol-core-278-2-5.tar.gz), [FreeBSD](http://www.rebol.com/downloads/v278/rebol-core-278-7-2.tar.gz), [OpenBSD](http://www.rebol.com/downloads/v278/rebol-core-278-9-4.tar.gz), [Solaris](http://www.rebol.com/downloads/v276/rebol-core-276-10-1.gz)

1. Extract the `rebol` binary, put it in root folder, that's all!

1. Let's test it: run `./rebol`, you'll see a `>>` prompt appear. Windows users need to double-click on the `rebol.exe` file to run it.

1. From the REBOL console type:

    `do/args %red.r "%red/tests/hello.red"`

The compilation process should finish with a `...output file size` message. The resulting binary is in the working folder. Windows users need to open a DOS console and run `hello.exe` from there.

2. To see the intermediary Red/System code generated by the compiler, use:

    `do/args %red.r "-v 1 %red/tests/hello.red"`

Command-line options:

    -d			: switches into debug mode.
    
    -o <file>	: outputs executable to given path and/or filename.
    
    -t <target>	: cross-compiles to another target (see table below).
    
    -v <level>	: sets verbose mode. 1-3 are for Red only, above for Red/System.
    

*Important*: Red will be distributed as a binary to end users and the effect of:

    red script.red
    
will be to compile and run it from memory directly. So the -o option will become mandatory in the future for generating an executable without running it. During the bootstrap stage, it is complex to support that feature, but anyway, we will implement it as soon as possible.

Running the Red REPL
-----------------------

1. From the REBOL console type:

    `do/args %red.r "%red/tests/console.red"`
    
1. From Unix, just type `console` from prompt, Windows users need to open a DOS console and run `console.exe` from there, or just click on the executable from Explorer (but you will miss the error messages then). Once the console launched, you should see a banner and a prompt:

    -=== Red Console alpha version ===-
    (only Latin-1 input supported)
    
    red>>

1. You can use it to test rapidly some Red code:

    red>> 1 + 2
    == 3
    
    red>> inc: func [n][n + 1]
    == func [n][n + 1]
    red>> inc 123
    == 124

Running the Red/System hello script
------------------------

1. From the REBOL console type:

    `change-dir %red-system/`

1. Type: `do/args %rsc.r "%tests/hello.reds"`, the compilation process should finish with a `...output file size` message.

1. The resulting binary is in `red-system/builds/`, go try it! Windows users need to open a DOS console and run `hello.exe` from there.


The `%rsc.r` script is only a wrapper script around the compiler, for testing purpose. It accepts a `-v <integer!>` option for verbose logs. Try it with:

    >> do/args %rsc.r "-v 5 %tests/hello.reds"

Cross-compilation support
-------------------------

Cross-compilation is easily achieved by using a `-t` command line option followed by a target ID. This command-line option works for both Red and Red/System compilers.

Currently supported targets are:

<div align="center">
<table>
	<tr><th align="left" style="background-color: #DDD;">Target ID</th><th align="left" style="background-color: #DDD;">Description</th></tr>
	<tr><td><b>MSDOS</b></td><td>Windows, x86, console-only applications</td></tr>
	<tr><td><b>Windows</b></td><td>Windows, x86, native applications</td></tr>
	<tr><td><b>Linux</b></td><td>GNU/Linux, x86</td></tr>
	<tr><td><b>Linux-ARM</b></td><td>GNU/Linux, ARMv5</td></tr>
	<tr><td><b>Darwin</b></td><td>Mac OS X Intel, console-only applications</td></tr>
	<tr><td><b>Syllable</b></td><td><a href="http://web.syllable.org/pages/index.html">Syllable 
	OS</a>, x86 </td></tr>
	<tr><td><b>Android</b></td><td>Android, ARMv5</td></tr>
</table>
</div>
<br/>

For example, from Windows, to emit Linux executables:

    >> do/args %rsc.r "-t Linux %tests/hello.reds"

From Linux, to emit Windows console executables:

    >> do/args %rsc.r "-t MSDOS %tests/hello.reds"
    
Anti-virus false positive
-------------------------
Some anti-virus programs are a bit too sensitive and can wrongly report an alert on some binaries generated by Red, just ignore those alerts and if possible, remove the Red folder path from the anti-virus scanner. We will try to workaround that in future releases. The badly reacting anti-virus apps are: Avira, AntiVir, BitDefender, F-Secure, F-Prot, DrWeb, Commtouch, Panda.

License
-------------------------
Both Red and Red/System are published under [BSD](http://www.opensource.org/licenses/bsd-3-clause) license, runtime is under [BSL](http://www.boost.org/users/license.html) license. BSL is a bit more permissive license than BSD, more suitable for the runtime parts.
