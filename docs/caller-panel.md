The Caller/Callee panel displays the list of callers (functions calling it) and callees (function being called by it) for the active function (selected in the Summary or other views).  

[![Profiling UI screenshot](img/.png)](img/caller-view_1002x539.png){:target="_blank"}

By default all function instances are displayed combined, with the lists of callers and callees merged and execution time summed up. To display individual instances instead, toggle the *Combine* button in the toolbar.

Expanding a caller function form the list goes up the call tree, showing a list of callers which forms a stack trace leading to the entry function. This makes it easy to see the call path that leads to a caller.

TODO: IMAGE

Expanding a callee function from the list goes down the call tree, showing a list of callees until a leaf node is reached. This makes it easy to see which functions are being called starting with a callee.

TODO: IMAGE

To navigate, double-click or Return key.
Ctr+Return opens assembly view

TODO:
- right-click context menu
- mouse, keyboard shortcuts
- hover shows preview like in flame graph (details panel)