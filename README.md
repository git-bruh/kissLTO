# kissLTO

A KISS repository for building with -O3, Graphite, and LTO optimizations, the KISS version of [gentooLTO](https://github.com/InBetweenNames/gentooLTO). This is a continuation of the [`kiss_optim` repo](https://git.ebc.li/archive/kiss_optim/) by [admicos](https://github.com/Admicos).

## Usage

* Add this repository to `KISS_PATH` such that it takes precedence over other repos:

`export KISS_PATH="/path/to/kissLTO/repo:$KISS_PATH"`

* Create a symlink at the top level of this repository pointing to the official KISS repositories:
```sh
cd /path/to/kissLTO
ln -sf /path/to/main-repo ./kiss-repo
```

* (**IMPORTANT**) Set a `KISS_HOOK` to automatically work around build failures in packages:

`export KISS_HOOK=/path/to/kissLTO/kiss-hook`

* Modify build flags:
```sh
export CFLAGS="-O3 -pipe -march=native -mtune=native -fno-math-errno -fdevirtualize-at-ltrans -fno-semantic-interposition -fipa-pta -flto=auto -fuse-linker-plugin"
export CXXFLAGS="$CFLAGS"
export LDFLAGS="-Wl,-O1 -Wl,--as-needed $CFLAGS"
```

* Re-build `gcc` and enable Graphite optimizations:

`CFLAGS="$CFLAGS -fgraphite-identity -floop-nest-optimize"`

* Re-build `binutils` and enable the `gold` linker:

`CFLAGS="$CFLAGS -fuse-ld=gold"`

* Do a full system rebuild to allow all packages to pick up the new optimizations.

## Packages

Some packages are forked to this repository for further optimizations:

* `binutils`: Forked to enable the `gold` linker.

* `ffmpeg`, `x264`: Forked for build fixes when using `LTO`.

* `firefox`, `gcc`, `python`, `zstd`: Built with `PGO`.
