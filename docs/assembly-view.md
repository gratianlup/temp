#### Overview

The Assembly view shows the function's machine code after disassembly, with syntax highlighting for x86_64/ARM64 architectures. The view is interactive, with the text being parsed into instructions with operands and higher-level constructs such as [basic blocks](https://en.wikipedia.org/wiki/Basic_block) and loops.

The assembly instructions are augmented with annotations from the debug information files, such as source line numbers and inlinees, and combined with the execution time from the profile trace.

The view has four parts:  

- a main toolbar at the top, with general action buttons.
- a secondary toolbar underneath with profiling-specific info and action buttons.
- the text view with the function's assembly
- several columns on the right side with the profiling data. If CPU performance counters are found and loaded from the trace, the additional columns with metrics and the counters are appended after the last column.  

[![Profiling UI screenshot](img/assembly-view_1164x473.png)](img/assembly-view_1164x473.png){:target="_blank"}

#### Assembly view

The function assembly area can be considere a read-only code editor, as such it follows the usual conventions. Each line corresponds to one instruction, with the following values left to right:

- instruction number (line number in the text)
- instruction virtual address (blue text)
- instruction opcode (bold text)
- a list of instruction operands
- source line number associated with the instruction from the debug info (gray text).
- inlinees (inlined functions) associated with the instruction from the debug info (green text).

##### Source lines

The debug information files usually contain a mapping between the source line numbers and the instructions that were generated for those lines. If avaliable, the source line number is appended at the end of an instruction. Note that accuracy of this mapping usually depends on the compiler optimization level, with higher levels being less accurate or even lacking the mapping for some instructions.

Hovering over the source line shows a tooltip with the name and path of the source file. To copy this info to clipboard, first *click* the source line, then press *Ctrl+C*.

##### Inlinees

During function inlining, the compiler may preserve additional details about the code being inlined so that the origin of an instruction can be saved into the debug information file. If avaliable, the inlinees are appended after the source line number.

For example, if the function contains a call chain like foo() -> bar(), with both calls being inlined, the final instructions will record the fact that they originate from bar(), which got inlined into foo(), then foo() into the function.

Hovering over the inlinee shows a tooltip with the call path (stack trace) of the functions inlined at that point. To copy this info to clipboard, first *click* the inlinee, then press *Ctrl+C*. Example:  

```
Inlined functions
name:line (file) in calling order:

std::uniform_int<int>::operator():2031 (random)
std::uniform_int<int>::_Eval:2077 (random)
std::_Rng_from_urng_v2<unsigned int,std::mersenne_twister_engine<unsigned int,32..:1866 (random)
```

##### Basic blocks

The assembly is parsed and analyzed to recover the function's [control-flow graph (CFG)](https://en.wikipedia.org/wiki/Control-flow_graph), identifying [basic blocks](https://en.wikipedia.org/wiki/Basic_block) and loops. This information is used to split the aseembly text into blocks and for the Flow Graph view.



##### Debug and profiling annotations

- basic blocks, collapse, time annotation
- jump targets
- line numbers, inlinees
- selection, time in status bar
- tooltip over tab name
- click on instr selects line in source panel, block in flow graph

MULTIPLE SELECTION IN FLAME GRAPH, SOURCE FILE and other lists also shows time in status bar!

- sync with source line
- sync with flow graph

- profiling marking and columns, extra for perf counters
- jump to hottest instr by default

- toolbar
- mouse, keyboard shortcuts
- profiling toolbar
  - jump to hottest
  - elements
  - blocks
  - inlinees
  - instances
  - threads

- back button, history, back/next mouse buttons
- call target arrow markings
- call target double-click

Exporting examples like in summary 3

TODO later:
- options panel