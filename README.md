# BuildCache

This is a simple compiler accelerator that caches and reuses build results to
avoid unnecessary re-compilations, and thereby speeding up the build process.

It is similar in spirit to [ccache](https://ccache.samba.org/).

## Usage

To use BuildCache for your builds, simply prefix the build command with
`buildcache`. For instance:

```bash
$ buildcache g++ -O2 hello.cpp -o hello.o
```

A convenient solution for bigger CMake-based projects is to use the
`RULE_LAUNCH_COMPILE` property to use BuildCache for all compilation commands,
like so:

```cmake
find_program(buildcache_program buildcache)
if(buildcache_program)
  set_property(GLOBAL PROPERTY RULE_LAUNCH_COMPILE "${buildcache_program}")
endif()
```

## Using with icecream

[icecream](https://github.com/icecc/icecream) (or ICECC) is a tool for
distributed compilation. To use icecream you can set the environment variable
`BUILDCACHE_PREFIX` to the icecc executable, e.g:

```bash
$ BUILDCACHE_PREFIX=/usr/bin/icecc buildcache g++ -O2 hello.cpp -o hello.o
```

## Supported compilers and languages

Currently only C and C++ via GCC and Clang are supported. New backends are
relatively easy to add though.

## Status

**NOTE:** BuildCache is still in early development and should not be considered
ready for production projects yet!

There are many missing features, such as cache housekeeping (the cache
currently grows indefinitely and needs to be cleared manually).

