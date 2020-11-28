# Hugo搭建个人博客


# 使用 Hugo 搭建个人博客

## 前期准备工作

1.  下载 [hugo](https://gohugo.io/)，查看官方文档根据步骤下载。
2.  测试 hugo 能否运行，在 gitbash 或者 cmd 中写入命令 `hugo version`，出现 `Hugo Static Site Generator v0.74.1-15163266 `类似的字样说明你安装成功。

## 创建你的项目

- Hugo 提供了一个 new 命令来创建一个新的网站:你应当在一个安全的目录下运行以下命令。
  - `hugo new site my_website`
  - `cd my_website `

## 初始化一个本地 git 仓库

- `git init`

## 为博客选择一个主题

- `git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke`
- 利用上面一行命令下载一个主题，如果有需要请访问[hugo 主题页面](https://themes.gohugo.io)令行下载其他主题。
- `echo 'theme = "ananke"' >> config.toml` 添加主题到配置文件。

## 添加一些内容

- `hugo new posts/my-first-post.md`，这个操作是在 content\posts 目录下建立一个你自己的 markdown 文件，默认都是在 posts 下添加.md 文件，hugo 就是利用这些.md 文件来作为博客静态页面的内容。
- ```
   ---
   title: "My First Post"
   date: 2019-03-26T08:47:11+01:00
   draft: true
   ---
   ## 里面的内容可以按照格式修改。
   ## draft 参数表示是否被部署 true则不会被直接部署。
  ```

## 启动 hugo 本地服务器

- `hugo server -D` 或者 `hugo server`
- ```
                           | EN
   +------------------+----+
   Pages            | 10
   Paginator pages  |  0
   Non-page files   |  0
   Static files     |  3
   Processed images |  0
   Aliases          |  1
   Sitemaps         |  1
   Cleaned          |  0

   Total in 11 ms
   Watching for changes in /Users/bep/quickstart/{content,data,layouts,static,themes}
   Watching for config changes in /Users/bep/quickstart/config.toml
   Environment: "development"
   Serving pages from memory
   Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
   Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
   Press Ctrl+C to stop
  ```

## 自定义主题

您的新站点看起来已经不错了，但是在将其发布给公众之前，您需要对其进行一些调整，只需要配置 config.toml 文件。

## 建立静态页面

- `hugo -D`或者`hugo`
- 将会在你的站点目录下输出一个 public 目录，这个就是利用 hugo 生成的静态页面。
- 之后会在 public 重新建立一个本地 git 仓库，与 github 上面一个对应的仓库关联。
- 每新写一篇博客就利用`hugo`命令重新生成一下 public 目录，也可以说是更新 public 目录。

## 连接到 github

- 首先你需要在 github 建立一个仓库，名字为(0x522.github.io)，0x522 是作者的用户名，这里需要该为你自己的。<br>
  运行命令
- `git remote add origin 你仓库的ssh或https链接` 这样就把 public 的本地仓库和你的 github 中的 repo 关联。
- 在 public 目录下
  - `git add .`
  - `git commit -m 你要备注的内容`
  - `git push -u origin master` 这是第一次 push 的写法，以后都直接 <br>
    `git push` 即可。
  - 每次更新博客时，重复以上三条操作，把更新推送到 GitHub。

## 搭建完成，快去丰富你的博客吧！

通过访问仓库的域名就可以直接访问你的博客啦！

