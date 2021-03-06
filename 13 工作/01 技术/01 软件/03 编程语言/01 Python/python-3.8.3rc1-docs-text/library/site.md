"site" —— 指定域的配置钩子
**************************

**源代码:** Lib/site.py

======================================================================

**这个模块将在初始化时被自动导入。** 此自动导入可以通过使用解释器的
"-S" 选项来屏蔽。

导入此模块将会附加域特定的路径到模块搜索路径并且添加一些内建对象，除非
使用了 "-S" 选项。在此例中，模块可以被安全地导入，而不会对模块搜索路径
和内建对象有自动的修改或添加。要明确地触发通常域特定的添加，调用函数
"site.main()"。

在 3.3 版更改: 即便使用了 "-S"，也会导入此模块用于触发路径操纵。

It starts by constructing up to four directories from a head and a
tail part. For the head part, it uses "sys.prefix" and
"sys.exec_prefix"; empty heads are skipped.  For the tail part, it
uses the empty string and then "lib/site-packages" (on Windows) or
"lib/python*X.Y*/site-packages" (on Unix and Macintosh).  For each of
the distinct head-tail combinations, it sees if it refers to an
existing directory, and if so, adds it to "sys.path" and also inspects
the newly added path for configuration files.

在 3.5 版更改: Support for the "site-python" directory has been
removed.

如果名为 "pyvenv.cfg" 的文件存在于 sys.executable 之上的一个目录中，则
sys.prefix 和 sys.exec_prefix 将被设置为该目录，并且还会检查 site-
packages （ sys.base_prefix 和 sys.base_exec_prefix 始终是 Python 安装
的 "真实" 前缀）。 如果 "pyvenv.cfg" （引导程序配置文件）包含设置为非
"true"（不区分大小写）的 "include-system-site-packages" 键，则不会在系
统级前缀中搜索 site-packages；反之则会。

一个路径配置文件是具有 "*name*.pth" 命名格式的文件，并且存在上面提到的
四个目录之一中；它的内容是要添加到 "sys.path" 中的额外项目（每行一个）
。不存在的项目不会添加到 "sys.path"，并且不会检查项目指向的是目录还是
文件。项目不会被添加到 "sys.path" 超过一次。空行和由 "#" 起始的行会被
跳过。以 "import" 开始的行（跟着空格或 TAB）会被执行。

注解:

  每次启动 Python，在 ".pth" 文件中的可执行行都将会被运行，而不管特定
  的模块实际上是否需要被使用。 因此，其影响应降至最低。可执行行的主要
  预期目的是使相关模块可导入（加载第三方导入钩子，调整 "PATH" 等）。如
  果它发生了，任何其他的初始化都应当在模块实际导入之前完成。将代码块限
  制为一行是一种有意采取的措施，不鼓励在此处放置更复杂的内容。

例如，假设 "sys.prefix" 和 "sys.exec_prefix" 已经被设置为 "/usr/local"
。 Python X.Y 的库之后被安装为 "/usr/local/lib/python*X.Y*"。假设有一
个拥有三个孙目录 "foo", "bar" 和 "spam" 的子目录
"/usr/local/lib/python*X.Y*/site-packages"，并且有两个路径配置文件
"foot.pth" 和 "bar.pth"。假定 "foo.pth" 内容如下:

   # foo package configuration

   foo
   bar
   bletch

并且 "bar.pth" 包含:

   # bar package configuration

   bar

则下面特定版目录将以如下顺序被添加到 "sys.path"。

   /usr/local/lib/pythonX.Y/site-packages/bar
   /usr/local/lib/pythonX.Y/site-packages/foo

Note that "bletch" is omitted because it doesn't exist; the "bar"
directory precedes the "foo" directory because "bar.pth" comes
alphabetically before "foo.pth"; and "spam" is omitted because it is
not mentioned in either path configuration file.

After these path manipulations, an attempt is made to import a module
named "sitecustomize", which can perform arbitrary site-specific
customizations. It is typically created by a system administrator in
the site-packages directory.  If this import fails with an
"ImportError" or its subclass exception, and the exception's "name"
attribute equals to "'sitecustomize'", it is silently ignored.  If
Python is started without output streams available, as with
"pythonw.exe" on Windows (which is used by default to start IDLE),
attempted output from "sitecustomize" is ignored.  Any other exception
causes a silent and perhaps mysterious failure of the process.

After this, an attempt is made to import a module named
"usercustomize", which can perform arbitrary user-specific
customizations, if "ENABLE_USER_SITE" is true.  This file is intended
to be created in the user site-packages directory (see below), which
is part of "sys.path" unless disabled by "-s".  If this import fails
with an "ImportError" or its subclass exception, and the exception's
"name" attribute equals to "'usercustomize'", it is silently ignored.

Note that for some non-Unix systems, "sys.prefix" and
"sys.exec_prefix" are empty, and the path manipulations are skipped;
however the import of "sitecustomize" and "usercustomize" is still
attempted.


Readline configuration
======================

On systems that support "readline", this module will also import and
configure the "rlcompleter" module, if Python is started in
interactive mode and without the "-S" option. The default behavior is
enable tab-completion and to use "~/.python_history" as the history
save file.  To disable it, delete (or override) the
"sys.__interactivehook__" attribute in your "sitecustomize" or
"usercustomize" module or your "PYTHONSTARTUP" file.

在 3.4 版更改: Activation of rlcompleter and history was made
automatic.


模块内容
========

site.PREFIXES

   A list of prefixes for site-packages directories.

site.ENABLE_USER_SITE

   Flag showing the status of the user site-packages directory.
   "True" means that it is enabled and was added to "sys.path".
   "False" means that it was disabled by user request (with "-s" or
   "PYTHONNOUSERSITE").  "None" means it was disabled for security
   reasons (mismatch between user or group id and effective id) or by
   an administrator.

site.USER_SITE

   Path to the user site-packages for the running Python.  Can be
   "None" if "getusersitepackages()" hasn't been called yet.  Default
   value is "~/.local/lib/python*X.Y*/site-packages" for UNIX and non-
   framework Mac OS X builds, "~/Library/Python/*X.Y*/lib/python/site-
   packages" for Mac framework builds, and
   "*%APPDATA%*\Python\Python*XY*\site-packages" on Windows.  This
   directory is a site directory, which means that ".pth" files in it
   will be processed.

site.USER_BASE

   Path to the base directory for the user site-packages.  Can be
   "None" if "getuserbase()" hasn't been called yet.  Default value is
   "~/.local" for UNIX and Mac OS X non-framework builds,
   "~/Library/Python/*X.Y*" for Mac framework builds, and
   "*%APPDATA%*\Python" for Windows.  This value is used by Distutils
   to compute the installation directories for scripts, data files,
   Python modules, etc. for the user installation scheme. See also
   "PYTHONUSERBASE".

site.main()

   Adds all the standard site-specific directories to the module
   search path.  This function is called automatically when this
   module is imported, unless the Python interpreter was started with
   the "-S" flag.

   在 3.3 版更改: This function used to be called unconditionally.

site.addsitedir(sitedir, known_paths=None)

   Add a directory to sys.path and process its ".pth" files.
   Typically used in "sitecustomize" or "usercustomize" (see above).

site.getsitepackages()

   Return a list containing all global site-packages directories.

   3.2 新版功能.

site.getuserbase()

   Return the path of the user base directory, "USER_BASE".  If it is
   not initialized yet, this function will also set it, respecting
   "PYTHONUSERBASE".

   3.2 新版功能.

site.getusersitepackages()

   Return the path of the user-specific site-packages directory,
   "USER_SITE".  If it is not initialized yet, this function will also
   set it, respecting "PYTHONNOUSERSITE" and "USER_BASE".

   3.2 新版功能.


命令行界面
==========

The "site" module also provides a way to get the user directories from
the command line:

   $ python3 -m site --user-site
   /home/user/.local/lib/python3.3/site-packages

If it is called without arguments, it will print the contents of
"sys.path" on the standard output, followed by the value of
"USER_BASE" and whether the directory exists, then the same thing for
"USER_SITE", and finally the value of "ENABLE_USER_SITE".

--user-base

   Print the path to the user base directory.

--user-site

   Print the path to the user site-packages directory.

If both options are given, user base and user site will be printed
(always in this order), separated by "os.pathsep".

If any option is given, the script will exit with one of these values:
"0" if the user site-packages directory is enabled, "1" if it was
disabled by the user, "2" if it is disabled for security reasons or by
an administrator, and a value greater than 2 if there is an error.

参见: **PEP 370** -- 分用户的 site-packages 目录
