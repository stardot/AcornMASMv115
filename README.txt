Introduction
============

This repository contains the original source code for the Acorn MASM
Assembler, version 1.15, which was part of Acorn's 6502 Development
Package.

The structure of the repository is as follows:

    original_sources.zip
              - a copy of the original MASM source files, with the
                original <cr> line endings.

    adfs/     - disk images in ADFS (adl and dat) formats;
                these are generated by the make_disk_images.sh script
                which uses COEUS's AcornFsUtils package.

    src/      - the MASM source files

    tools/    - binaries for MASM (both DFS and non-DFS versions)

Assembling the MASM Source Code
===============================

An Acorn Turbo (256K) 6502 Co Processor is needed to assemble the MASM
sources. As originals are exceedingly rare, an alternative is the
emulated version provided by PiTubeDirect Fer-De-Lance (and later) as
Co Pro 17. The reset banner should say: "Acorn TUBE 6502 256K".

The source code is in Acorn MASM format, and a copy of the "Turbo"
version of MASM (called TurMASM) is included in the disk images.

Using ADFS:
===========

The disk organization on ADFS involves a single disk image containing
the tools (TurMASM), build scripts, source code, and sufficient free
space for the build process.

In the adfs/ directory are the following versions:

   AcornMASMv115.adl - this is a 640KB interleaved disk image,
   suitable for writing to a floppy disk.

   AcornMASMv115.dat - this is a 640KB non-interleaved disk image,
   suitable for using with BeebSCSI (e.g. rename it to scsi0.dat).

Transfer one of these disk images onto your physical hardware.

To assemble the sources, you just need to boot the disk (it contains
an appropriate !BOOT file)

Three binary files are generated in the root directory:

The Normal version of MASM for the Normal 64K 6502 Co Processor,
called NewMasm:
    (this has a MD5SUM of 58cac885421b5632841f13389a44418a)

A support file for the normal version of MASM that is loaded on the IO
Processor, call NewIoMasm:
    (this has a MD5SUM of dac3ac252f7a1bd4dffb3716d3a37d4e)

The Turbo version of MASM for the Turbo 256K 6502 Co Processor,
called NewTurMasm:
    (this has a MD5SUM of d78f37940d8e40f630531c7ff74b8119)

Known Issues:
=============

1. The MD5SUM for NewTurMasm may not be constant, as the build script
does not pad the binary with 00 bytes.

2. It should be possible to switch the build to Normal MASM, thus not
requiring the Turbo Co Pro, as all sources are < 17KB.

3. Currently the DFS versions are not built.

Notes:
======

The default is to build the ADFS versions of MASM.

It's possible to build a DFS version by changing the FS define to DFS:

; The filing system numbers
DFS     * 0   ; The normal DFS
OTHER   * 1   ; NFS and WDFS
FS      * DFS ; The currently selected filing system <<<<

This adds a small about of extra code to implement the < and >
directives (these are described in the MASM documentation).

Unfortunately with v115 of normal Masm, if you try to build the DFS
version, it exceeds the available code space by 2 bytes (ending at
&F802), and you get a "code too long" error during assembly.

As a work around, try shortening one of the error messages.

e.g. in MASM 44 change:
 = "Macro definition table full"
to
 = "Macro table full"

Acknowledgements:
=================

Many thanks to Stuart Swales and Paul Fellows (ex Acornsoft) for
discovering these long-lost sources and making them available to the
Acorn community.
