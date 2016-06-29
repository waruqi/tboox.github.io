---
layout: page
---

### Languages

* [中文](/cn)

# The TBOOX Open Source Projects

* [TBOX](#tbox): The Treasure Box Library
* [GBOX](#gbox): The Graphic Box Library
* [XMake](#xmake): The Automatic Cross-platform Build Tool

<div id="tbox"></div>

# The Treasure Box Library (TBOX)

## Introduction

TBOX is a glib-like cross-platform C library that is simple to use yet powerful in nature.

The project focuses on making C development easier and provides many modules (.e.g stream, asio, regex, container, algorithm ...), 
so that any developer can quickly pick it up and enjoy the productivity boost when developing in C language.

Supports the following platforms:

- Windows
- Macosx
- Linux
- Android
- iOS

If you want to know more, please refer to:

* [Documents](https://github.com/waruqi/tbox/wiki/documents)
* [Github](https://github.com/waruqi/tbox)

## Features

#### The stream library
- Supports file, data, http and socket source
- Supports the stream filter for gzip, charset and...
- Implements transfer for two stream
- Implements transfer pool (asio) for multi-stream
- Implements the static buffer stream for parsing data

#### The asynchronous io library
- Supports reactor and proactor mode
- Uses epoll, poll, select, kqueue and iocp os system api

#### The database library
- Supports mysql and sqlite3 database and enumerates data using the iterator mode

#### The xml parser library
- Supports DOM and SAX mode and Supports xpath

#### The serialization and deserialization library. 
- Supports xml, json, bplist, xplist, binary formats

#### The memory library
- Implements some memory pools for optimizing memory
- Supports fast memory error detecting. it can detect the following types of bugs for the debug mode:
  - out-of-bounds accesses to heap and globals
  - use-after-free
  - double-free, invalid free
  - memory leaks

#### The container library
- Implements hash table, single list, double list, vector, stack, queue
  and min/max heap. Supports iterator mode for algorithm

#### The algorithm library
- Uses the iterator mode
- Implements find, binary find and reverse find algorithm
- Implements sort, bubble sort, quick sort, heap sort and insert sort algorithm
- Implements count, walk items, reverse walk items, for_all and rfor_all

#### The network library
- Implements dns(cached), ssl(openssl and polarssl), http and cookies
- Supports asynchronous io mode for dns, ssl and http using the asio and stream library

#### The platform library
- Implements timer, fast and low precision timer
- Implements atomic and atomic64 operation
- Implements spinlock, mutex, event, semaphore, thread and thread pool 
- Implements file, socket operation

#### The charset library
- Supports utf8, utf16, gbk, gb2312, uc2 and uc4
- Supports big endian and little endian mode

#### The zip library
- Supports gzip, zlibraw, zlib formats using the zlib library if exists
- Implements lzsw, lz77 and rlc algorithm

#### The utils library
- Implements base32, base64 encoder and decoder
- Implements crc32, adler32, md5 and sha1 hash algorithm
- Implements assert and trace output for the debug mode
- Implements bits operation for parsing u8, u16, u32, u64 data

#### The math library
- Implements random generator
- Implements fast fixed-point calculation, Supports 6-bits, 16-bits, 30-bits fixed-point number

#### The libc library
- Implements lightweight libc library interfaces, the interface name contains `tb_xxx` prefix for avoiding conflict
- Implements strixxx strrxxx wcsixxx wcsrxxx interface extension
- optimizes some frequently-used interface, .e.g. memset, memcpy, strcpy ... 
- Implements `memset_u16`, `memset_u32`, `memset_u64` extension interfaces

#### The libm library
- Implements lightweight libm library interfaces, the interface name contains `tb_xxx` prefix for avoiding conflict
- Supports float and double type

#### The regex library
- Supports match and replace
- Supports global/multiline/caseless mode
- uses pcre, pcre2 and posix regex modules

## In the Plans
- Add coroutine module 
- Add server module
- Reconstruction xml module
- Add more algorithms and container
- Optimizes more libc interfaces 
- Implements more libm interfaces and not wrap it only

## Projects

Some projects using tbox:

* [gbox](https://github.com/waruqi/gbox)
* [itrace](https://github.com/waruqi/itrace)
* [more](https://github.com/waruqi/tbox/wiki/tbox-projects)

## Build

Please install xmake first: [xmake](https://github.com/waruqi/xmake)

    # build for the host platform
    cd ./tbox
    xmake

    # build for the mingw platform
    cd ./tbox
    xmake f -p mingw --sdk=/home/mingwsdk 
    xmake
    
    # build for the iphoneos platform
    cd ./tbox
    xmake f -p iphoneos 
    xmake
    
    # build for the android platform
    cd ./tbox
    xmake f -p android --ndk=xxxxx
    xmake
    
    # build for the linux cross-platform
    cd ./tbox
    xmake f -p linux --sdk=/home/sdk # --toolchains=/home/sdk/bin
    xmake

    
## Example

    #include "tbox/tbox.h"

    int main(int argc, char** argv)
    {
        // init tbox
        if (!tb_init(tb_null, tb_null)) return 0;

        // trace
        tb_trace_i("hello tbox");

        // init vector
        tb_vector_ref_t vector = tb_vector_init(0, tb_element_cstr(tb_true));
        if (vector)
        {
            // insert item
            tb_vector_insert_tail(vector, "hello");
            tb_vector_insert_tail(vector, "tbox");

            // dump all items
            tb_for_all (tb_char_t const*, cstr, vector)
            {
                // trace
                tb_trace_i("%s", cstr);
            }

            // exit vector
            tb_vector_exit(vector);
        }

        // init stream
        tb_stream_ref_t stream = tb_stream_init_from_url("http://www.xxx.com/file.txt");
        if (stream)
        {
            // open stream
            if (tb_stream_open(stream))
            {
                // read line
                tb_long_t size = 0;
                tb_char_t line[TB_STREAM_BLOCK_MAXN];
                while ((size = tb_stream_bread_line(stream, line, sizeof(line))) >= 0)
                {
                    // trace
                    tb_trace_i("line: %s", line);
                }
            }

            // exit stream
            tb_stream_exit(stream);
        }

        // wait some time
        getchar();

        // exit tbox
        tb_exit();
        return 0;
    }


<div id="gbox"></div>

# The Graphic Box Library (GBOX)

gbox is a mutli-platform c graphic library. 

It is now in the early stages of development only for reference and learning.

If you are interesting to this project, please view the source code.

####Features

- Support 2d vector drawing
- Support matrix transform
- Support fill and stroke for solid, gradient and image
- Support custom path and drawing
- Support float and fixed point computation
- Provide custom drawing devices
- Support OpenGL ES 1.0/2.0 
- Support soft algorithm for drawing
- Implement mesh structure and algorithm
- Implement complex polygon tessellator and triangulation
- Support odd and nonzero fill
- Implement polygon scanning algorithm
- Provide multi-platform windows(.e.g glut, sdl and x11, framebuffer, ios, android in the feature)

####In the plans

- Draw svg images
- Implement anti-aliasing
- More drawing devices and windows
- Draw text 
- Development ui and game engine

####Build

please install xmake first: [xmake](https://github.com/waruqi/xmake)

    cd ./gbox
    xmake

####Documents

* [Documents](https://github.com/waruqi/gbox/wiki/documents)
* [Codes](https://github.com/waruqi/gbox)

####Contacts

- Email:   	    
	- waruqi@gmail.com
- Website: 	    
	- http://www.tboox.org
	- http://www.tboox.net

<div id="xmake"></div>

# The Automatic Cross-platform Build Tool (XMake)

## Introduction

xmake is a make-like build utility based on lua. 

The project focuses on making development and building easier and provides many features (.e.g package, install, plugin, macro, action, option, task ...), 
so that any developer can quickly pick it up and enjoy the productivity boost when developing and building project.

####Features

- Create projects and supports many project templates
- Support c/c++, objc/c++, swift and assembly language
- Automatically probe the host environment and configure project 
- Provide some built-in actions (config, build, package, clean, install, uninstall and run)
- Provide some built-in plugins (doxygen, macro, project) 
- Provide some built-in macros (batch packaging)
- Describe the project file using lua script, more flexible and simple
- Custom packages, platforms, plugins, templates, tasks, macros, options and actions
- Do not generate makefile and build project directly
- Support multitasking with argument: -j 

####Actions

- config: Configure project before building. 
- global: Configure the global options for xmake.
- build: Build project.
- clean: Remove all binary and temporary files.
- create: Create a new project using template.
- package: Package the given target
- install: Install the project binary files.
- uninstall: Uninstall the project binary files.
- run: Run the project target.

####Plugins

- The doxygen plugin: Make doxygen document from source codes
- The macro plugin: Record and playback commands 
- The hello plugin: A simple plugin demo to show 'hello xmake!'
- The project plugin: Create the project file for IDE (.e.g makefile and vs, xcode in the feature ...)

####Languages

- C/C++
- Objc/Objc++
- Swift
- Assembly

####Platforms

- Windows (x86, x64, amd64, x86_amd64)
- Macosx (i386, x86_64)
- Linux (i386, x86_64, cross-toolchains ...)
- Android (armv5te, armv6, armv7-a, armv8-a, arm64-v8a)
- iPhoneos (armv7, armv7s, arm64, i386, x86_64)
- Watchos (armv7k, i386)
- Mingw (i386, x86_64)

####In the plans

- Manage package and dependence
- Download package automaticlly
- Create package repository for porting other third-party source codes, it's goal is that one people port it and many people shared.
- Implement more plugins
- Create more project file for IDE (.e.g vs, xcode ..)

####Examples

Create a c++ console project:

        xmake create -l c++ -t 1 console
     or xmake create --language=c++ --template=1 console

Project xmakefile: xmake.lua

    target("console")
        set_kind("binary")
        add_files("src/*.c") 

Configure project:

   This is optional, if you compile the targets only for linux, macosx and windows and the default compilation mode is release.

       xmake f -p iphoneos -m debug
    or xmake f --plat=macosx --arch=x86_64
    or xmake f -p windows
    or xmake config --plat=iphoneos --mode=debug
    or xmake config --plat=android --arch=armv7-a --ndk=xxxxx
    or xmake config -p linux -a i386
    or xmake config -p mingw --cross=i386-mingw32- --toolchains=/xxx/bin
    or xmake config -p mingw --sdk=/mingwsdk
    or xmake config --help

Compile project：
 
       xmake
    or xmake -r
    or xmake --rebuild

Run target：

       xmake r console
    or xmake run console

Package all：

       xmake p
    or xmake package
    or xmake package console
    or xmake package -o /tmp
    or xmake package --output=/tmp

Package all archs using macro:
       
       xmake m package 
    or xmake m package -p iphoneos
    or xmake m package -p macosx -f "-m debug" -o /tmp/
    or xmake m package --help

Install targets：

       xmake i
    or xmake install
    or xmake install console
    or xmake install -o /tmp
    or xmake install --output=/tmp

If you need known more detailed usage，please refer to [documents](https://github.com/waruqi/xmake/wiki/documents)
or run:

       xmake -h
    or xmake --help
    or xmake config --help
    or xmake package --help
    or xmake macro --help
    ...

The simple xmake.lua file:

    -- the debug mode
    if is_mode("debug") then
        
        -- enable the debug symbols
        set_symbols("debug")

        -- disable optimization
        set_optimize("none")
    end

    -- the release mode
    if is_mode("release") then

        -- set the symbols visibility: hidden
        set_symbols("hidden")

        -- enable fastest optimization
        set_optimize("fastest")

        -- strip all symbols
        set_strip("all")
    end

    -- add target
    target("test")

        -- set kind
        set_kind("static")

        -- add files
        add_files("src/*.c") 
