---
title: 使用githubPage和Jekyll搭建自己的博客
date: 2018-03-12 00:30:04
categories:
- Tech
---
嗨，欢迎回来w  

今天来回顾一下这个小东西是怎么产生的，包括但不限于操作步骤、参考的教程和资源，走过的弯路等等，以便自己梳理流程、总结经验，顺便复习一下web的知识（此处向王青老师致敬）。如果你具有基本的前后端方面的知识会更加方便理解，因为本文很可能不会对诸如“静态网站”“动态网站”等等概念进行阐述。当然还是看这个不靠谱作者的心情而定了。  
另外，因为我习惯于在教程中见到陌生的术语就去搜索一下，所以这些粗浅的对于计算机网络的理解也会出现在本文中。局限于自己的知识水平，如果有哪里理解不当的地方欢迎指点和讨论（毕竟这门课大三才学嘛）（借口）。

# 简述
既然建立独立博客是建站的一种，那么购买及解析域名、选择服务器和前端技术等等都是不能绕开的话题了。  

主流方案约莫有两种：一是购买云服务器（主机/空间）搭配[WordPress](https://cn.wordpress.org/)食用，二是使用Github提供的[GithubPage](https://pages.github.com/)搭配[Jekyll](https://jekyllrb.com/)。  

WordPress是一个使用PHP语言和MySQL数据库开发的博客平台，用户（也就是我）可以在支持这两者的服务器上架设网站。它的优势在于扩展性非常强大，拥有及其丰富的模板和插件，如果你重视这一点并且不介意在服务器上花费的成本，那么我比较推荐使用WordPress（虽然很可惜这篇博文并不能帮助到你）。顺便一提许多WordPress服务需要科学上网，也因此上方链接放的是国内可以访问的中文简体版本。  

由于GithubPage实际上使用了github的代码和服务器搭建的网站，github仅支持上传静态网页而不能在上面创建真正的博客系统。Jekyll是一个生成静态网页的引擎，不需要数据库支持。只要上传符合Jekyll规范的文件，github能够使用这种生成引擎为你转化出静态的页面和网站。  

我选择Jekyll来部署博客主要是因为它
* 轻量级的网站框架，避免了动态网站的笨重
* 不需要自己购买或者搭建服务器
* 使用Markdown标记语言写作，无缝接轨Sublime Text3（还有比这更棒的码字体验么w）  

Jekyll也有许多开源的模板，简洁清爽的风格真是深得我心。同时它本身并不支持的评论系统、数据统计与分析服务等等动态部分可以通过引入第三方服务来扩展。  

总之，就是它啦。

当然你也可以使用其他Web框架了，甚至是从自己搭建服务器开始到网页设计一手包办（听起来就非常geek），但是建站的基本要素是万变不离其宗的，总之希望大家都顺利w。

# 安装过程
## 购买和绑定独立域名
推荐[Godaddy](https://sg.godaddy.com/)，支持支付宝，就像普通的网购一样注册证号、选择域名、支付即可，不同域名后缀费用不等。优惠码随便一搜的都可以用，优惠力度不等。  
域名的选择尽量跟自己相关并且不要过长。完美主义如我还查了一下用什么后缀比较好，记得有篇博文写得特别详细但我找不到了，大意就是首推.com/.cn/.org，个人网站的话使用.me/.site/.website/.space也ok。我是比较心水.me啦但是.space便宜啊（无辜脸）。  进入购物车结算的时候记得勾选“不用”，支付之后提示购买成功的邮件可能会有所延迟。

据说Godaddy提供的域名解析服务器（默认值）被墙掉了，需要把域名解析服务迁移到国内比较稳定的服务商处，此处选择[DNSPod](https://www.dnspod.cn)：
- DNSPod界面：首先添加域名记录，就是把购买的域名扔进去。
- DNSPod界面：在自己的域名下添加一条A记录，地址填写Github Page的服务IP地址：192.30.252.153
![Markdown](http://i2.bvimg.com/618639/213e66166ca2e4d0.png)
- Godaddy界面：我的产品->DNS->域名服务器（NameServer）修改为dnspod的两个地址：f1g1ns1.dnspod.net、f1g1ns2.dnspod.net。
![NameServer](http://i2.bvimg.com/618639/d466a1918d86fdb0s.png)

如果依旧不清楚如何操作请查看DNSPod提供的[Godaddy注册商域名修改DNS地址](https://support.dnspod.cn/Kb/showarticle/tsid/42/)。狗爹会有邮件通知域名生效，可能需要等上半天到一天的时间。等待期间我们可以开始配置github了。


## 配置和链接Github
假设我已经有了一个Github（是的我已经有了），接着假设我已经在本地安装了Git并且熟悉了基本的push/clone/add/commit（是的我已经会了），那么就愉快的开始配置SSH吧。

进入Git Bash。
1. 检查SSH keys的设置
```
$ cd ~/.ssh
```
如果显示"No such file or directory", 跳过第二步。
2. 备份和移除员原来的ssh keys的设置
```
$ ls
config  id_rsa  id_rsa.pub  known_hosts
$ mkdir key_backup
$ cp id_rsa* key_backup
$ rm id_rsa*
```
3.生成新的SSH Key
输入下面的代码，`xxx@youremail.com`内是你自己的邮箱
```
$ ssh-keygen -t rsa -C "xxx@youremail.com"
```
当反馈如下信息，直接回车
```
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):<此处回车就好>
```
接着会要你输入passphrase并确认
```
Enter passphrase (empty for no passphrase):<输入加密串>
Enter same passphrase again:<再次输入加密串>
```
最后看到一段英文文字之后跟了一副randomart image就设置完成了。文字会包括以下部分，提示你id_rsa.pub文件位置：
```
Your identificaiton has been saved in 
/Users/your_user_directory/.ssh/id_rsa.
Your public key has been saved in
/Users/your_user_directory/.ssh/id_rsa.pub
```

4. 给Github添加SSH Key
进入id_rsa.pub文件位置，用文本编辑工具打开该文件（注意不是和它放在一起的文本文件），Ctrl+A和Ctrl+C。  
在Github上进入Settings，侧栏进入"SSH and GPG keys"，在Key一栏Ctrl+V，点击Add Key即可。图片来自网络。![Markdown](http://i2.bvimg.com/618639/63113c04bf6e6cc3.jpg)

5. 测试
输入以下命令测试设置是否成功，其中`git@github.com`不要修改。
```
$ ssh -T git@github.com
```
如果出现如下反馈，输入`yes`即可。
```
The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?
```
6. 设置账号信息
这一步主要是完善个人信息，输入以下代码
```
$ git config --global user.name "你的名字"
$ git config --global user.email "your_email@youremail.com"
```
*设置token*  
token是工具们除了设置SSH之外另一种链接Github的方法。据说新版的接口已经不需要配置token了于是我跳过了这一步。但是在后来使用Jekyll模板的过程中出现了错误，说明还是有比较旧的API是使用token的。参见[问题2](#problem_token)。

## 建立博客仓库
GitHub Pages分两种，一种是你的GitHub用户名建立的`yourusername.github.io`这样的 *User & Organization Pages* ，另一种是依附项目的pages。建立个人博客使用的是前者，并且形如`yourusername.github.io`这样的可访问网站，每个github用户名下只能建立一个。

1. 在github中建立一个名为`yourusername.github.io`的仓库。提交一个`index.html`文件（不提交不让生成的页面的）（废话）。然后`push`到该仓库的`master`上。第一次提交等待约十分钟至页面生效，访问`yourusername.github.io`就可以看到上传的页面了。
2.绑定域名
希望用户可以通过我自(hua)己(qian)的域名来访问，需要在仓库的根目录下新建一个名为`CNAME`的文件并把域名扔进去：
```
wuyq53.space
```
据说根据DNS的情况，最长可能需要一天才能生效。
![Markdown](http://i2.bvimg.com/618639/9da85833ed5cfeb9.png)

## 使用Jekyll模板
如前文所述，Jekyll是一个文本转换引擎，用你最顺手的标记语言写文档（Markdown/Html/Textile，甚至可以混用），引擎负责通过`layout`将文档拼装起来。因此它的基本文件架构非常重要并且比较严格。

~~但是作为一个打算使用模板的人何须关心这些呢~~

使用Jeckll模板非常简单。  
1. 将Github上是仓库`yourusername.github.io`clone到本地；
2. [Jekyll Themes](http://jekyllthemes.org/)选择顺眼的模板下载下来；
3. 将压缩包解压至1中的文件夹`yourusername.github.io-master`，若有index.html冲突，选择“复制和替换”；
4. 通过如下git基本命令更新仓库。进入Github主页查看是否已经更新，等待几分钟通过域名访问博客，如果看到出现对应模板，则成功。当然了你这么聪明肯定可以自行查看git命令手册一类的。
```
$ git add -A
$ git commit -m "text motified"
$ git push
```
5. 在根目录下的`_config.yml`内修改各种参数来更改信息。在文件夹`_post`添加命名格式为`yyyy-mm-dd-title.md`的markdown文件。（具体的很多博客里都有写了，参看[附录](#sources)）。再次更新仓库。

我使用的模板是[**NextT**](http://theme-next.simpleyyt.com/)。有许多人贡献了自己的代码以至于现在它已经是一个相对来说非常成熟的模板，包括第三方服务等等。所以如果你选择的模板里不包括这些资源，动手配置一下也是非常棒的呢。

## 不使用Jekyll模板
这就涉及到本地的Jekyll环境配置了，包括安装Ruby、gem等等。虽然最后blog里并没有用到但是我还是都尝试了一遍（虽然是因为大多数教程都混在一起说所以我没有理解好模板的用途什么的）。  
更待续新...

# 可能出现的问题·
~~其实是我遇到过已经出现过的问题了。~~
1. Github Page的服务IP地址是会有变动的，如果你看到这篇文章的时候博客不能登录，请查看[GitHub Pages](https://help.github.com/articles/troubleshooting-custom-domains/#dns-configuration-errors)使用最新IP。
<p id="problem_token"></p>
2. 在用命令 `jekyll server`测试模板的时候出现如下错误
```
GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.
```
**解决方法** 在系统环境变量中添加 JEKYLL_GITHUB_TOKEN 和 SSL_CERT_FILE（非代理访问Github且非MacOS）  
[Fix the errors of GitHub Metadata and SSL certificate when running Jekyll serve](https://www.hieule.info/programming/fix-errors-github-metadata-ssl-certificate-running-jekyll-serve)

3. 分清楚本地环境下使用Jekyll文件的和提交到Github上的区别。

4. 持续更新...

# 一些理解
域名（Domain Name）简而言之就是访客在浏览器url的输入框内输入进去就能访问到我的博客。它在网域名称系统中（DNS，Domain Name System）中和IP地址相互映射。我的理解是，用户向浏览器提供域名，浏览器根据绑定在域名上的域名服务器地址把域名提交过去，域名服务器解析域名即在数据库中找到与之映射的IP地址，IP地址所指向的计算机（服务器）响应请求。

# 后记
很久没有这样输出内容了，因为平时的写的东西只给自己看的时候总是会整出一些奇奇怪怪的表达，或者想到哪写到哪，所以好歹是第一篇总是会特别谨慎的吧。虽然还是免不了全角半角混用。

也会很担心一篇教程向的分享到底多细致才比较合适。有时候不免觉得自己在解释概念、叙述步骤或者插混打科方面实在太啰嗦了一点，而可能该弄清楚的反而觉得以后再说。但还是希望自己走过的弯路别人不要再走了，同时也主要是整理自己的心得供自己以后参考。

写到最后还有些累了，明天还有操作系统的早课，Jekyll的post使用、本地的Jekyll环境配置、问题总结等等都只能择日再叙了，非常抱歉。

总之谢谢你看到这里w

# 教程和资源
<p id="sources"></p>
1. [使用Github Pages建独立博客](http://beiyuu.com/github-pages)
2. [在 Windows 上搭建本地 Jekyll 编译环境时问题汇总](http://blog.csdn.net/wudalang_gd/article/details/74619791)
3. [Kresnik的随想和技术空间-在github pages网站下用jekyll制作博客教程](http://kresnik.wang/works/tech/2015/06/07/%E5%9C%A8github-pages%E7%BD%91%E7%AB%99%E4%B8%8B%E7%94%A8jekyll%E5%88%B6%E4%BD%9C%E5%8D%9A%E5%AE%A2%E6%95%99%E7%A8%8B.html)
4. [如何搭建一个独立博客——简明 GitHub Pages与 jekyll 教程](http://www.cnfeat.com/blog/2014/05/11/how-to-build-a-blog/)
5. [一步步在GitHub上创建博客主页(1)](http://www.pchou.info/ssgithubPage/2013-01-03-build-github-blog-page-01.html)
6. [Jekyll模板的使用](http://blog.csdn.net/hezy94/article/details/78369551)
6. [解决Hexo博客中 Disqus 在国内不能访问的方案](https://www.jianshu.com/p/9cc4cc8628c9)
7. [在Jekyll博客添加评论系统：gitment篇](https://www.jianshu.com/p/2940e0eda89f)
8. [为NexT主题添加文章阅读量统计功能](https://notes.wanghao.work/2015-10-21-%E4%B8%BANexT%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E9%98%85%E8%AF%BB%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%8A%9F%E8%83%BD.html#%E9%85%8D%E7%BD%AELeanCloud)
5. 图床[贴图库](http://www.tietuku.com/)
6. [Ruby镜像](https://gems.ruby-china.org/)
