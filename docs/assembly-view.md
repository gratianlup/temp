#### Overview

The Assembly view shows the function's machine code after disassembly, with syntax highlighting for x86_64/ARM64 architectures. The view is interactive, with the text being parsed into instructions with operands and higher-level constructs such as [basic blocks](https://en.wikipedia.org/wiki/Basic_block) and loops are recovered.

The assembly instructions are augmented with annotations from the debug information files, such as source line numbers and inlinees, and combined with the execution time from the profile trace.

The Assembly view has four parts:  

- a main toolbar at the top, with general action buttons.
- a secondary toolbar underneath with profiling-specific info and action buttons.
- the text view with the function's assembly
- several columns on the right side with the profiling data. If CPU performance counters are found and loaded from the trace, the additional columns with metrics and the counters are appended after the last column.  

[![Profiling UI screenshot](img/assembly-view_1164x473.png)](img/assembly-view_1164x473.png){:target="_blank"}

???+ note
    When a function is opened in the Assembly view, its corresponding source file is automatically loaded in the *Source File* view and its [control-flow graph (CFG)](https://en.wikipedia.org/wiki/Control-flow_graph) displayed in the *Flow Graph* view.<br>
    [![Profiling UI screenshot](img/assembly-source_1073x279.png)](img/assembly-source_1073x279.png){:target="_blank"}

#### Assembly view

The function assembly area can be considered a read-only code editor, as such it follows the usual conventions. Each line corresponds to one instruction, with the following values left to right:

- instruction number (line number in the text)
- instruction virtual address (blue text)
- instruction opcode (bold text)
- a list of instruction operands
- source line number associated with the instruction from the debug info (gray text).
- inlinees (inlined functions) associated with the instruction from the debug info (green text).

##### Source lines

The debug information files usually contain a mapping between the source line numbers and the instructions that were generated for those lines. If available, the source line number is appended at the end of an instruction. Note that accuracy of this mapping usually depends on the compiler optimization level, with higher levels being less accurate or even lacking the mapping for some instructions.

Hovering over the source line shows a tooltip with the name and path of the source file. To copy this info to clipboard, first *click* the source line, then press *Ctrl+C*.  

[![Profiling UI screenshot](img/assemlby-line-number_816x106.png){: style="width:500px"}](img/assemlby-line-number_816x106.png){:target="_blank"}  

???+ note
    Clicking an instruction selects and brings into view its corresponding source line number in the *Source File* view and its basic block in the *Flow Graph* view.
    

##### Inlinees

During function inlining, the compiler may preserve additional details about the code being inlined so that the origin of an instruction can be saved into the debug information file. If available, the inlinees are appended after the source line number.

For example, if the function contains a call chain like foo() -> bar(), with both calls being inlined, the final instructions will record the fact that they originate from bar(), which got inlined into foo(), then foo() into the function.

Hovering over the inlinee shows a tooltip with the call path (stack trace) of the functions inlined at that point. To copy this info to clipboard, first *click* the inlinee, then press *Ctrl+C*:  

[![Profiling UI screenshot](img/assembly-inlinees_926x222.png){: style="width:500px"}](img/assembly-inlinees_926x222.png){:target="_blank"}

##### Basic blocks

The assembly is parsed and analyzed to recover the function's [control-flow graph (CFG)](https://en.wikipedia.org/wiki/Control-flow_graph), identifying [basic blocks](https://en.wikipedia.org/wiki/Basic_block) and loops. This information is used to split the assembly text into basic blocks and for the Flow Graph view.

The example below shows a subset of a function's basic blocks, with the corresponding control-flow graph part from the *Flow Graph* view. The basic blocks B3-B6 are marked on the left side and can be collapsed/expanded by *clicking* the -/+ buttons.

Notice how B5 is recognized for being a loop (the last instruction in the block jumps to the start of the block). The Flow Graph view uses a green arrow to mark loops - B4 is also the start block of a larger loop.

[![Profiling UI screenshot](img/assembly-flow-graph_579x600.png){: style="width:500px"}](img/assembly-flow-graph_579x600.png){:target="_blank"}  

##### Profiling annotations

Instruction execution time is displayed and annotated on several parts of the assembly instructions, columns, basic blocks and the control-flow graph, using text, colors and flame icons:

- the *Time (%)* column displays the instruction's execution time percentage relative to the total function execution time. The column style can be changed in the Assembly options.
- the *Time (ms)* column displays the instruction's execution time value. The time unit and column style can be changed in the Assembly options.
- the instruction background is colored based on its execution time - the slowest instruction has a red color, next slowest orange, then shades of yellow. The instruction location is also marked in the vertical text scrollbar.
- the three slowest instructions also have a flame icon in the *Time (%)* column using the same color coding.

[![Profiling UI screenshot](img/assembly-marking_1235x202.png)](img/assembly-marking_1235x202.png){:target="_blank"} 

- the basic blocks have a label with the block's execution time percentage, as a sum of its instruction's execution time (in the example above, the 55.73% label for B4). Hovering over the label shows the block's execution time value. The label background color uses the same color coding.
- the basic blocks in the *Flow Graph* view have below the same execution time percentage label as in the Assembly view, with the corresponding background color.

[![Profiling UI screenshot](img/assembly-marking-blocks_793x203.png)](img/assembly-marking-blocks_793x203.pngg){:target="_blank"} 

???+ note
    When displaying a function for the first time, by default, the slowest instruction is selected and brought into view (this can be configured in the Assembly options). 
    When the function is displayed subsequently, the last vertical scroll position is used instead.

    To jump at any time to the slowest instruction, *click* the red *Flame* icon from the toolbar or the *Ctrl+H* keyboard shortcut.

##### Call targets

Combining the parsed assembly and profiling information, call instructions are marked with their target(s) using an arrow icon placed before the call opcode:  

- for direct calls (target is an function name/address), a black arrow is used.
- for indirect or virtual function calls (target is a register or memory operand), a green arrow is used.

Hovering with the mouse over the arrow displays a list of the target functions, with details about the execution time. For example, the indirect call below at runtime has the *std::_Random_device* function as a sole target:

[![Profiling UI screenshot](img/assembly-call-target_691x172.png){: style="width:500px"}](img/assembly-call-target_691x172.png){:target="_blank"} 

???+ note
    Functions in the list have a right-click context menu with options to open the Assembly view, preview popup, and select the function in the other views. Double-click/Ctrl+Return opens the Assembly view for the selected function. Combine these shortcuts with the Shift key to open the Assembly view in a new tab instead.

!!! bug "TODO"
    Call target double-click to quick jump

#### Profiling toolbar

##### Profile

[![Profiling UI screenshot](img/assembly-profile_782x436.png){: style="width:550px"}](img/assembly-profile_782x436.png){:target="_blank"}  

##### Blocks

[![Profiling UI screenshot](img/assembly-blocks_560x439.png){: style="width:400px"}](img/assembly-blocks_560x439.png){:target="_blank"}  

##### Inlinees

[![Profiling UI screenshot](img/assembly-inlinees_1303x459.png)](img/assembly-inlinees_1303x459.png){:target="_blank"}  

##### Instances

[![Profiling UI screenshot](img/assembly-instances_1027x182.png)](img/assembly-instances_1027x182.png){:target="_blank"}  

##### Threads

#### Exporting

The function's assembly, combined with source line numbers and profiling annotations and execution time can be exported and saved into multiple formats, with the slowest instructions marked using a similar style as in the application:

- Excel worksheet (*.xlsx)  
  [![Profiling UI screenshot](img/summary-export-excel_1366x327.png)](img/summary-export-excel_1366x327.png){:target="_blank"}
- HTML table (*.html)  
  [![Profiling UI screenshot](img/summary-export-html_1209x287.png)](img/summary-export-html_1209x287.png){:target="_blank"}
- Markdown table (*.md)  
  [![Profiling UI screenshot](img/summary-export-markdown_1003x172.png)](img/summary-export-markdown_1003x172.png){:target="_blank"}

#### Assembly view interaction

???+ abstract "Toolbar"
    | Button | Description |
    | ------ | ------------|
    | ![](img/flame-graph-toolbar-sync.png) | If enabled, selecting a function also selects it in the other profiling views. |
    | ![](img/flame-graph-toolbar-source.png) | If enabled, selecting a function also displays the source in the Source file view, with the source lines annotated with profiling data. |
    | Export | Export the current function list into one of multiple formats (Excel, HTML and Markdown) or copy to clipboard the function list as  a HTML/Markdown table. |
    | Search box | Search for functions with a specific name using a case-insensitive substring search. Searching filters the list down to display only the matching entries. Press the *Escape* key to reset the search or the *X* button next to the input box. |

???+ abstract "Mouse shortcuts"
    | Action | Description |
    | ------ | ------------|
    | Hover | Hovering over a function displays a popup with the stack trace (call path) end with the slowest function's instance. Pin or drag the popup to keep it open. |
    | Click | Selects the function in the other views if *Sync* is enabled in the toolbar and displays the source in the Source file view if *Source* is enabled in the toolbar.  |
    | Double-click | Opens the Assembly view of the selected function in the current tab. |
    | Shift+Double-click | Opens the Assembly view of the selected function in a new tab. |
    | Right-click | Shows the context menu for the selected functions. |

    !!! note ""
        When multiple functions are selected, the application status bar displays the sum of their execution time as a percentage and value.

???+ abstract "Keyboard shortcuts"
    | Keys | Description |
    | ------ | ------------|
    | Return | Opens the Assembly view of the selected function in the current tab. |
    | Shift+Return | Opens the Assembly view of the selected function in a new tab. |
    | Ctrl+Shift+Left | Opens the Assembly view of the selected function in a new tab docked to the left of the active tab. |
    | Ctrl+Shift+Right | Opens the Assembly view of the selected function in a new tab docked to the right of the active tab. |
    | Alt+Return | Opens a preview popup with the assembly of the selected function. Press the *Escape* key to close the popup.<br><br>Multiple preview popups can be can be kept open at the same time. |
    | Ctrl+C | Copies to clipboard a HTML and Markdown table with a summary of the selected functions. |
    | Ctrl+Shift+C | Copies to clipboard the function names of the selected functions. |
    | Ctrl+Alt+C | Copies to clipboard the mangled/decorated function names of the selected functions. |


- tooltip over tab name

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