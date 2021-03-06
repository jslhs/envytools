=====================
envydis documentation
=====================

.. contents::

Using envydis
=============

``envydis`` reads from standard input and prints the disassembly to standard
output. By default, input is parsed as sequence space- or comma-separated
hexadecimal numbers representing the bytes to disassemble. The options are:

Input format
------------

.. option:: -w

  Instead of sequence of hexadecimal bytes, treat input as sequence of
  hexadecimal 32-bit words

.. option:: -W

  Instead of sequence of hexadecimal bytes, treat input as sequence of
  hexadecimal 64-bit words

.. option:: -i

  Treat input as pure binary

Input subranging
----------------

.. option:: -b <base>

  Assume the start of input to be at address <base> in code segment

.. option:: -s <skip>

  Skip/discard that many bytes of input before starting to read code

.. option:: -l <limit>

  Don't disassemble more than <limit> bytes.

Variant selection
-----------------

.. option:: -m <machine>

  Select the ISA to disassemble. One of:

  - [****] nv50: nv50 [tesla] CUDA/shader ISA
  - [*** ] nvc0: nvc0 [fermi] CUDA/shader ISA
  - [**  ] gk110: nv?? (tbd) [kepler GK110] CUDA/shader ISA
  - [**  ] ctx: nv40 and nv50 PGRAPH context-switching microcode
  - [*** ] falcon: falcon microcode, used to power various engines on nv98+ cards
  - [****] hwsq: PBUS hardware sequencer microcode
  - [****] xtensa: xtensa variant as used by video processor 2 [nv84-gen]
  - [*** ] vuc: video processor 2/3 master/mocomp microcode
  - [****] macro: nvc0 PGRAPH macro method ISA
  - [**  ] vp1: video processor 1 [nv41-gen] code
  - [****] vcomp: PVCOMP video compositor microcode

  Where the quality level is:

  - [    ]: Bare beginnings
  - [*   ]: Knows a few instructions
  - [**  ]: Knows enough instructions to write some simple code
  - [*** ]: Knows most instructions, enough to write advanced code
  - [****]: Knows all instructions, or very close to.

.. option:: -V <variant>

  Select variant of the ISA.

  For nv50:

  - nv50: The original NV50 [aka compute capability 1.0]
  - nv84: NV84, NV86, NV92, NV94, NV96, NV98 [aka compute capability 1.1]
  - nva0: NVA0 [aka compute capability 1.3]
  - nvaa: NVAA, NVAC [aka compute capability 1.2]
  - nva3: NVA3, NVA5, NVA8, NVAF [aka compute capability 1.2 + d3d10.1]

  For nvc0:

  - nvc0: NVC0:NVE4 cards
  - nve4: NVE4+ cards

  For ctx:

  - nv40: NV40:NV50 cards
  - nv50: NV50:NVA0 cards
  - nva0: NVA0:NVC0 cards

  For hwsq:

  - nv17: NV17:NV41 cards
  - nv41: NV41:NV50 cards
  - nv50: NV50:NVC0 cards

  For falcon:

  - fuc0: falcon version 0 [NV98, NVAA, NVAC]
  - fuc3: falcon version 3 [NVA3 and up]
  - fuc4: falcon version 4 [NVD9 and up, selected engines only]
  - fuc5: falcon version 4 [NVF0 and up, selected engines only]

  For vuc:

  - vp2: VP2 video processor [NV84:NV98, NVA0]
  - vp3: VP3 video processor [NV98, NVAA, NVAC]
  - vp4: VP4 video processor [NVA3:NVD9]

.. option:: -F <feature>

  Enable optional ISA feature. Most of these are auto-selected by :option:`-V`,
  but can also be specified manually. Can be used multiple times to enable
  several features.

  For nv50:

  - sm11: SM1.1 new opcodes [selected by nv84, nva0, nvaa, nva3]
  - sm12: SM1.2 new opcodes [selected by nva0, nvaa, nva3]
  - fp64: 64-bit floating point [selected by nva0]
  - d3d10_1: Direct3D 10.1 new features [selected by nva3]

  For nvc0:

  - nvc0op: NVC0:NVE4 exclusive opcodes [selected by nvc0]
  - nve4op: NVE4+ exclusive opcodes [selected by nve4]

  For ctx:

  - nv40op: NV40:NV50 exclusive opcodes [selected by nv40]
  - nv50op: NV50:NVC0 exclusive opcodes [selected by nv50, nva0]
  - callret: call/ret opcodes [selected by nva0]

  For hwsq:

  - nv17f: NV17:NV50 flags [selected by nv17, nv41]
  - nv41f: NV41:NV50 flags [selected by nv41]
  - nv41op: NV41 new opcodes [selected by nv41, nv50]

  For falcon:

  - fuc0op: falcon version 0 exclusive opcodes [selected by fuc0]
  - fuc3op: falcon version 3+ exclusive opcodes [selected by fuc3, fuc4]
  - pc24: 24-bit PC opcodes [selected by fuc4]
  - crypt: Cryptographic coprocessor opcodes [has to be manually selected]

  For vuc:

  - vp2op: VP2 exclusive opcodes [selected by vp2]
  - vp3op: VP3+ exclusive opcodes [selected by vp3, vp4]
  - vp4op: VP4 exclusive opcodes [selected by vp4]

.. option:: -O <mode>

  Select processor mode.

  For nv50:

  - vp: Vertex program
  - gp: Geometry program
  - fp: Fragment program
  - cp: Compute program

Output format
-------------

.. option:: -n

  Disable output coloring

.. option:: -q

  Disable printing address + opcodes.

