# Compilation & Installation of the Project

> Note that we focuses on doing this on macOS (for now).

As stated in README.md of Liberation Circuit (from now will refer to it as "the project") in section _Compiling on macOS_ what we need to do is as follows

* `git clone https://github.com/linleyh/liberation-circuit.git`
* `cd liberation-circuit`
* `brew install allegro`
* `./do`
* `cd bin`
* `libcirc`

## Allegro 5

Just a sneak peek into what shared libraries Allegro 5 just installed on your system which can be listed by executed `ls -1 /usr/local/lib/liballegro*`

```
/usr/local/lib/liballegro.5.2.4.dylib
/usr/local/lib/liballegro_acodec.5.2.4.dylib
/usr/local/lib/liballegro_audio.5.2.4.dylib
/usr/local/lib/liballegro_color.5.2.4.dylib
/usr/local/lib/liballegro_dialog.5.2.4.dylib
/usr/local/lib/liballegro_font.5.2.4.dylib
/usr/local/lib/liballegro_image.5.2.4.dylib
/usr/local/lib/liballegro_main.5.2.4.dylib
/usr/local/lib/liballegro_memfile.5.2.4.dylib
/usr/local/lib/liballegro_physfs.5.2.4.dylib
/usr/local/lib/liballegro_primitives.5.2.4.dylib
/usr/local/lib/liballegro_ttf.5.2.4.dylib
/usr/local/lib/liballegro_video.5.2.4.dylib
```

and as well via `ls -1 /usr/local/opt/allegro/lib/liballegro*`

```
/usr/local/opt/allegro/lib/liballegro.5.2.4.dylib
/usr/local/opt/allegro/lib/liballegro_acodec.5.2.4.dylib
/usr/local/opt/allegro/lib/liballegro_audio.5.2.4.dylib
/usr/local/opt/allegro/lib/liballegro_color.5.2.4.dylib
/usr/local/opt/allegro/lib/liballegro_dialog.5.2.4.dylib
/usr/local/opt/allegro/lib/liballegro_font.5.2.4.dylib
/usr/local/opt/allegro/lib/liballegro_image.5.2.4.dylib
/usr/local/opt/allegro/lib/liballegro_main.5.2.4.dylib
/usr/local/opt/allegro/lib/liballegro_memfile.5.2.4.dylib
/usr/local/opt/allegro/lib/liballegro_physfs.5.2.4.dylib
/usr/local/opt/allegro/lib/liballegro_primitives.5.2.4.dylib
/usr/local/opt/allegro/lib/liballegro_ttf.5.2.4.dylib
/usr/local/opt/allegro/lib/liballegro_video.5.2.4.dylib
```

I stripped out redundant `.dylib` with the same name left with only with name + version for clarity.

Anyway, if you successfully compile the project, and try to peek which shared library the final binary depends on via `otool -L bin/libcirc` you will see the following output

```
libcirc:
	/usr/local/opt/allegro/lib/liballegro.5.2.dylib (compatibility version 5.2.0, current version 5.2.4)
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1252.50.4)
	/usr/local/opt/allegro/lib/liballegro_audio.5.2.dylib (compatibility version 5.2.0, current version 5.2.4)
	/usr/local/opt/allegro/lib/liballegro_acodec.5.2.dylib (compatibility version 5.2.0, current version 5.2.4)
	/usr/local/opt/allegro/lib/liballegro_font.5.2.dylib (compatibility version 5.2.0, current version 5.2.4)
	/usr/local/opt/allegro/lib/liballegro_image.5.2.dylib (compatibility version 5.2.0, current version 5.2.4)
	/usr/local/opt/allegro/lib/liballegro_primitives.5.2.dylib (compatibility version 5.2.0, current version 5.2.4)
	/usr/local/opt/allegro/lib/liballegro_dialog.5.2.dylib (compatibility version 5.2.0, current version 5.2.4)
	/usr/local/opt/allegro/lib/liballegro_main.5.2.dylib (compatibility version 5.2.0, current version 5.2.4)
```

Clearly the project linked with the following sub-system (called "Addons") of Allegro 5

1. [Audio](https://liballeg.org/a5docs/trunk/audio.html)
2. [Acodec](https://liballeg.org/a5docs/trunk/acodec.html)
3. [Font](https://liballeg.org/a5docs/trunk/font.html)
4. [Image](https://liballeg.org/a5docs/trunk/image.html)
5. [Primitives](https://liballeg.org/a5docs/trunk/primitives.html)
6. [Dialog](https://liballeg.org/a5docs/trunk/native_dialog.html) - involve around native UI and dialog as used in the game. It's based native implementation on Windows, X (via GTK), and macOS.
7. [Main](https://liballeg.org/a5docs/trunk/main.html) - strangely enough this sub-system has no public API but needed by macOS to make entry point of the project works. As stated in the link provided, it sasys something about name mangling in which I presume there's only a problem on macOS and it's needed for compilation on such platform only.

## `do` script

`do` is a shell script with customizied code to compile C project. It will always compile all source files even certain files are already compiled. As stated in [README.md](https://github.com/linleyh/liberation-circuit) of the project. To quickly re-compile only changes made, use `redo` instead.

`redo` can be obtained via [here](http://news.dieweltistgarnichtso.net/bin/redo-sh.html) (shell script version) or [here](http://jdebp.eu./Softwares/redo/) (in C++).

## Under the hood

It is somewhat seems like a magic when you execute `./do` as it seems magically generate output objects and executable in `bin/` directory. But if you carefully look at the `do` script, it has some secret sauce.

Its purpose is to generated a linked executable command (not grab from Internet) in `.do_built.dir/` then continue with the sequence in `do` script.

> Interesting to know that `do` script doesn't try to grab any binary commands from Internet as might be initially thought. In fact those commands are soft-linked to things like `/usr/bin/true` and event `do` script itself.

Yes, you could check this yourself after executing `./do` then you can execute `ls -la`. It will list the following output.

```
lrwxr-xr-x  1 haxpor  staff  41 Aug  2 17:00 redo -> /Users/haxpor/scrap/liberation-circuit/do
lrwxr-xr-x  1 haxpor  staff  13 Aug  2 17:00 redo-always -> /usr/bin/true
lrwxr-xr-x  1 haxpor  staff  41 Aug  2 17:00 redo-ifchange -> /Users/haxpor/scrap/liberation-circuit/do
lrwxr-xr-x  1 haxpor  staff  13 Aug  2 17:00 redo-ifcreate -> /usr/bin/true
lrwxr-xr-x  1 haxpor  staff  13 Aug  2 17:00 redo-stamp -> /usr/bin/true
```

Thus we can see that its naming just to accomodate meaning when using in `do` script. We already had all additional tools to compile the project.

Also note that `Makefile`, `CMakeLists.txt`l, and `CMake` are not used in compilation steps on macOS.

As well, interesting to know there are `*.do` files scattering around the directory as seen as following

* `src/default.o.do`
* `src/g_game.do`
* `src/m_main.c`

## Compilation Quirks

The project used `gcc`'s option flags of

* `-fno-strict-overflow` - but use `-fwrapv` with `do` script for macOS
* `-ffast-math` - but use `-fwrapv` with `do` script for macOS
* `-O3`
* `-MD -MF <file>` - to output dependencies as `*.deps` file when start to compile

All of above is for optimization. For detail of gcc's compilation options can be seen [here](https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html).
