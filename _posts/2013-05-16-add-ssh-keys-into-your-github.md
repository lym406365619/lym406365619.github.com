---
layout: post
title: Github项目添加SSH公钥
categories: [Github]
tags: [git, ssh]
description: 在使用git clone命令时，如果你使用的操作系统未在Github网站上添加SSH公钥时，比如拷贝jekyll-bootstrap项目，使用命令git clone git@github.com:plusjade/jekyll-bootstrap.git。系统会报如下错误
---
{% include JB/setup %}

#Github项目添加SSH公钥

##1 测验SSH公钥是否添加到Github账户中
在使用<code>git clone</code>命令时，如果你使用的操作系统未在Github网站上添加SSH公钥，比如拷贝[jekyll-bootstrap](http://jekyllbootstrap.com)项目，使用命令<code>git clone git@github.com:plusjade/jekyll-bootstrap.git</code>。系统会报如下错误：
<pre class="command-line">
    <span class="command">Permission denied (publickey).</span>
    <span class="command">fatal: The remote end hung up unexpectedly</span>
</pre>
或者你可以使用<code>ssh</code>命令连接github.com的SSH服务，登录用户名为git（所有GitHub用户共享此SSH用户名，不要写成其他），测试如下
<pre class="command-line">
    <span class="command">$ ssh -T git@github.com</span>
    <span class="command">Permission denied (publickey).</span>
</pre>
上面的例子显示登录失败，这是因为我们还未在GitHub账户中添加SSH公钥。这时需要在本地创建SSH公钥，然后将生成的SSH公钥的文件内容添加到Github帐号上去。

##2 生成SSH公钥
创建SSH公钥的方法很简单，执行如下命令：
<pre class="command-line">
    <span class="command">$ ssh-keygen -t rsa -C "<em>your_email@youremail.com</em>"</span>
</pre>

执行这条命令会提示文件保存路径，press Enter；

然后提示输入passphrase（密码），输入两次（可以不输直接两次Enter）；

然后会在.ssh目录生产两个文件：<code>id_rsa</code>和<code>id_rsa.pub</code>。

其中~/.ssh/id_rsa是私钥文件，~/.ssh/id_rsa.pub是公钥文件。注意私钥文件要严加保护，不能泄露给任何人。如果在执行<code>ssh-keygen</code>命令时选择了使用口令保护私钥，私钥文件是经过加密的。至于公钥文件~/.ssh/id_rsa.pub则可以放心地公开给他人。

##3 添加SSH公钥
使用文本编辑工具打开公钥文件~/.ssh/id_rsa.pub，可以使用<code>gedit</code>、<code>cat</code>、<code>vim</code>等编辑器，这里我使用<code>gedit</code>打开:
<pre class="command-line">
    <span class="command">$ gedit ~/.ssh/id_rsa.pub</span>
</pre>

接着拷贝<code>id_rsa.pub</code>文件内的所有内容，粘贴到github帐号管理中的添加SSH界面中。登录你的Github帐号后

  1） 点击右上方的Accounting settings图标

  2） 选择 SSH keys

  3） 点击 Add SSH key
 
  4） 在Title里填一个你自己喜欢的名称，然后将上面拷贝的~/.ssh/id_rsa.pub文件内容粘帖到Key中
  
  5） 点击“add key”按钮

如下图所示：

<img src="/img/blog/git_ssh.png" class="pic"></img>

添加过程github会提示你输入一次你的github密码。

##4 测试SSH连接Github
使用如下代码测试，结果如下：
<pre class="command-line">
    <span class="command">lym@lym-desktop:~$ ssh -T git@github.com</span>
    <span class="command">Hi lym406365619! You've successfully authenticated, but GitHub does not provide shell access</span>
</pre>
如果出现上述，则表明已经SSH设置成功了。

##5 设置git用户名与邮箱
打开电脑终端，首先设置你的用户名：
<pre class="command-line">
    <span class="command">$ git config --global user.name "<em>Your Name Here</em>"</span>
    <span class="comment"># Sets the default name for git to use when you commit</span>
</pre>
之后设置你的邮箱：
<pre class="command-line">
    <span class="command">git config --global user.email "<em>your_email@example.com</em>"</span>
    <span class="comment"># Sets the default email for git to use when you commit</span>
</pre>

注意：你的邮箱地址必须跟你其中的一个Github账户有关。如果不是，请点击[这里](https://help.github.com/articles/how-do-i-change-my-primary-email-address)，在你的Github账户中添加邮箱，并且也可以通过上述链接隐藏你的邮箱。
