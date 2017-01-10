# AltTab-Replacement

This is a replacement for the Alt-Tab behaviour on linux Cinnamon.

The new version lists the Alt-Tab icons in a fixed, unchanging order. Additionally, the user can
specify the sort order by setting an environment variable.

## The Problem Statement

On linux/Cinnamon (and windows), pressing <alt><tab> pops up a list of active applications, allowing the user to
navigate and select a window to switch to. When a different window is selected, the current window is pushed down, and
the new window comes forward.

<img align="right" src="https://github.com/ToolChainGang/AltTab-Replacement/wiki/images/TaskOrder1.png" width="50%">

The problem is that alt-tab sorts the windows in least-recently-used order; meaning, the window you are in is the 1st
position, the one you were in last is in 2nd position, and so on.

This works for switching between two - and only two - windows, but if you need three or more it quickly becomes a
permutation engine. Selecting the 4th window puts the 1st window in 2nd place, the 2nd window in 3rd place, and so on.

<img align="right" src="https://github.com/ToolChainGang/AltTab-Replacement/wiki/images/TaskOrder2.png" width="50%">
<img align="right" src="https://github.com/ToolChainGang/AltTab-Replacement/wiki/images/TaskOrder3.png" width="50%">

For example, if you simultaneously use an editor for source, a command window to build, and a third window for test
(seeing the results), you cannot quickly switch between windows because the ordering is always different. You have to
stop, look at the list, recognize the icons, and then tell your fingers how many times to tap the <tab> key.


Add one or two more apps (a file explorer say, or a system monitor) and it rapidly becomes an exercise in fumbling and
redoing while paying attention: your fingers *want* to learn a fixed action to switch to the editor, but half the time
they go somewhere else and you have to stop, look, and recover.

It's annoying as hell.
