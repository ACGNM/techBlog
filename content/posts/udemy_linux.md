---
title: "Udemy_linux"
date: 2021-03-07T21:23:26+09:00
draft: false
toc: false
images:
tags:
  - linux
---
## Day 1
查找Linux发行版的网站 [DistroWatch.com](https://distrowatch.com/?language=EN)

## Day 2
- /etc 系统设置文件夹
- /var 主要是日志文件存储地
- /usr/local 非系统自带软件安装地（并非所有）
	/opt 可选，第三方软件安装地
    - 具体内容有可能是软件目录下有自己的目录结构，也有可能共享目录（说的是设置，日志等）：如在/etc下存储自己的设置，在/var下存储自己的日志 <- 不在自己的安装目录下
    
    - 例子1 
    	```
    	/etc/opt/myapp
    	/opt/myapp/bin
    	/opt/myapp/lib
    	/var/opt/myapp
        ```
    
    - 例子2
    	```
    	/usr/local/bin/myapp
    	/usr/local/lib/libmyapp.so
    	/usr/local/etc/myapp.conf
    	```

- Tilde(波浪号，チルダ)
		```
		~jason = /home/jason
    	~gongzhihao = /home/gongzhihao
    	~root = /root
    	~ftp = /srv/ftp
        ```
        
- man 相关
	- 使用man查询用法说明后，回车下一行，空格下一页，g回到顶端，G到底部（shift+g），推出按q
    - `man -k 搜索内容` 在man页面中进行搜索
- 环境变量
	- PATH：里面的值是执行命令时的搜索路径（执行后在里面搜索对象，目录之间用冒号隔开）
    
- `cd -` 回到刚才的目录

- `pwd` present working directory

- `echo $OLDPWD` 刚才的工作目录（= `cd -`）

- **创建和删除目录**
	- `mkdir [-p] 目录名` -p 是用来创建父目录（parent）或者说可以直接创建深层目录如：mkdir -p 1/2/3
    - `rmdir [-p] 目录名` -p 作用同上（但是只能删除空目录）
    - `rm -rf 目录名` 递归删除目录下所有内容（recursively delete）

- 显示文件或者目录信息
	- `ls -F`  显示文件类型
		- `/` 目录。`@` 链接。`*` 可执行文件
    - `ls -t` 按时间排序。 `ls -r` 逆向排序。 `ls -latr` 将所有文件详的细信息按照时间逆序排列（最新的在最下面）。
    - `ls -R` 递归地显示当前文件夹下的所有内容。`ls --color` 将结果上色。`ls -d` 只显示目录。
    - `tree` 输出树形结构。`tree -d`只显示目录不显示文件。`ls -c`给结果上色。
    - 如果目标文件或者目录有空格，使用双引号或者单引号括住文件名来执行命令。 如 `ls "my notes.txt"` 。
    
## Day 3
- 权限定义
	- ugoa 代表 user, group, other, all
    - 修改权限: chmod ugoa(中的一种或几种) +/-/= 权限（rwx）文件名
    	- e.g. `chmod u+rw,g+w sales.data`
            
    - 用数字代表的权限: 
    	| r | w | x |
    	|---|---|---|
    	| 4 | 2 | 1 |
    
    	- 指定的权限是数字的加和
        	- `chmod 644 sales.data` (-rw-r--r--)
- 创建一个文件默认被指定到当前用户的主群组（primary group）
	- 如果想改变所属的群组，使用`chgrp`
- 文件的权限会受到所在文件夹权限的影响
	- 即使文件本身的权限可读（或者其他），如果所在文件夹甚至其所在文件夹的父文件夹(及其父文件夹直至root)的权限上不允许，则无法执行想要的动作
- 创建文件时的默认权限（File Creation Mask）是指创建文件时赋予的权限
	- 如果创建时没有指定`mask`
    	- 目录默认给予777
        - 文件默认给予666
    - `umask`命令用来查看当前文件夹赋予的默认权限状况
    	- 与`chmod`相反，显示的结果是创建时减去的值
        	- 如默认创建文件时权限为`666`，`umask`结果为`0022`时创建文件权限会被自动设置为`644`
        - 以四位表示，转换为三位去掉最前面的零即可（如`0022=022`）
        - `umask 007`将umask设置为`00007`(即创建文件或目录时Other权限为0)
        
- 浏览文件内容
	- `cat`
    - `more`
    - `less`
    - `head`
    - `tail`
    	- `tail -f`可以持续追踪文件内容 
        - `tail -n`指定显示最后多少行（`head`同样用法）
        - 默认显示10行
    
- 编辑文件
	- `nano`
	- `vi`
		- `vim`是vi improvement
		- 分为三个模式
			- Command Mode
				- `k, j`:上一行, 下一行
				- `h, l`:左一字符，右一字符
				- `w, b`:左一词，右一词
				- `^, $`:到本行的开始，结束
			- Insert Mode
				- `i` 在光标位置插入
				- `I` 在行的开头处插入
				- `a` 在光标后插入
				- `A` 在行的结尾处插入
			- Line Mode
				- `:w` 写入文件
				- `:w!` 强行写入
				- `:q` 退出
				- `:q!` 退出但不保存
				- `:wq!` 保存并退出
				- `:x` 与:wq相同
				- `:n` 将光标放在第n行
				- `$` 将光标放在最后一行
				- `set nu/noun` 显示/关闭行数显示
		- 重复命令
			- `5k` 光标向上移动五行
			- `80i<Text><Esc>` 插入<Text>八十次
		- 删除命令
			- `x` 删除一个字符
			- `dw` 删除一个单词
			- `dd` 删除一行
			- `D` 从当前位置开始删除
			- `Dd` 删除从当前位置到结尾的内容
		- 替换/改变命令
			- `r` 改变当前字符
			- `cw` 改变一个词
			- `cc` 改变当前行
			- `c$` 改变当前位置到结尾的内容
			- `C` 与`c$`相同
			- `~` 改变当前字符大小写
		- 复制与粘贴
			- `yy` 复制当前的行
			- `y<position>` 复制指定位置的字符
				- `yw`, `y3w` 复制一个词，复制三个词
			- `p` 粘贴最近删除或复制的内容
		- `u`, `Ctrl-R` Undo和Redo
		- `/<pattern>`, `?<pattern>` 向后，向前搜索
	- Emacs
    	
         
- 只有特定group的人有权限怎么做？
