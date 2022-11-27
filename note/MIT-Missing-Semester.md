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

`pwd`: print work dictionary  
`cd` : go to path  
`ls` : show contents of current directory  
`ls -h`: with `-h` or `-help` will print help text  
`ls -l`: use a long listing format to show contents    
```ubuntu
(base) chen@ubuntu:~$ ls -l ~
总用量 48
drwxr-xr-x  2 chen chen 4096 11月 28 03:50 Desktop
drwxr-xr-x  3 chen chen 4096 11月 27 11:38 Documents
```
`drwxr-xr-x`: 
- `d` tell us `Desktop` is a dictionary
- `rwx` indicate permissions the owner of the file(dictionary)
    - `r` read
    - `w` write
    - `x` execute
    - `-` have no permissions
    - first `rwx` for user, second `rwx` for groups(with user), third `rwx` for everyone else  
`mv` : move or rename a file  
`cp` : copy   
`mkdir` : create a new dictionary  

## Connecting programs
`a>b` : use a instead b output steam
`a>>b`: let a add to b last  
`a<b` : use b instead a input steam
```ubuntu
ls /home > count.txt
# it will write home dictionary ls info into count.txt
cat < hello.txt > hello2.txt
# copy hello.txt contents to hello2.txt
```
`a|b` : let b use a's output as input(we call "|" pipes, it links left and right)
```ubuntu
ls -l | tail -n3
# ls -l will show all info, tail -n3 will show last 3 line info.
```
`sudo` : do something like a super user(root)