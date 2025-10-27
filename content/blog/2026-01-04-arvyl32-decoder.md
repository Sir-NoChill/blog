---
title: "ArVyl32 Decoder"
description: Cateloguing my progression on a veryl-based rv32i processor.
date: 2026-01-04
tags: [hardware, riscv]
---

I am using the [veryl](https://veryl-lang.org) language to design a
riscv-compliant chip. I hope (maybe) to submit to tiny tapeout later this year,
but we will have to see what I can get done.

## The Veryl Language

I am using veryl because the syntax of system verilog disgust me. Veryl
basically just adds in a nice development environment and some extra
type-checking into system verilog. It just makes the language nicer. It also
provides completely interoperable code generation with system verilog, meaning
that it can be directly integrated without hassle into any verilog project.

Some neat notes about what I discovered so far:
- Packed and unpacked arrays are
  [essentially the same thing](#packed-and-unpacked-arrays).
- [Structs](#structs) are nice and user-defined types in hardware. Are they
  zero-cost abstractions? I don't yet know.
- [Case statements](#case-statements) are clean and extensible
- Providing type-safe outputs means we don't need to remember how the structs
  are configured!

for the duration of this series I will be using 'verilog' and 'system-verilog'
to mean system verilog unless explicitly mentioned otherwise.

Begin rant.

I know some more experienced hardware people might think I'm a little bit weak
for needing these crutches to write hardware code, but I'll be the first to
admit that I am weak and in desperate need of development tools. I also think
that code should be comprehensible, which seems to go against the hardware ethos
(have you seen their code? Rediculous. I need to do a PhD to fix it). In other
words, hardware already needs to be brought into the modern age. Some projects
have been working on that, like [circt](https://circt.llvm.org), but we still
lack a common base of tools that allow for the low-level specification of a chip
while providing a nice development environment.

In truth, I'm not entirely sure how we even got this far with the current
hardware tools. I have friends who say it's important that hardware move
somewhat slower than software, and I agree, however look at all the rediculously
_bad_ hardware that exitsts? Why can't we make it better? Why can't people have
an approachable and _helpful_ development environment for it? Well, that's what
I'm trying to figure out by embarking on this quest.

Rant terminated.

### Packed and Unpacked Arrays <a id="packed-and-unpacked-arrays"></a>

So in system-verilog there are both [packed](https://www.chipverify.com/systemverilog/systemverilog-packed-arrays)
and [unpacked](https://www.chipverify.com/systemverilog/systemverilog-unpacked-arrays)
arrays. The essential difference is that there is no fundamental difference.
Packed arrays are guaranteed to be a sequence provided in order, and they can
only be made of other packable elements. Unpacked arrays can be made of
unpackable elements. That's all I can discern concretely for now.

With that, we can start the definition of the decoder to consume a 32-bit word
for our instruction.

```veryl
module DecodeUnit (
  clk: input clock,
  rst: input reset,
  instruction: input logic<32>,
  decoded_instruction_type: output RV32InstructionType,
  decoded_instruction: output RV32BaseInstructionType,
i) {
}
```

This looks a lot like system-verilog by design, since veryl is built to
transpile cleanly to system-verilog.

### Structs <a id="structs"></a>

So I defined a whole bunch of structs to refer to the various fields of a rv32
instruction. For those unfamiliar, I refer you to the [specification](https://docs.riscv.org/reference/isa/unpriv/rv32.html)
which contains all the necessary details.

```veryl

// Type definitions

/// R-Type definition as per the RISC-V Specification as found here:
/// https://docs.riscv.org/reference/isa/unpriv/rv32.html
struct RType {
  func7: logic<7>,
  rs2: logic<5>,
  rs1: logic<5>,
  func3: logic<3>,
  rd: logic<5>,
  opcode: logic<7>,
}

/// I-Type definition as per the RISC-V Specification as found here:
/// https://docs.riscv.org/reference/isa/unpriv/rv32.html
struct IType {
  imm: logic<12>,
  rs1: logic<5>,
  func3: logic<3>,
  rd: logic<5>,
  opcode: logic<7>,
}

/// S-Type definition as per the RISC-V Specification as found here:
/// https://docs.riscv.org/reference/isa/unpriv/rv32.html
struct SType {
  imm: logic<7>,
  rs2: logic<5>,
  rs1: logic<5>,
  func3: logic<3>,
  imm5: logic<5>,
  opcode: logic<7>,
}

/// B-Type definition as per the RISC-V Specification as found here:
/// https://docs.riscv.org/reference/isa/unpriv/rv32.html
struct BType {
  imm: logic<12>,
  rs2: logic<5>,
  rs1: logic<5>,
  func3: logic<3>,
  imm5: logic<5>,
  opcode: logic<7>,
}

/// U-Type definition as per the RISC-V Specification as found here:
/// https://docs.riscv.org/reference/isa/unpriv/rv32.html
struct UType {
  imm: logic<20>,
  rd: logic<5>,
  opcode: logic<7>,
}

/// J-Type definition as per the RISC-V Specification as found here:
/// https://docs.riscv.org/reference/isa/unpriv/rv32.html
struct JType {
  imm: logic<20>,
  rd: logic<5>,
  opcode: logic<7>,
}

/// Type union of all the instruction types for ease
union RV32BaseInstructionType {
  _raw: logic<32>,
  rtype: RType,
  itype: IType,
  stype: SType,
  btype: BType,
  utype: UType,
  jtype: JType,
}

function get_raw_opcode(
  instr: input RV32BaseInstructionType
) -> logic<7> {
  return instr._raw[6:0];
}

/// The  actual result from the decoder
enum RV32InstructionType {
  R,
  I,
  S,
  B,
  U,
  J,
  Error,
}

```

I always keep an error state available so that we can cleanly abort on an
unknown instruction. Not sure if that is standard or even advisable, but I'm not
currently looking to care...


### Case Statements <a id="case-statements"></a>

Dang, these are nice in veryl. You can define your binary masks with wildcards
so that you can match on a range of values.

```veryl
always_comb {
  // Recall that x in cases will match any digit, in this case binary
  decoded_instruction_type = case opcode {
    7'b0x1_0111: RV32InstructionType::U,
    //...
    default: RV32InstructionType::Error,
  };
}
```

I'm currently doing this in a comb block because I don't know how to pipeline it
in verilog yet. Need to do that at some point...

## Actually building the thing

So that's what I have for the decode unit so far. I need to finish implementing
all of the instruction opcodes (or do I?) and then getting that information to
the ALU. The fetch unit seems deceptively easy to build and the writeback/mem
stages are all... almost trivial? But maybe hardware design isn't as jank as I
thought it was.

I need to take a closer look at the generated code, but we will start with the
rv32i base instruction set (minus ecall, ebreak, fence and pause). Then we can
move forward.

## Links

- [RISC-V Spec](https://docs.riscv.org/reference/isa/unpriv/rv32.html)
- [Veryl Language](https://veryl-lang.org)
- [Single Cycle RV32 Core](https://github.com/merldsu/RISCV_Single_Cycle_Core)
- [System Verilog Ref](https://www.chipverify.com/tutorials/systemverilog)
