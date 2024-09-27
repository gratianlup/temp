

???+ note "Flame graphs"
    A flame graph is an alternative, more compact way of viewing a call tree. In this view, function instances are nodes with a size proportional to the time spent relative to the parent (caller) function and makes it easier to identify the portions of the app that take most of the time.  

    Nodes are sorted in the horizontal direction based on decreasing time relative to their parent node, while the ordering in the vertical direction forms a stack trace (a path in the call tree).

    The following links provide an introduction to the flame graph visualization concept, its history, and how it's being used across the industry for performance investigations.  

    - [CPU Flame Graphs (brendangregg.com)](https://www.brendangregg.com/FlameGraphs/cpuflamegraphs.html)
    - [Visualizing Performance - The Developers’ Guide to Flame Graphs (youtube.com)](https://www.youtube.com/watch?v=VMpTU15rIZY)

### Toolbar buttons
