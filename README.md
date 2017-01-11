# AltTab-Replacement

This is a replacement for the Alt-Tab behaviour on linux Cinnamon.

This version of Alt-Tab lists the application icons in a fixed, unchanging order so that tapping Alt-Tab a
fixed number of times always gets you to the same application. The user can specify the application order 
with environment variables.


Table of Contents
=================

* [The Problem Statement](#the-problem-statement)
* [Determining class and title](#determining-class-and-title)
* [Setting the sort order](#setting-the-sort-order)
* [Examples](#examples)
* [Named Workspaces](#named-workspaces)
* [Installation](#installation)

<hr width="100%">

## The Problem Statement

On linux/Cinnamon (and windows), pressing &lt;alt&gt;&lt;tab&gt; pops up a list of active applications, allowing the user to
navigate and select a window to switch to. When a different window is selected, the current window is pushed down, and
the new window comes forward.

<img align="right" src="https://github.com/ToolChainGang/AltTab-Replacement/wiki/images/TaskOrder1.png">

The problem is that alt-tab sorts the windows in least-recently-used order; meaning, the window you are in is the 1st
position, the one you were in last is in 2nd position, and so on.

This works for switching between two - and only two - windows, but if you need three or more it quickly becomes a
permutation engine. Selecting the 4th window puts the 1st window in 2nd place, the 2nd window in 3rd place, and so on.

<img align="right" src="https://github.com/ToolChainGang/AltTab-Replacement/wiki/images/TaskOrder2.png">
<img align="right" src="https://github.com/ToolChainGang/AltTab-Replacement/wiki/images/TaskOrder3.png">

For example, if you simultaneously use an editor for source, a command window to build, and a third window for test
(seeing the results), you cannot quickly switch between windows because the ordering is always different. You have to
stop, look at the list, recognize the icons, and then tell your fingers how many times to tap the <tab> key.


Add one or two more apps (a file explorer say, or a system monitor) and it rapidly becomes an exercise in fumbling and
redoing while paying attention: your fingers *want* to learn a fixed action to switch to the editor, but half the time
they go somewhere else and you have to stop, look, and recover.

It's annoying as hell.

This new version presents the tasks in a fixed, unchanging order. The user can use environment variables to specify
the sort order.

<hr width="100%">

## Determining class and title

The Alt-Tab behaviour from this project is determined by the CLASS and TITLE of the application window.

The CLASS identifies the application, and will be something like "Firefox" or "Nemo".

The TITLE identifies what the application is showing, such as a specific web page
(Firefox) or directory (Nemo). The TITLE is the string shown in the taskbar, and will be something like
"Google - Mozilla Firefox" (for Firefox) or "Home" (for Nemo), depending on what your application is showing.

To show all window classes and titles:

````bash
#
# List all window titles
#
wmctrl -l -x | awk '{print substr($0,index($0,$5))}'

#
# List all window classes
#
wmctrl -l -x | awk '{ print substr($3,1+index($3,".")) }'

````

<hr width="100%">

## Setting the sort order

Environment variables in the user's .profile file can define the sort order.

1. In workspace n, the sort order is determined by "ALT_TAB_WORKSPACEn" if this variable exists.
2. Otherwise, the sort order is determined by "ALT_TAB_ORDER" if this variable exists.
3. Otherwise, the windows are sorted by title.

The ALT_TAB_xxxx variables contain multiple CLASS:TITLE specifiers separated by semicolon.

Workspaces are numbered from 1, not zero. The first (leftmost) workspace is "WORKSPACE1".

The variables are part of the user's original login, so changes must be made to the user's .profile file
(and not, for example, the .bashrc file). After editing, log out and log back in for changes to take effect.

## Examples

Example1:

````bash
export ALT_TAB_WORKSPACE1="Firefox;Nemo;Gnome-terminal"
export ALT_TAB_WORKSPACE2="Gnome-terminal:Edit;Gnome-terminal:Work;VirtualBox;Firefox;Nemo"
````

In WORKSPACE1 the alt-tab listing will have firefox leftmost (in first position), Nemo in 2nd position, the
terminal in third position. Other windows will be to the right of the terminal.

In WORKSPACE2 the alt-tab listing will have both terminals leftmost, but the one titled "Edit" will
come before (to the left) of the one titled "Work", in 2nd position. VirtualBox, Firefox, and Nemo will follow,
then any unspecified windows.

Other workspaces will be sorted by title (the default).

Example2:

````bash
export ALT_TAB_ORDER="Firefox;Nemo;Gnome-terminal"
export ALT_TAB_WORKSPACE2="Gnome-terminal:Edit;Gnome-terminal:Work;VirtualBox;Firefox;Nemo"
````

WORKSPACE2 Alt-Tab will be as described above.

All other workspaces will show Firefox->Nemo->Gnome.


Example3:

````bash
#
# No preference in window order
#
#export ALT_TAB_ORDER="Firefox;Nemo;Gnome-terminal"

````

Alt-tab will sort by title, the default.

## Named Workspaces

If you have named your workspace, use "ALT_TAB_" with the new workspace name.

Example4:

````bash
#
# Workspace named "PERSONAL"
#
export ALT_TAB_PERSONAL="Thunderbird;Firefox;Nemo"

````

The renamed workspace "PERSONAL" will show in the order Thunderbird -> Firefox -> Nemo

All other workspaces will sort by title, the default.


<hr width="100%">

## Installation

Make a backup copy of the original appSwitcher.js file (important!) and move the new file into place.

1. Login as root
2. cd /usr/share/cinnamon/js/ui/appSwitcher
3. cp appSwitcher.js appSwitcher.js.orig
4. cp download-directory/appSwitcher.js .
5. Reboot window manager (usually ctrl-alt-esc will do this)

Change "download-directory" above to the directory you downloaded the project files to.
