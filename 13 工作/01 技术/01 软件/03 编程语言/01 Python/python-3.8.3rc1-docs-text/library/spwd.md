"spwd" --- The shadow password database
***************************************

======================================================================

This module provides access to the Unix shadow password database. It
is available on various Unix versions.

You must have enough privileges to access the shadow password database
(this usually means you have to be root).

Shadow password database entries are reported as a tuple-like object,
whose attributes correspond to the members of the "spwd" structure
(Attribute field below, see "<shadow.h>"):

+---------+-----------------+-----------------------------------+
| 索引    | 属性            | 意义                              |
|=========|=================|===================================|
| 0       | "sp_namp"       | 登录名                            |
+---------+-----------------+-----------------------------------+
| 1       | "sp_pwdp"       | Encrypted password                |
+---------+-----------------+-----------------------------------+
| 2       | "sp_lstchg"     | Date of last change               |
+---------+-----------------+-----------------------------------+
| 3       | "sp_min"        | Minimal number of days between    |
|         |                 | changes                           |
+---------+-----------------+-----------------------------------+
| 4       | "sp_max"        | Maximum number of days between    |
|         |                 | changes                           |
+---------+-----------------+-----------------------------------+
| 5       | "sp_warn"       | Number of days before password    |
|         |                 | expires to warn user about it     |
+---------+-----------------+-----------------------------------+
| 6       | "sp_inact"      | Number of days after password     |
|         |                 | expires until account is disabled |
+---------+-----------------+-----------------------------------+
| 7       | "sp_expire"     | Number of days since 1970-01-01   |
|         |                 | when account expires              |
+---------+-----------------+-----------------------------------+
| 8       | "sp_flag"       | Reserved                          |
+---------+-----------------+-----------------------------------+

The sp_namp and sp_pwdp items are strings, all others are integers.
"KeyError" is raised if the entry asked for cannot be found.

定义了以下函数：

spwd.getspnam(name)

   Return the shadow password database entry for the given user name.

   在 3.6 版更改: Raises a "PermissionError" instead of "KeyError" if
   the user doesn't have privileges.

spwd.getspall()

   Return a list of all available shadow password database entries, in
   arbitrary order.

参见:

  模块 "grp"
     针对用户组数据库的接口，与本模块类似。

  Module "pwd"
     An interface to the normal password database, similar to this.
