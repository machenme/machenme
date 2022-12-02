# MIT-Missing-Semester

## 课程简介

- 先修要求：无
- 编程语言：shell
- 课程难度：🌟🌟
- 预计学时：10 小时

正如课程名字所言：“计算机教学中消失的一个学期”，这门课将会教会你许多大学的课堂上不会涉及但却对每个 CSer 无比重要的工具或者知识点。例如 Shell 编程、命令行配置、Git、Vim、`tmux`、`ssh` 等等。如果你是一个计算机小白，那么我非常建议你学习一下这门课，因为它基本涉及了本书必学工具中的绝大部分内容。

除了 MIT 官方的学习资料外，北京大学图灵班开设的前沿计算实践中也开设了相关课程，资料位于[这个网站](http://vcl.pku.edu.cn/course/PFCII/2021-spring/index.html)下，供大家参考。

## 课程资源

- 课程网站：<https://missing.csail.mit.edu/2020/>
- 课程视频：<https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J>
- 课程作业：一些随堂小练习，具体见课程网站。

---

# Shell(bash)

## Navigate in shell

`pwd`: print work dictionary 显示当前目录    
`cd` : change dictionary 改变目录  
`ls` : show contents of current directory  显示当前目录文件    
`ls -l`: use a long listing format to show contents  用列表方式显示当前目录详细信息
```ubuntu
(base) chen@ubuntu:~$ ls -l ~
总用量 48
drwxr-xr-x  2 chen chen 4096 11月 28 03:50 Desktop
drwxr-xr-x  3 chen chen 4096 11月 27 11:38 Documents
```
`drwxr-xr-x`:   d代表文件夹，rwx分别代表读、写、执行权限  
- `d` tell us `Desktop` is a dictionary  
- `rwx` indicate permissions the owner of the file(dictionary)
    - `r` read
    - `w` write
    - `x` execute
    - `-` have no permissions
    - first `rwx` for user, second `rwx` for groups(with user), third `rwx` for everyone else  

`mv` : move or rename a file  移动文件或者重命名  
`cp` : copy   拷贝文件  
`mkdir` : create a new dictionary  创建目录  

## Connecting programs
`a>b` : use a instead b output steam  使用a作为b的输出  
`a>>b`: let a add to b last  把a添加到b的最后面  
`a<b` : use b instead a input steam  使用b作为a的输入
```ubuntu
ls /home > count.txt
# it will write home dictionary ls info into count.txt
```
`a|b` : let b use a's output as input(we call "|" pipes, it links left and right)  让b使用a的输出，比如查询的时候需要传入查询的表达式包含的信息  
```ubuntu
ls -l | tail -n3
# ls -l will show all info, tail -n3 will show last 3 line info.
```
`sudo` : do something like a super user(root)  管理员权限  

# vim

see vscode extension : Learn Vim
- `i`: insert mode. with i from normal model, vim are same like other ide.
- `esc` or `ctrl+c` or `ctrl+[`: from any model back to normal model. 
- `hjkl`: move command.
    - `h`: move to left char,  same as `←`.
    - `j`: move to next line,  same as `↓`.
    - `j`: move to last line,  same as `↑`.
    - `l`: move to right char, same as `→`.
- `w`: move word by word.
- `b`: move backwards word by word.
- `W`: to move word by WORD(ignore some mark like `,()_[]`).
- `B`: to move backwards WORD by WORD.
- `e`: move word end by word end.
- `ge`: back word end by word end.
- `f{character}` eg `fa` : will go to first `a` at line. do it again will find second `a` and so on.
- `F{character}` eg `Fb` : find the previous occurrence of a character.
- `t{character}` eg `ta` : to move the cursor just before the next occurrence of a character.
- `T{character}` eg `Ta` : to move the cursor just before the last occurrence of a character. 
- `fd,`,`fd;;` : when you type `fd,`, it means you find `d` then back to last `d`,`,`same as `Fd`. `;` means `fd`.
- `0` : move to first character of a line.
- `$` : move to end of a line. 
- `^` : move to first non-block character of a line.
- `g_`: move to end non-block character of a line.
- `}`: jumps entire paragraphs downwards.
- `{`: similarly but upwards.
- `CTRL-D`: lets you move down half a page by scrolling the page.
- `CTRL-U`: lets you move up half a page also by scrolling.
- `/{pattern}`: to search forward. 
- `?{pattern}`: to search backward.
    - `n`: to go to next match.
    - `N`: to go to previous match.
- `{count}{command}` eg `3w`: `www`, `5j`: `jjjjj`.
- `gd`: go to definition of whatever is under your cursor.
- `gf`: go to files.
- `gg`: go to the top of the file.
- `G` : go to the end of the file.
- `%` : jump to match `([{}]).

