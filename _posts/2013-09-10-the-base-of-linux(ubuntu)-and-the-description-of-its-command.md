---
layout: post
title: Linux(Ubuntu)基础及其基本命令介绍
categories: [Ubuntu]
tags: [Linux]
description: Linux 的灵感源自 1969 年就出现的 Unix 操作系统，时至今日该系统仍被广泛使用，并在不断发展中。 Unix 背后的许多设计惯例也同样存在于 Linux 中，对系统基本原理的理解至关重要。
---
{% include JB/setup %}
# Linux(Ubuntu)基础及其基本命令介绍

Linux 的灵感源自 1969 年就出现的 Unix 操作系统，时至今日该系统仍被广泛使用，并在不断发展中。 Unix 背后的许多设计惯例也同样存在于 Linux 中，对系统基本原理的理解至关重要。 Unix 最初主要使用命令行界面，这在 Linux 中也得到了保留。也就是说，图形用户界面及其窗口、图标、菜单等都构建在基本的命令行界面之上。更进一步，这也意味着在命令行里可以十分便捷的管理和访问 Linux 的文件系统。

##1 目录和文件系统

Linux 和 Unix 文件系统被组织成一个有层次的树形结构。文件系统的最上层是 <strong>/</strong>，或称为 <strong>根目录</strong>。在 Unix 和 Linux 的设计理念中，一切皆为文件——包括硬盘、分区和可插拔介质。这就意味着所有其它文件和目录（包括其它硬盘和分区）都位于根目录中。 例如：/home/jebediah/cheeses.odt 给出了正确的完整路径，它指向 cheeses.odt 文件，而该文件位于 jebediah 目录下，该目录又位于 home 目录，最后，home 目录又位于根(/) 目录下。 在根 (/) 目录下，有一组重要的系统目录，在大部分 Linux 发行版里都通用。直接位于根 (<strong>/</strong>) 目录下的常见目录列表如下：

:------|:----------
<strong>/bin</strong> | 重要的二进制 (binary) 应用程序
/boot | 启动 (boot) 配置文件
/dev | 设备 (device) 文件
<strong>/etc</strong> | 配置文件、启动脚本等 (etc)
<strong>/home</strong> | 本地用户主 (home) 目录
/lib | 系统库 (libraries) 文件
/lost+found | 在根 (/) 目录下提供一个遗失+查找(lost+found) 系统
/media | 挂载可移动介质 (media)，诸如 CD、数码相机等
/mnt | 挂载 (mounted) 文件系统
/opt | 提供一个供可选的 (optional) 应用程序安装目录
/proc | 特殊的动态目录，用以维护系统信息和状态，包括当前运行中进程 (processes) 信息。
/root | root (root) 用户主文件夹，读作“slash-root”
<strong>/sbin</strong> | 重要的系统二进制 (system binaries) 文件
/sys | 系统 (system) 文件
<strong>/tmp</strong> | 临时(temporary)文件
<strong>/usr</strong> | 包含绝大部分所有用户(users)都能访问的应用程序和文件
<strong>/var</strong> | 经常变化的(variable)文件，诸如日志或数据库等

##2 常用命令

<strong>1.</strong> 切换到 root 用户 ，输入“<strong>sudo -i</strong>”或“<strong>sudo su -</strong>”, 退出“<strong>exit</strong>”

<strong>2. pwd</strong> 显示当前目录， pwd = print working directory

<strong>3. ls</strong> 列出目录下当前文件

<strong>4. cp</strong> 复制文件/目录 cp (源文件或目录) (目标文件或目录)
{% highlight sh %}
cp -r  复制文件夹 包括子目录和文件
{% endhighlight %}

<strong>5. rm</strong> 删除文件/目录 可以删除文件
{% highlight sh %}
rm -rf  删除目录包含子目录和文件
rmdir  删除空文件夹
{% endhighlight %}

<strong>6. mv</strong> 移动或重命名 文件

<strong>7. cd</strong> 进入目录
{% highlight sh %}
cd / 		#进入根目录 

cd 或 cd ~ 	#进入用户的 home 目录

cd - 		#进入上次访问的目录 (相当于 back) 
cd ..  		#进入上级目录 
{% endhighlight %}

<strong>8. man</strong> 显示某个命令的 manual

<strong>9. df</strong> 显示文件系统空间信息
{% highlight sh %}
df -h  	#用 M 和 G 做单位显示文件系统空间信息 -h 意思是 human-readable 
{% endhighlight %}

<strong>10. du</strong> 显示目录的空间使用信息
{% highlight sh %}
du -sh /media/floppy	#-s 意思 summary  -h 意思 human-readable 
{% endhighlight %}

<strong>11. ifconfig</strong> 显示系统的网络

<strong>12. locate</strong>
命令会在您的计算机里搜索您指定的任意文件。它使用您系统中的文件索引以便进行快速查找：运行命令 updatedb 可以更新该索引。每天您一开机，该命令便会（在合适的时机）自动运行。运行该命令需要具备管理员权限.

<strong>13.</strong> Ubuntu卸载编译的软件
{% highlight sh %}
cd 源代码目录
make clean
./configure
make uninstall
{% endhighlight %}

<strong>14.</strong> 查找命令

<strong>1）find</strong>

find是最常见和最强大的查找命令，你可以用它找到任何你想找的文件。

<strong>find</strong>的使用格式如下：

　　<strong>$ find &lt;指定目录> &lt;指定条件> &lt;指定动作></strong>

　　- <指定目录>： 所要搜索的目录及其所有子目录。默认为当前目录。

　　- <指定条件>： 所要搜索的文件的特征。

　　- <指定动作>： 对搜索结果进行特定的处理。

如果什么参数也不加，find默认搜索当前目录及其子目录，并且不过滤任何结果（也就是返回所有文件），将它们全都显示在屏幕上。

find的使用实例：

　　<strong>$ find . -name 'my*'</strong>

搜索当前目录（<strong>含子目录</strong>，以下同）中，所有文件名以my开头的文件。

　　<strong>$ find . -name 'my*' -ls</strong>

搜索当前目录中，所有文件名以my开头的文件，并显示它们的详细信息。

　　<strong>$ find . -type f -mmin -10</strong>

搜索当前目录中，所有过去10分钟中更新过的普通文件。如果<strong>不加-type f</strong>参数，则搜索<strong>普通文件+特殊文件+目录</strong>。

<strong>2）locate</strong>

locate命令其实是"find -name"的另一种写法，<strong>但是要比后者快得多，原因在于它不搜索具体目录，而是搜索一个数据库（/var/lib/locatedb）</strong>，这个数据库中含有本地所有文件信息。Linux系统自动创建这个数据库，并且每天自动更新一次，所以使用locate命令查不到最新变动过的文件。为了避免这种情况，可以在使用locate之前，先使用<strong>updatedb</strong>命令，手动更新数据库。

<strong>locate</strong>命令的使用实例：

　　<strong>$ locate /etc/sh</strong>

搜索etc目录下所有以sh开头的文件。

　　<strong>$ locate ~/m</strong>

搜索用户主目录下，所有以m开头的文件。

　　<strong>$ locate -i ~/m</strong>

搜索用户主目录下，所有以m开头的文件，并且忽略大小写。

<strong>3）whereis</strong>

whereis命令只能用于<strong>程序名的搜索</strong>，而且只搜索二进制文件（参数-b）、man说明文件（参数-m）和源代码文件（参数-s）。如果省略参数，则返回所有信息。

<strong>whereis</strong>命令的使用实例：

　　<strong>$ whereis grep</strong>

<strong>4）which</strong>

which命令的作用是，在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。也就是说，使用which命令，就<strong>可以看到某个系统命令是否存在，以及执行的到底是哪一个位置的命令</strong>。

<strong>which</strong>命令的使用实例：

　　<strong>$ which grep</strong>

<strong>5）type</strong>

type命令其实不能算查找命令，它是用来区分某个命令到底是由shell自带的，还是由shell外部的独立二进制文件提供的。如果一个命令是外部命令，那么使用-p参数，会显示该命令的路径，相当于which命令。

<strong>type</strong>命令的使用实例：

　　<strong>$ type cd</strong>

系统会提示，cd是shell的自带命令（build-in）。

　　<strong>$ type grep</strong>

系统会提示，grep是一个外部命令，并显示该命令的路径。

　　<strong>$ type -p grep</strong>

加上-p参数后，就相当于which命令。(<strong>注意</strong>：如果是内置命令的话，没有输出)

　　<strong>$ type -P grep</strong>

<strong>-P</strong>参数代表强制搜索，此时内置命令也会有输出

<strong>15.</strong> vim编辑

a. <strong>ctrl+f</strong>下翻页(<strong>f</strong>orward)、<strong>ctrl+b</strong>上翻页(<strong>b</strong>ackwards)、<strong>M</strong>将光标移动的该页中部(<strong>M</strong>iddle)、<strong>gg</strong>回到文件顶部、<strong>G</strong>回到文件底部、<strong>hjkl</strong>移动光标

b. <strong>vim查找命令</strong>：命令模式下，按‘<strong>/</strong>’，然后输入要查找的字符，Enter。

c. 手动重加载文件的命令是 <strong>:e!</strong> (以前是退出编辑重新打开，用这个的话方便多了)

d. <strong>清空文件内容</strong>(3种)：在命令模式下，首先执行gg，这里是跳至文件首行，再执行dG，这样就清空了整个文件！
还有一种方法就要退出VIM，然后使用echo >> file。最后一种是删除了这个文件再新建一个就是了

<strong>16.</strong> 查看当前时间
{% highlight sh %}
test@lym406365619:~$ date
Tue Jan 20 10:22:53 CST 2014
{% endhighlight %}

<strong>17.</strong> Linux时间同步 -- <strong>ntpdate</strong>
{% highlight sh %}
sudo ntpdate -u pool.ntp.org
#pool.ntp.org是一组授时服务器虚拟集群
{% endhighlight %}

时间同步之后，如果没有设置时间区域，默认的时区是UTC。把时区改成北京时间，执行这面的命令：
{% highlight sh %}
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
{% endhighlight %}

<br />

##参考
[http://wiki.ubuntu.org.cn](http://wiki.ubuntu.org.cn/?title=Ubuntu%E6%A1%8C%E9%9D%A2%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97)