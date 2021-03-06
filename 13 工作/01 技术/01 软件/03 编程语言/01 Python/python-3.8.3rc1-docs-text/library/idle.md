IDLE
****

**源代码：** Lib/idlelib/

======================================================================

IDLE 是 Python 所内置的开发与学习环境。

IDLE 具有以下特性：

* 编码于 100% 纯正的 Python，使用名为 "tkinter" 的图形用户界面工具

* 跨平台：在 Windows、Unix 和 macOS 上工作近似。

* 提供输入输出高亮和错误信息的 Python 命令行窗口 （交互解释器）

* 提供多次撤销操作、Python 语法高亮、智能缩进、函数调用提示、自动补全
  等功能的多窗口文本编辑器

* 在多个窗口中检索，在编辑器中替换文本，以及在多个文件中检索（通过
  grep）

* 提供持久保存的断点调试、单步调试、查看本地和全局命名空间功能的调试器

* 配置、浏览以及其它对话框


目录
====

IDEL 具有两个主要窗口类型，分别是命令行窗口和编辑器窗口。用户可以同时
打开多个编辑器窗口。对于 Windows 和 Linux 平台，都有各自的主菜单。如下
记录的每个菜单标识着与之关联的窗口类型。

导出窗口，例如使用 编辑=>在文件中查找 是编辑器窗口的的一个子类型。它们
目前有着相同的主菜单，但是默认标题和上下文菜单不同。

在macOS上，只有一个应用程序菜单。它根据当前选择的窗口动态变化。它具有
一个IDLE菜单，并且下面描述的某些条目已移动到符合Apple准则的位置。


文件菜单 （命令行和编辑器）
---------------------------

新建文件
   创建一个文件编辑器窗口。

打开...
   使用打开窗口以打开一个已存在的文件。

近期文件
   打开一个近期文件列表，选取一个以打开它。

打开模块...
   打开一个已存在的模块 （搜索 sys.path）

类浏览器
   于当前所编辑的文件中使用树形结构展示函数、类以及方法。在命令行中，
   首先打开一个模块。

路径浏览
   在树状结构中展示 sys.path 目录、模块、函数、类和方法。

保存
   如果文件已经存在，则将当前窗口保存至对应的文件。自打开或上次保存之
   后经过修改的文件的窗口标题栏首尾将出现星号 * 。如果没有对应的文件，
   则使用“另存为”代替。

保存为...
   使用“保存为”对话框保存当前窗口。被保存的文件将作为当前窗口新的对应
   文件。

另存为副本...
   保存当前窗口至另一个文件，而不修改当前对应文件。

打印窗口
   通过默认打印机打印当前窗口。

关闭
   关闭当前窗口（如果未保存则询问）。

退出
   关闭所有窗口并退出 IDLE（如果未保存则询问）


编辑菜单（命令行和编辑器）
--------------------------

撤销操作
   撤销当前窗口的最近一次操作。最高可以撤回 1000 条操作记录。

重做
   重做当前窗口最近一次所撤销的操作。

剪切
   复制选区至系统剪贴板，然后删除选区。

复制
   复制选区至系统剪贴板。

粘贴
   插入系统剪贴板的内容至当前窗口。

剪贴板功能也可用于上下文目录。

全选
   选择当前窗口的全部内容。

查找...
   打开一个提供多选项的查找窗口。

再次查找
   重复上次搜索，如果结果存在。

查找选区
   查找当前选中的字符串，如果存在

在文件中查找...
   打开文件查找对话框。将结果输出至新的输出窗口。

替换...
   打开 查找并替换 对话框。

前往行
   Move the cursor to the beginning of the line requested and make
   that line visible.  A request past the end of the file goes to the
   end. Clear any selection and update the line and column status.

提示完成
   打开一个可滚动列表，允许选择关键字和属性。请参阅下面的 “编辑和导航”
   部分中的 Completions 。

展开文本
   展开键入的前缀以匹配同一窗口中的完整单词；重复以获得不同的扩展。

显示调用贴士
   在函数的右括号后，打开一个带有函数参数提示的小窗口。请参阅下面的“编
   辑和导航”部分中的 Calltips 。

显示周围括号
   突出显示周围的括号。


格式菜单（仅 window 编辑器）
----------------------------

增加缩进
   将选定的行右移缩进宽度（默认为4个空格）。

减少缩进
   将选定的行向左移动缩进宽度（默认为4个空格）。

注释
   在所选行的前面插入 ##。

取消注释
   从所选行中删除开头的 ＃ 或 ##。

制表符化
   将 *前导* 空格变成制表符。 （注意：我们建议使用4个空格来缩进Python
   代码。）

取消制表符化
   将 *所有* 制表符转换为正确的空格数。

缩进方式切换
   打开一个对话框，以在制表符和空格之间切换。

缩进宽度调整
   打开一个对话框以更改缩进宽度。 Python社区接受的默认值为4个空格。

格式段落
   在注释块或多行字符串或字符串中的选定行中，重新格式化当前以空行分隔
   的段落。段落中的所有行的格式都将少于N列，其中N默认为72。

删除尾随空格
   通过将 str.rstrip 应用于每行（包括多行字符串中的行），删除行尾非空
   白字符之后的尾随空格和其他空白字符。除Shell窗口外，在文件末尾删除多
   余的换行符。


运行菜单（仅 window 编辑器）
----------------------------

运行模块
   执行 检查模块 。如果没有错误，重新启动 shell 以清理环境，然后执行模
   块。输出显示在 shell 窗口中。请注意，输出需要使用 “打印” 或 “写入”
   。执行完成后，Shell 将保留焦点并显示提示。此时，可以交互地探索执行
   的结果。这类似于在命令行执行带有 "python -i file" 的文件。

运行... 定制
   与 运行模块 相同，但使用自定义设置运行该模块。*命令行参数* 扩展
   "sys.argv" ，就像在命令行上传递一样。该模块可以在命令行管理程序中运
   行，而无需重新启动。

检查模块
   检查 “编辑器” 窗口中当前打开的模块的语法。如果尚未保存该模块，则
   IDLE会提示用户保存或自动保存，如在 “空闲设置” 对话框的 “常规” 选项
   卡中所选择的那样。如果存在语法错误，则会在 “编辑器” 窗口中指示大概
   位置。

Python Shell
   打开或唤醒Python Shell窗口。


Shell 菜单（仅 window 编辑器）
------------------------------

查看最近重启
   将Shell窗口滚动到上一次Shell重启。

重启Shell
   重新启动shell 以清理环境。

上一条历史记录
   循环浏览历史记录中与当前条目匹配的早期命令。

下一条历史记录
   循环浏览历史记录中与当前条目匹配的后续命令。

中断执行
   停止正在运行的程序。


调试菜单（仅 window 编辑器）
----------------------------

跳转到文件/行
   Look on the current line. with the cursor, and the line above for a
   filename and line number.  If found, open the file if not already
   open, and show the line.  Use this to view source lines referenced
   in an exception traceback and lines found by Find in Files. Also
   available in the context menu of the Shell window and Output
   windows.

调试器（切换）
   激活后，在Shell中输入的代码或从编辑器中运行的代码将在调试器下运行。
   在编辑器中，可以使用上下文菜单设置断点。此功能不完整，具有实验性。

堆栈查看器
   在树状目录中显示最后一个异常的堆栈回溯，可以访问本地和全局。

自动打开堆栈查看器
   在未处理的异常上切换自动打开堆栈查看器。


选项菜单（命令行和编辑器）
--------------------------

配置 IDLE
   打开配置对话框并更改以下各项的首选项：字体、缩进、键绑定、文本颜色
   主题、启动窗口和大小、其他帮助源和扩展名。在MacOS上，通过在应用程序
   菜单中选择首选项来打开配置对话框。有关详细信息，请参阅：帮助和首选
   项下的 首选项设置。

大多数配置选项适用于所有窗口或将来的所有窗口。以下选项仅适用于活动窗口
。

显示/隐藏代码上下文（仅 window 编辑器）
   Open a pane at the top of the edit window which shows the block
   context of the code which has scrolled above the top of the window.
   See Code Context in the Editing and Navigation section below.

显示/隐藏行号（仅 window 编辑器）
   Open a column to the left of the edit window which shows the number
   of each line of text.  The default is off, which may be changed in
   the preferences (see Setting preferences).

缩放/还原高度
   Toggles the window between normal size and maximum height. The
   initial size defaults to 40 lines by 80 chars unless changed on the
   General tab of the Configure IDLE dialog.  The maximum height for a
   screen is determined by momentarily maximizing a window the first
   time one is zoomed on the screen. Changing screen settings may
   invalidate the saved height.  This toggle has no effect when a
   window is maximized.


Window 菜单（命令行和编辑器）
-----------------------------

列出所有打开的窗口的名称；选择一个将其带到前台（必要时对其进行去符号化
）。


帮助菜单（命令行和编辑器）
--------------------------

关于 IDLE
   显示版本，版权，许可证，荣誉等。

IDLE 帮助
   显示此 IDLE 文档，详细介绍菜单选项，基本编辑和导航以及其他技巧。

Python 文档
   访问本地Python文档（如果已安装），或启动Web浏览器并打开
   docs.python.org显示最新的Python文档。

海龟演示
   使用示例Python代码和turtle绘图运行turtledemo模块。

可以在“常规”选项卡下的“配置IDLE”对话框中添加其他帮助源。有关“帮助”菜单
选项的更多信息，请参见下面的 帮助源 小节。


上下文菜单
----------

Open a context menu by right-clicking in a window (Control-click on
macOS). Context menus have the standard clipboard functions also on
the Edit menu.

剪切
   复制选区至系统剪贴板，然后删除选区。

复制
   复制选区至系统剪贴板。

粘贴
   插入系统剪贴板的内容至当前窗口。

Editor windows also have breakpoint functions.  Lines with a
breakpoint set are specially marked.  Breakpoints only have an effect
when running under the debugger.  Breakpoints for a file are saved in
the user's ".idlerc" directory.

设置断点
   在当前行设置断点

清除断点
   清除当前行断点

shell 和输出窗口还具有以下内容。

跳转到文件/行
   与调试菜单相同。

The Shell window also has an output squeezing facility explained in
the *Python Shell window* subsection below.

压缩
   If the cursor is over an output line, squeeze all the output
   between the code above and the prompt below down to a 'Squeezed
   text' label.


编辑和导航
==========


编辑窗口
--------

IDLE may open editor windows when it starts, depending on settings and
how you start IDLE.  Thereafter, use the File menu.  There can be only
one open editor window for a given file.

The title bar contains the name of the file, the full path, and the
version of Python and IDLE running the window.  The status bar
contains the line number ('Ln') and column number ('Col').  Line
numbers start with 1; column numbers with 0.

IDLE assumes that files with a known .py* extension contain Python
code and that other files do not.  Run Python code with the Run menu.


按键绑定
--------

在本节中，'C' 是指 Windows 和 Unix 上的 "Control" 键，以及 macOS 上的
"Command" 键。

* "Backspace" 向左删除; "Del" 向右删除

* "C-Backspace" 向左删除单词; "C-Del" 向右删除单词

* 方向键和 "Page Up"/"Page Down" 移动

* "C-LeftArrow" 和 "C-RightArrow" 按单词移动

* "Home"/"End" 跳转到行首/尾

* "C-Home"/"C-End" 跳转到文档首/尾

* 一些有用的Emacs绑定是从Tcl / Tk继承的：

     * "C-a" 行首

     * "C-e" 行尾

     * "C-k" 删除行（但未将其放入剪贴板）

     * "C-l" center window around the insertion point

     * "C-b" go backward one character without deleting (usually you
       can also use the cursor key for this)

     * "C-f" go forward one character without deleting (usually you
       can also use the cursor key for this)

     * "C-p" go up one line (usually you can also use the cursor key
       for this)

     * "C-d" 删除下一个字符

Standard keybindings (like "C-c" to copy and "C-v" to paste) may work.
Keybindings are selected in the Configure IDLE dialog.


自动缩进
--------

After a block-opening statement, the next line is indented by 4 spaces
(in the Python Shell window by one tab).  After certain keywords
(break, return etc.) the next line is dedented.  In leading
indentation, "Backspace" deletes up to 4 spaces if they are there.
"Tab" inserts spaces (in the Python Shell window one tab), number
depends on Indent width. Currently, tabs are restricted to four spaces
due to Tcl/Tk limitations.

See also the indent/dedent region commands on the Format menu.


完成
----

Completions are supplied for functions, classes, and attributes of
classes, both built-in and user-defined. Completions are also provided
for filenames.

The AutoCompleteWindow (ACW) will open after a predefined delay
(default is two seconds) after a '.' or (in a string) an os.sep is
typed. If after one of those characters (plus zero or more other
characters) a tab is typed the ACW will open immediately if a possible
continuation is found.

If there is only one possible completion for the characters entered, a
"Tab" will supply that completion without opening the ACW.

'Show Completions' will force open a completions window, by default
the "C-space" will open a completions window. In an empty string, this
will contain the files in the current directory. On a blank line, it
will contain the built-in and user-defined functions and classes in
the current namespaces, plus any modules imported. If some characters
have been entered, the ACW will attempt to be more specific.

If a string of characters is typed, the ACW selection will jump to the
entry most closely matching those characters.  Entering a "tab" will
cause the longest non-ambiguous match to be entered in the Editor
window or Shell.  Two "tab" in a row will supply the current ACW
selection, as will return or a double click.  Cursor keys, Page
Up/Down, mouse selection, and the scroll wheel all operate on the ACW.

"Hidden" attributes can be accessed by typing the beginning of hidden
name after a '.', e.g. '_'. This allows access to modules with
"__all__" set, or to class-private attributes.

Completions and the 'Expand Word' facility can save a lot of typing!

Completions are currently limited to those in the namespaces. Names in
an Editor window which are not via "__main__" and "sys.modules" will
not be found.  Run the module once with your imports to correct this
situation. Note that IDLE itself places quite a few modules in
sys.modules, so much can be found by default, e.g. the re module.

If you don't like the ACW popping up unbidden, simply make the delay
longer or disable the extension.


提示
----

A calltip is shown when one types "(" after the name of an
*accessible* function.  A name expression may include dots and
subscripts.  A calltip remains until it is clicked, the cursor is
moved out of the argument area, or ")" is typed.  When the cursor is
in the argument part of a definition, the menu or shortcut display a
calltip.

A calltip consists of the function signature and the first line of the
docstring.  For builtins without an accessible signature, the calltip
consists of all lines up the fifth line or the first blank line.
These details may change.

The set of *accessible* functions depends on what modules have been
imported into the user process, including those imported by Idle
itself, and what definitions have been run, all since the last
restart.

For example, restart the Shell and enter "itertools.count(".  A
calltip appears because Idle imports itertools into the user process
for its own use. (This could change.)  Enter "turtle.write(" and
nothing appears.  Idle does not import turtle.  The menu or shortcut
do nothing either.  Enter "import turtle" and then "turtle.write("
will work.

In an editor, import statements have no effect until one runs the
file.  One might want to run a file after writing the import
statements at the top, or immediately run an existing file before
editing.


代码上下文
----------

Within an editor window containing Python code, code context can be
toggled in order to show or hide a pane at the top of the window.
When shown, this pane freezes the opening lines for block code, such
as those beginning with "class", "def", or "if" keywords, that would
have otherwise scrolled out of view.  The size of the pane will be
expanded and contracted as needed to show the all current levels of
context, up to the maximum number of lines defined in the Configure
IDLE dialog (which defaults to 15).  If there are no current context
lines and the feature is toggled on, a single blank line will display.
Clicking on a line in the context pane will move that line to the top
of the editor.

The text and background colors for the context pane can be configured
under the Highlights tab in the Configure IDLE dialog.


Python Shell 窗口
-----------------

With IDLE's Shell, one enters, edits, and recalls complete statements.
Most consoles and terminals only work with a single physical line at a
time.

When one pastes code into Shell, it is not compiled and possibly
executed until one hits "Return".  One may edit pasted code first. If
one pastes more that one statement into Shell, the result will be a
"SyntaxError" when multiple statements are compiled as if they were
one.

The editing features described in previous subsections work when
entering code interactively.  IDLE's Shell window also responds to the
following keys.

* "C-c" 中断执行命令

* "C-d" sends end-of-file; closes window if typed at a ">>>" prompt

* "Alt-/" (Expand word) is also useful to reduce typing

  历史命令

  * "Alt-p" retrieves previous command matching what you have typed.
    On macOS use "C-p".

  * "Alt-n" retrieves next. On macOS use "C-n".

  * "Return" while on any previous command retrieves that command


文本颜色
--------

Idle defaults to black on white text, but colors text with special
meanings. For the shell, these are shell output, shell error, user
output, and user error.  For Python code, at the shell prompt or in an
editor, these are keywords, builtin class and function names, names
following "class" and "def", strings, and comments. For any text
window, these are the cursor (when present), found text (when
possible), and selected text.

Text coloring is done in the background, so uncolorized text is
occasionally visible.  To change the color scheme, use the Configure
IDLE dialog Highlighting tab.  The marking of debugger breakpoint
lines in the editor and text in popups and dialogs is not user-
configurable.


启动和代码执行
==============

Upon startup with the "-s" option, IDLE will execute the file
referenced by the environment variables "IDLESTARTUP" or
"PYTHONSTARTUP". IDLE first checks for "IDLESTARTUP"; if "IDLESTARTUP"
is present the file referenced is run.  If "IDLESTARTUP" is not
present, IDLE checks for "PYTHONSTARTUP".  Files referenced by these
environment variables are convenient places to store functions that
are used frequently from the IDLE shell, or for executing import
statements to import common modules.

In addition, "Tk" also loads a startup file if it is present.  Note
that the Tk file is loaded unconditionally.  This additional file is
".Idle.py" and is looked for in the user's home directory.  Statements
in this file will be executed in the Tk namespace, so this file is not
useful for importing functions to be used from IDLE's Python shell.


命令行语法
----------

   idle.py [-c command] [-d] [-e] [-h] [-i] [-r file] [-s] [-t title] [-] [arg] ...

   -c command  run command in the shell window
   -d          enable debugger and open shell window
   -e          open editor window
   -h          print help message with legal combinations and exit
   -i          open shell window
   -r file     run file in shell window
   -s          run $IDLESTARTUP or $PYTHONSTARTUP first, in shell window
   -t title    set title of shell window
   -           run stdin in shell (- must be last option before args)

如果有参数：

* If "-", "-c", or "r" is used, all arguments are placed in
  "sys.argv[1:...]" and "sys.argv[0]" is set to "''", "'-c'", or
  "'-r'".  No editor window is opened, even if that is the default set
  in the Options dialog.

* Otherwise, arguments are files opened for editing and "sys.argv"
  reflects the arguments passed to IDLE itself.


启动失败
--------

IDLE uses a socket to communicate between the IDLE GUI process and the
user code execution process.  A connection must be established
whenever the Shell starts or restarts.  (The latter is indicated by a
divider line that says 'RESTART'). If the user process fails to
connect to the GUI process, it displays a "Tk" error box with a
'cannot connect' message that directs the user here.  It then exits.

A common cause of failure is a user-written file with the same name as
a standard library module, such as *random.py* and *tkinter.py*. When
such a file is located in the same directory as a file that is about
to be run, IDLE cannot import the stdlib file.  The current fix is to
rename the user file.

Though less common than in the past, an antivirus or firewall program
may stop the connection.  If the program cannot be taught to allow the
connection, then it must be turned off for IDLE to work.  It is safe
to allow this internal connection because no data is visible on
external ports.  A similar problem is a network mis-configuration that
blocks connections.

Python installation issues occasionally stop IDLE: multiple versions
can clash, or a single installation might need admin access.  If one
undo the clash, or cannot or does not want to run as admin, it might
be easiest to completely remove Python and start over.

A zombie pythonw.exe process could be a problem.  On Windows, use Task
Manager to check for one and stop it if there is.  Sometimes a restart
initiated by a program crash or Keyboard Interrupt (control-C) may
fail to connect.  Dismissing the error box or using Restart Shell on
the Shell menu may fix a temporary problem.

When IDLE first starts, it attempts to read user configuration files
in "~/.idlerc/" (~ is one's home directory).  If there is a problem,
an error message should be displayed.  Leaving aside random disk
glitches, this can be prevented by never editing the files by hand.
Instead, use the configuration dialog, under Options.  Once there is
an error in a user configuration file, the best solution may be to
delete it and start over with the settings dialog.

If IDLE quits with no message, and it was not started from a console,
try starting it from a console or terminal ("python -m idlelib") and
see if this results in an error message.


运行用户代码
------------

With rare exceptions, the result of executing Python code with IDLE is
intended to be the same as executing the same code by the default
method, directly with Python in a text-mode system console or terminal
window. However, the different interface and operation occasionally
affect visible results.  For instance, "sys.modules" starts with more
entries, and "threading.activeCount()" returns 2 instead of 1.

By default, IDLE runs user code in a separate OS process rather than
in the user interface process that runs the shell and editor.  In the
execution process, it replaces "sys.stdin", "sys.stdout", and
"sys.stderr" with objects that get input from and send output to the
Shell window. The original values stored in "sys.__stdin__",
"sys.__stdout__", and "sys.__stderr__" are not touched, but may be
"None".

When Shell has the focus, it controls the keyboard and screen.  This
is normally transparent, but functions that directly access the
keyboard and screen will not work.  These include system-specific
functions that determine whether a key has been pressed and if so,
which.

IDLE's standard stream replacements are not inherited by subprocesses
created in the execution process, whether directly by user code or by
modules such as multiprocessing.  If such subprocess use "input" from
sys.stdin or "print" or "write" to sys.stdout or sys.stderr, IDLE
should be started in a command line window.  The secondary subprocess
will then be attached to that window for input and output.

The IDLE code running in the execution process adds frames to the call
stack that would not be there otherwise.  IDLE wraps
"sys.getrecursionlimit" and "sys.setrecursionlimit" to reduce the
effect of the additional stack frames.

If "sys" is reset by user code, such as with "importlib.reload(sys)",
IDLE's changes are lost and input from the keyboard and output to the
screen will not work correctly.

When user code raises SystemExit either directly or by calling
sys.exit, IDLE returns to a Shell prompt instead of exiting.


Shell中的用户输出
-----------------

When a program outputs text, the result is determined by the
corresponding output device.  When IDLE executes user code,
"sys.stdout" and "sys.stderr" are connected to the display area of
IDLE's Shell.  Some of its features are inherited from the underlying
Tk Text widget.  Others are programmed additions.  Where it matters,
Shell is designed for development rather than production runs.

For instance, Shell never throws away output.  A program that sends
unlimited output to Shell will eventually fill memory, resulting in a
memory error. In contrast, some system text windows only keep the last
n lines of output. A Windows console, for instance, keeps a user-
settable 1 to 9999 lines, with 300 the default.

A Tk Text widget, and hence IDLE's Shell, displays characters
(codepoints) in the BMP (Basic Multilingual Plane) subset of Unicode.
Which characters are displayed with a proper glyph and which with a
replacement box depends on the operating system and installed fonts.
Tab characters cause the following text to begin after the next tab
stop. (They occur every 8 'characters').  Newline characters cause
following text to appear on a new line.  Other control characters are
ignored or displayed as a space, box, or something else, depending on
the operating system and font.  (Moving the text cursor through such
output with arrow keys may exhibit some surprising spacing behavior.)

   >>> s = 'a\tb\a<\x02><\r>\bc\nd'  # Enter 22 chars.
   >>> len(s)
   14
   >>> s  # Display repr(s)
   'a\tb\x07<\x02><\r>\x08c\nd'
   >>> print(s, end='')  # Display s as is.
   # Result varies by OS and font.  Try it.

The "repr" function is used for interactive echo of expression values.
It returns an altered version of the input string in which control
codes, some BMP codepoints, and all non-BMP codepoints are replaced
with escape codes. As demonstrated above, it allows one to identify
the characters in a string, regardless of how they are displayed.

Normal and error output are generally kept separate (on separate
lines) from code input and each other.  They each get different
highlight colors.

For SyntaxError tracebacks, the normal '^' marking where the error was
detected is replaced by coloring the text with an error highlight.
When code run from a file causes other exceptions, one may right click
on a traceback line to jump to the corresponding line in an IDLE
editor. The file will be opened if necessary.

Shell has a special facility for squeezing output lines down to a
'Squeezed text' label.  This is done automatically for output over N
lines (N = 50 by default). N can be changed in the PyShell section of
the General page of the Settings dialog.  Output with fewer lines can
be squeezed by right clicking on the output.  This can be useful lines
long enough to slow down scrolling.

Squeezed output is expanded in place by double-clicking the label. It
can also be sent to the clipboard or a separate view window by right-
clicking the label.


开发 tkinter 应用程序
---------------------

IDLE is intentionally different from standard Python in order to
facilitate development of tkinter programs.  Enter "import tkinter as
tk; root = tk.Tk()" in standard Python and nothing appears.  Enter the
same in IDLE and a tk window appears.  In standard Python, one must
also enter "root.update()" to see the window.  IDLE does the
equivalent in the background, about 20 times a second, which is about
every 50 milliseconds. Next enter "b = tk.Button(root, text='button');
b.pack()".  Again, nothing visibly changes in standard Python until
one enters "root.update()".

Most tkinter programs run "root.mainloop()", which usually does not
return until the tk app is destroyed.  If the program is run with
"python -i" or from an IDLE editor, a ">>>" shell prompt does not
appear until "mainloop()" returns, at which time there is nothing left
to interact with.

When running a tkinter program from an IDLE editor, one can comment
out the mainloop call.  One then gets a shell prompt immediately and
can interact with the live application.  One just has to remember to
re-enable the mainloop call when running in standard Python.


在没有子进程的情况下运行
------------------------

By default, IDLE executes user code in a separate subprocess via a
socket, which uses the internal loopback interface.  This connection
is not externally visible and no data is sent to or received from the
Internet. If firewall software complains anyway, you can ignore it.

If the attempt to make the socket connection fails, Idle will notify
you. Such failures are sometimes transient, but if persistent, the
problem may be either a firewall blocking the connection or
misconfiguration of a particular system.  Until the problem is fixed,
one can run Idle with the -n command line switch.

If IDLE is started with the -n command line switch it will run in a
single process and will not create the subprocess which runs the RPC
Python execution server.  This can be useful if Python cannot create
the subprocess or the RPC socket interface on your platform.  However,
in this mode user code is not isolated from IDLE itself.  Also, the
environment is not restarted when Run/Run Module (F5) is selected.  If
your code has been modified, you must reload() the affected modules
and re-import any specific items (e.g. from foo import baz) if the
changes are to take effect.  For these reasons, it is preferable to
run IDLE with the default subprocess if at all possible.

3.4 版后已移除.


帮助和偏好
==========


帮助源
------

Help menu entry "IDLE Help" displays a formatted html version of the
IDLE chapter of the Library Reference.  The result, in a read-only
tkinter text window, is close to what one sees in a web browser.
Navigate through the text with a mousewheel, the scrollbar, or up and
down arrow keys held down. Or click the TOC (Table of Contents) button
and select a section header in the opened box.

Help menu entry "Python Docs" opens the extensive sources of help,
including tutorials, available at "docs.python.org/x.y", where 'x.y'
is the currently running Python version.  If your system has an off-
line copy of the docs (this may be an installation option), that will
be opened instead.

Selected URLs can be added or removed from the help menu at any time
using the General tab of the Configure IDLE dialog.


首选项设置
----------

The font preferences, highlighting, keys, and general preferences can
be changed via Configure IDLE on the Option menu. Non-default user
settings are saved in a ".idlerc" directory in the user's home
directory.  Problems caused by bad user configuration files are solved
by editing or deleting one or more of the files in ".idlerc".

On the Font tab, see the text sample for the effect of font face and
size on multiple characters in multiple languages.  Edit the sample to
add other characters of personal interest.  Use the sample to select
monospaced fonts.  If particular characters have problems in Shell or
an editor, add them to the top of the sample and try changing first
size and then font.

On the Highlights and Keys tab, select a built-in or custom color
theme and key set.  To use a newer built-in color theme or key set
with older IDLEs, save it as a new custom theme or key set and it well
be accessible to older IDLEs.


macOS 上的IDLE
--------------

Under System Preferences: Dock, one can set "Prefer tabs when opening
documents" to "Always".  This setting is not compatible with the
tk/tkinter GUI framework used by IDLE, and it breaks a few IDLE
features.


扩展
----

IDLE contains an extension facility.  Preferences for extensions can
be changed with the Extensions tab of the preferences dialog. See the
beginning of config-extensions.def in the idlelib directory for
further information.  The only current default extension is zzdummy,
an example also used for testing.
