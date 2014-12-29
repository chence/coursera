# Coursera 下载器

[![Build Status](https://travis-ci.org/coursera-dl/coursera.png?branch=master)](https://travis-ci.org/coursera-dl/coursera)
[![Coverage Status](https://coveralls.io/repos/coursera-dl/coursera/badge.png)](https://coveralls.io/r/coursera-dl/coursera)

[Coursera][1] 作为*大规模在线开放课程* (MOOC)的领先的领先者，截止到[2013年2月][13]有了精选自
62个不同机构的超过300多个课程。这些教育工作者和机构们在Coursera上慷慨的贡献使得许多人得到了他们原本得不到的出色的教育资源。

这个脚本使批量下载Coursera的课程资源（比如 视频，ppt等）更容易。提供一个或多个课程名称以及帐号信息，
它会获得周和课程名的提纲页，然后下载相关资料到合适命名的文件和目录。

为什么这个脚本是有帮助的？ 像[`wget`][2]的工具也能用，但是有这些限制：

1. 视频名称里包含数字，但是这不对应于实际的顺序。手动重命名这些文件是非常痛苦的。
2. 使用教学大纲页面提供的有更多信息的名字。
3. 使用wget循环得到视频中包含的未提交或未链接的额外视频时，时常会重复下载。

*下载全部* 是另一种可能, 但是这个脚本还提供了更多的特性比如适当的命名文件。

这项工作的最初灵感部分源自于[youtube-dl][3]——作者之前用于从其他地方比如可汗学院下载视频的工具。


## 特性

  * Intentionally detailed names, so that it will display and sort properly
    on most interfaces (e.g., MX Video, or [VLC][4] on Android devices).
  * 基于正则表达式的 section (week) and lecture 名称过滤器，使您仅下载需要的资源。
  * 通过文件扩展名过滤使您获得仅需的资源。
  * 允许通过命令行或者.netrc的方式进行登录认证。
  * 核心功能通过Linux，Mac和Windows平台的测试。


## 说明

`coursera-dl` 需要 Python 2 或者 Python 3 和 一个报名参加了感兴趣课程的免费Coursera帐号。
（目前／2014年5月，我们对该程序在Python 2.6, 2.7, 3.2, 3.3, 3.4和Pypy下执行自动化测试通过）

在任何的操作系统下，确保Python的执行目录添加到了你的PATH环境变量下，并且您已经安装了所有依赖（参考
下节），因为一个基础的用法，你需要在该项目的主目录下使用｀Python｀命令调用这个脚本。参考该文档的
“运行脚本”这一节，你可以使用更多关于这个脚本的高级特性。

*注意:* You must already have (manually) agreed to the Honor of Code of the
particular courses that you want to use with `coursera-dl`.

### 安装所有缺少的依赖包。

我们强烈推荐您考虑使用[`pip`][17]安装Python包，因为它是当前的[首选方法][18]。如果您使用`pip`，
你可以直接使用这个命令 `pip install -r requirements.txt` 安装所有需要的依赖包。

### 你自己安装依赖包

**警告：** 除非您知道要做什么否则这个方法是不被推荐的。在您提交bug之前，请检查您安装的模块和
`requirements.txt`文件中推荐的版本号是一致的。

您可以选择自己安装这些依赖，但是有用户出现过使用不同于 `requirements.txt` 中列出的版本号的包导致
下载到的资源不全的情况。

无论如何，您应该安装：

* [Beautiful Soup 4][5]: 必须的。请参见下面的 html5lib。
  - Ubuntu/Debian: `sudo apt-get install python-bs4`
  - Mac OSX + MacPorts: `sudo port install py-beautifulsoup4`
  - Other: `pip install beautifulsoup4`
* [Argparse][6]: 必须的。 （但是仅当您使用Python2.6的时候）
  - Ubuntu/Debian: `sudo apt-get install python-argparse`
  - Other: `pip install argparse`
* [requests][16]: 必须的。
  - Ubuntu/Debian: `sudo apt-get install python-requests`
  - Mac OSX + MacPorts: `sudo port install py-requests`
  - Other: `pip install requests`
* [six][19]: 必须的。
  - Ubuntu/Debian: `sudo apt-get install python-six`
  - Mac OSX + MacPorts: `sudo port install py27-six`
  - Other: `pip install six`
* [html5lib][15]: 不必须，但建议用来解析页面。
  - Ubuntu/Debian: `sudo apt-get install python-html5lib`
  - Mac OSX + MacPorts: `sudo port install py-html5lib`
  - Other: `pip install html5lib`
* [easy_install][7]: 仅在不使用预包装的依赖时是必须的。另外，PIP已经取代了它。
  - Ubuntu/Debian: `sudo apt-get install python-setuptools`

再次，请确保您安装的是`requirements.txt`文件中提到的版本号（更新的版本应该也可以）。

在Mac OSX下使用MacPorts，您应该用到下列命令：

    port
    > select --set python python27
    > install py-beautifulsoup
    > install py-argparse
    > install py-setuptools
    > install py-requests
    > install py27-six

### 创建一个Coursera的帐号

如果你还没有，创建一个[Coursera][1]的帐号并且报名参加一个课程。
查看课程列表 https://www.coursera.org/courses

### 运行脚本

运行这个脚本下载资料，需要提供您的Coursera帐号认证信息（如 邮箱和密码 或者 `~/.netrc`文件），课程名称，
以及一些其他的参数：

    一般:                     coursera-dl -u <user> -p <pass> modelthinking-004
    多个课程:                  coursera-dl -u <user> -p <pass> saas historyofrock1-001 algo-2012-002
    按section名称筛选:         coursera-dl -u <user> -p <pass> -sf "Chapter_Four" crypto-004
    按lecture名称筛选:         coursera-dl -u <user> -p <pass> -lf "3.1_" ml-2012-002
    仅下载ppt文件:             coursera-dl -u <user> -p <pass> -f "ppt" qcomp-2012-001
    使用~/.netrc文件:          coursera-dl -n -- matrix-001
    获取课程预览:               coursera-dl -n -b ni-001
    指定下载目录:               coursera-dl -n --path=C:\Coursera\Classes\ comnetworks-002
    显示帮助:                  coursera-dl --help

    维持课程列表在一个目录下：
      初始:              mkdir -p CURRENT/{class1,class2,..classN}
      更新:              coursera-dl -n --path CURRENT `\ls CURRENT`

    注意：如果你的`ls`命令是显示彩色输出的一个别名，您可能会遇到问题。
    一定要确保ls命令（在使用 \ls的时候）没有特殊字符被发送到脚本。

在\*nix平台下，使用 `~/.netrc`文件是每次在命令行上都要输入帐号（邮箱）密码的一个好的替代。
要使用它，只需在家目录（或者Windows下[类似的方法][8]）下一个名为 `.netrc`的文件添加
类似下面的一行内容：

    machine coursera-dl login <user> password <pass>

如果这个文件还不存在就创建一个。从此，您在调用cousera-dl时可以简单的使用-n来替代-u和-p了。
这是一个快捷方式，让你不再烦人的直接在命令行输入用户名和密码（尤其您在使用了一个很“强”的密码的时候）

**注意**： 如果你的密码包含逗号，引号或者其他“有趣的符号”（比如 `<`， ``，``，``，``等等），
那你必须在shell上对它们进行转义。与bash或其他许多其他s​​hell相比，更好的方式是把您的密码放在单引号里，
这样你就不会遇到问题。查看issue#213获得更多信息。

## 故障排除

如果您在下载课程资料时遇到问题，请尝试看看下列操作之一能否解决您的问题：

* 使用这个链接确保您的课程名称是拼写正确的
    `https://class.coursera.org/<CLASS_NAME>/class/index`

* 您是否尝试使用`--clear-cache`选项来清除您的cookie和帐号信息？

* 需要注意的是很多课程（可能是大多数的？）在课程结束后没多久可能会删除这些资料，也有其他课程会把资料
保留到下一次开课(to avoid problems with academic dishonesty, apparently).

  总之，它不能保证你一定能够下载完成后的课程，不幸的是，没有什么可以帮到您。

* 确保您已经安装了上述`requirements.txt`文件中提到的所有依赖包

* One can export a Netscape-style cookies file with a browser extension
  ([1][9], [2][10]) and use it with the `-c` option. This comes in handy
  when the authentication via password is not working (the authentication
  process changes now and then).

* 如果结果显示为0节，最有可能的是您提供的凭据（在命令行或在您的.netrc文件的用户名和/或密码）无效。

* 对于那些还没有开始，但已经有一个以前的迭代，有时预览可用，包含从上次当然，所有的类课程。这些文件可以通过传递-b参数下载。

* 如果您使用Beautiful Soup 4，请确保您已经安装了html5lib：

        $ python
        >>> import html5lib
        >>> print(html5lib.__version__)
        1.0b2

* If you get an error like `Could not find class: <CLASS_NAME>`:
    * Verify that the name of the course is correct. Current class
      names in coursera are composed by a short course name e.g. `class`
      and the current version of the course (a number). For example, for a
      class named `class`, you would have to use `class-001`, `class-002`
      etc.
    * Second, verify that you are enrolled in the course. You won't be
      able to access the course materials if you are not officially
      enrolled and agreed to the honor course *via the website*.

* If:
  - You get an error when using `-n` to specify that you want to use a
    `.netrc` file and,
  - You want the script to use your default netrc file and,
  - You get a message saying `coursera-dl: error: too few arguments`

  Then you should specify `--` as an argument after `-n`, that is, `-n --`
  or change the order in which you pass the arguments to the script, so that
  the argument after `-n` begins with an hyphen (`-`).  Otherwise, Python's
  `argparse` module will think that what you are passing is the name of the
  netrc file that you want to use. See issue #162.

## Filing an issue/Reporting a bug

When reporting bugs against `coursera-dl`, please don't forget to include
enough information so that you can help us help you:

* Is the problem happening with the latest version of the script?
* What operating system are you using?
* Do you have all the recommended versions of the modules? See them in
  the file `requirements.txt`.
* What is the course that you are trying to access?
* What is the precise command line that you are using (feel free to hide
  your username and password with asterisks, but leave all other
  information untouched).
* What are the precise messages that you get? Please, copy and paste them.
  Don't reword the messages.

## Feedback

I enjoy getting feedback. Here are a few of the comments I've received:

* "Thanks for the good job! Knowledge will flood the World a little more thanks
  to your script!"
  <br>Guillaume V. 11/8/2012

* "Just wanted to send you props for your Python script to download Coursera
  courses. I've been using it in Kenya for my non-profit to get online courses
  to places where internet is really expensive and unreliable. Mostly kids here
  can't afford high school, and downloading one of these classes by the usual
  means would cost more than the average family earns in one week. Thanks!"
  <br>Jay L., [Tunapanda][14] 3/20/2013

* "I am a big fan of Coursera and attend lots of different courses. Time
  constraints don't allow me to attend all the courses I want at the same time.
  I came across your script, and I am very happily using it!  Great stuff and
  thanks for making this available on Github - well done!"
  <br>William G.  2/18/2013

* "This script is awesome! I was painstakingly downloading each and every video
  and ppt by hand -- looked into wget but ran into wildcard issues with HTML,
  and then.. I came across your script.  Can't tell you how many hours you've
  just saved me :) If you're ever in Paris / Stockholm, it is absolutely
  mandatory that I buy you a beer :)"
  <br>Razvan T. 11/26/2012

* "Thanks a lot! :)"
  <br>Viktor V. 24/04/2013

## Contact

Post bugs and issues on [github][11]. Send other comments to John Lehmann:
first last at geemail dotcom or [@jplehmann][12]

[1]: https://www.coursera.org
[2]: http://sourceforge.net/projects/gnuwin32/files/wget/1.11.4-1/wget-1.11.4-1-setup.exe
[3]: https://rg3.github.com/youtube-dl
[4]: https://f-droid.org/repository/browse/?fdid=org.videolan.vlc
[5]: http://www.crummy.com/software/BeautifulSoup
[6]: http://pypi.python.org/pypi/argparse
[7]: http://pypi.python.org/pypi/setuptools
[8]: http://stackoverflow.com/a/6031266/962311
[9]: https://chrome.google.com/webstore/detail/lopabhfecdfhgogdbojmaicoicjekelh
[10]: https://addons.mozilla.org/en-US/firefox/addon/export-cookies
[11]: https://github.com/coursera-dl/coursera/issues
[12]: https://twitter.com/jplehmann
[13]: http://techcrunch.com/2013/02/20/coursera-adds-29-schools-90-courses-and-4-new-languages-to-its-online-learning-platform
[14]: http://www.tunapanda.org
[15]: https://github.com/html5lib/html5lib-python
[16]: http://docs.python-requests.org/en/latest/
[17]: http://www.pip-installer.org/en/latest/
[18]: http://python-distribute.org/pip_distribute.png
[19]: https://pypi.python.org/pypi/six/


[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/coursera-dl/coursera/trend.png)](https://bitdeli.com/free "Bitdeli Badge")
