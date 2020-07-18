# 使用hexo搭建个人博客

## hexo 和 hugo 都是现在比较火的搭建博客的第三方框架

### 前期准备

1. [git](https://git-scm.com/)
2. [nodejs](https://nodejs.org/)

两者直接安装最新版本或者推荐版本即可。

### 开始搭建

- 因为hexo是基于nodejs框架的，所以搭建前必须要安装nodejs。

- 如果已经安装完毕 ，win+R 中输入`cmd`测试命令 `node -v`是否成功。

- 安装成功后要在命令行输入如下命令`npm -v` 测试npm（node package manager ）版本。

- 测试成功后说明nodejs的步骤已经搞定了。然后继续在命令行输入`npm install -g cnpm --registry=https://registry.npm.taobao.org`这是使用淘宝团队的cnpm镜像，完全可以代替npm使用。使用时需要将命令中的`npm`全部改成`cnpm`即可。

- cnpm安装完毕后要用命令`cnpm -v`来检验是否安装成功。

- 在进行hexo框架的安装，命令`cnpm install -g hexo-cli` `-g global全局安装 -cli Command Line Interface命令行界面` 。

- 安装完hexo使用`hexo -v`验证安装成功与否。

- 使用命令`mkdir blog`，在默认路径下建立一个文件夹blog，博主的默认路径是：`C:\Users\cyunt>`

- 注意：我们在这里建立了一个blog文件，也就是我们的博客文件，这之后的所有操作都是在这个文件上进行的，所以如果之后的操作出现的什么问题，就可以删除这个文件再重新做，目录下 `C:\Users\cyunt>del blog`这是删除这个文件的命令。

- 根据你的命令行的路径，可以在GUI界面找到你刚才建立的blog文件夹，（如果之后除了什么问题就del掉blog文件夹）windows用户之后可以用鼠标来操作文件。

- 用命令行继续操作，`cd blog`进入blog文件，`C:\Users\cyunt\blog>`此时是位于这个目录当中。

- 在blog路径下使用命令`hexo init`来初始化一个博客，hexo会自动生成一个博客。

  初始化完成会提示`INFO Start blogging with Hexo!`

  可以继续使用命令`dir`查看生成的文件。

- 继续启动博客使用命令`hexo s`

  启动成功后会出现如下提示，可以使用`http://localhost:4000`来查看本地4000端口号的博客样式，按`ctrl+c`断开本地服务，即退出。

  `INFO  Start processing`
  `INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.`

- 查看本地博客以后，如果你想要新建一篇博客。

  就可以在命令行输入`hexo n "hellogithub"` 

  之后出现提示`INFO  Created: ~\blog\source\_posts\hellogithub.md`

  之后在`source\_posts\`路径下就会生成一个`hellogithub.md`的markdown文件，windows用户可以用GUI找到这个文件用markdown软件去编辑你的博客啦~！**PS：**不会使用markdown的朋友可以去找教程看看

- 编辑完成后使用命令`hexo clean`运行完提示如下。

  `INFO  Deleted database.`
  `INFO  Deleted public folder.`

  **作用**是清除缓存文件 (`db.json`) 和已生成的静态文件 (`public`)。

  在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。

  然后继续使用。

  再继续使用命令`hexo g`重新生成博客，然后再`hexo s`启动本地在`http://localhost:4000`可以查看博客内容。

- 之前的操作就搞定了本地的博客部署，下一步就是将我们的博客给部署到互联网上，使它能够被访问。

- 登录github，新建一个代码仓库,然后Repository name*填 `你的昵称.github.io`，点击`Create repository`，这样就建立了一个代码仓库。

- 建立仓库之后再回到命令行，在blog目录下输入命令`cnmp install --save hexo-deployer-git`

  这是下载一个git部署插件，以便于我们之后将博客部署到github上。

- 找到之前的`C:\Users\cyunt\blog`路径下面有一个`_config.yml`文件，我们用记事本或者其他文本编辑工具(notepad++ or sublime text)打开，找到**文档最下面的**

  `#Deployment`

  `##Docs: https://hexo.io/docs/deployment.html`

  **修改代码如下**：

  `deploy:
    type: 'git'
    repo: https://github.com/0x522/0x522.github.io.git
    branch: master`

  注意：`type，repo，branch`后面需要加空格，`repo`就是你的代码仓库对应的https地址，建立仓库后会自动生成。

- 再将博客部署到github上，能够提供互联网访问，命令`hexo d`

  如果这一步失败，就先用git bash设置你的github的用户名和email

  设置步骤：

  1.打开git bash

  2.写入两个命令

  `git config -global user.name "你的用户名"`

  `git config -global user.email "你的email"`

  **注意name和email后面加空格**，这是将你的github与git绑定

  然后再运行`hexo d`

  如果提示让你输入用户名和email来确认你的身份。

  就继续在cmd命令行内输入这两个命令，然后再用命令`hexo d`

  `git config -global user.name "你的用户名"`

  `git config -global user.email "你的email"`

- github部署就完成了，赶快登录你的`https://你的github用户名.github.io`来访问你的博客吧~

  

#### 关于npm

当一个网站依赖的代码越来越多，程序员发现这是一件很麻烦的事情：

1. 去 jQuery 官网下载 jQuery
2. 去 BootStrap 官网下载 BootStrap
3. 去 Underscore 官网下载 Underscore
4. ……

有些程序员就受不鸟了，一个拥有三大美德的程序员 Isaac Z. Schlueter（以下简称 Isaaz）给出一个解决方案：用一个工具把这些代码集中到一起来管理吧！

这个工具就是他用 JavaScript （运行在 Node.js 上）写的 npm，全称是 Node Package Manager

###### 具体步骤：

NPM 的思路大概是这样的：

1. 买个服务器作为代码仓库（registry），在里面放所有需要被共享的代码

2. 发邮件通知 jQuery、Bootstrap、Underscore 作者使用 npm publish 把代码提交到 registry 上，分别取名 jquery、bootstrap 和 underscore（注意大小写）

3. 社区里的其他人如果想使用这些代码，就把 jquery、bootstrap 和 underscore 写到 package.json 里，然后运行 npm install ，npm 就会帮他们下载代码

4. 下载完的代码出现在 node_modules 目录里，可以随意使用了。

这些可以被使用的代码被叫做「包」（package），这就是 NPM 名字的由来：Node Package(包) Manager(管理器)。

#### 关于cnpm

- 因为npm安装插件是从国外服务器下载，受网络影响大，可能出现异常，所以我们乐于分享的淘宝团队干了这事。来自官网：”这是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。”
