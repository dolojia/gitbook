---

---

# gitbook写书及发布到github上

### 一、 安装node

检查node是否安装成功

```shell
$ node -v
v10.16.0
```

### 二、安装gitbook

1. 安装命令

   ```shell
   npm install gitbook-cli -g
   ```

2. 初始化项目(新建文件)

   ```shell
   $ gitbook init
   warn: no summary file in this book
   info: create README.md
   info: create SUMMARY.md
   info: initialization is finished
   ```

3. 启动服务，然后在浏览器地址栏中输入 `http://localhost:4000` 便可预览书籍

   ```shell
   $ gitbook serve
   Starting server ...
   Serving book on http://localhost:4000
   ```

4. 更多命令介绍

   ```shell
   gitbook build  #生成网页而不开启服务器
   
   ```

   

### 三、文件介绍

1. 使用`gitbook init`后会自动生成两个文件`README.md`和`SUMMARY.md`

   * `README.md`使用过`git`的都知道这个文件
   * `SUMMARY.md`就是自己要写文章**章节目录**

2. 系统文件目录如下

   ![gitbook001](./images/gitbook001.png)

3. 发布后网页样式

   ![gitbook002](./images/gitbook002.png)

### 四、通过配置文件来配置

1. 在书籍下面都可以创建一个`book.json`

   ```shell
   {
     "title": "标题",
     "author": "作者",
     "description": "简单描素",
     "language": "zh-hans", 
     "gitbook": "3.2.3", 
     "styles": {
       "website": "./styles/website.css" 
     },
     "structure": {
       "readme": "README.md" 
     },
     "links": {
       "sidebar": {
         "我的博客": "https://blog.csdn.net/kuangshp128" 
       }
     },
     "plugins": [ 
       "-sharing",
       "splitter",
       "expandable-chapters-small",
       "anchors",
   
       "github",
       "github-buttons",
       "donate",
       "sharing-plus",
       "anchor-navigation-ex",
       "favicon"
     ],
     "pluginsConfig": {
       "github": {
         "url": "https://github.com/kuangshp/"
       },
       "github-buttons": {
         "buttons": [{
           "user": "kuangshp",
           "repo": "mysql",
           "type": "star",
           "size": "small",
           "count": true
         }]
       },
       "donate": {
         "alipay": "./source/images/donate.png",
         "title": "",
         "button": "赞赏",
         "alipayText": " "
       },
       "sharing": {
         "douban": false,
         "facebook": false,
         "google": false,
         "hatenaBookmark": false,
         "instapaper": false,
         "line": false,
         "linkedin": false,
         "messenger": false,
         "pocket": false,
         "qq": false,
         "qzone": false,
         "stumbleupon": false,
         "twitter": false,
         "viber": false,
         "vk": false,
         "weibo": false,
         "whatsapp": false,
         "all": [
           "google", "facebook", "weibo", "twitter",
           "qq", "qzone", "linkedin", "pocket"
         ]
       },
       "anchor-navigation-ex": {
         "showLevel": false
       },
       "favicon": {
         "shortcut": "./source/images/favicon.jpg",
         "bookmark": "./source/images/favicon.jpg",
         "appleTouch": "./source/images/apple-touch-icon.jpg",
         "appleTouchMore": {
           "120x120": "./source/images/apple-touch-icon.jpg",
           "180x180": "./source/images/apple-touch-icon.jpg"
         }
       }
     }
   }
   ```

2. 关于`book.json`字段的介绍

* title: 书籍标题
* author:书籍作者
* description: 本书描述
* language:语言(中文设置 "zh-hans" 即可)
* gitbook:gitbook的版本
* styles:自定义样式
* structure: readme文件的位置(指定 Readme、Summary、Glossary 和 Languages 对应的文件名)
* links:链接跳转{在左侧导航栏添加链接信息}
* plugins:插件
* pluginsConfig:配置插件的属性

3. 插件介绍

   GitBook 有 [插件官网](https://link.jianshu.com/?t=https%3A%2F%2Fplugins.gitbook.com%2F)，默认带有 5 个插件，highlight、search、sharing、font-settings、livereload，如果要去除自带的插件， 可以在插件名称前面加 `-`，比如：

   ```bash
   "plugins": [
       "-search"
   ]
   ```

   如果要配置使用的插件可以在 book.json 文件中加入即可，比如我们添加 [plugin-github](https://link.jianshu.com/?t=https%3A%2F%2Fplugins.gitbook.com%2Fplugin%2Fgithub)，我们在 book.json 中加入配置如下即可：

   ```json
   {
       "plugins": [ "github" ],
       "pluginsConfig": {
           "github": {
               "url": "https://github.com/your/repo"
           }
       }
   }
   ```

   然后在终端输入 `gitbook install ./` 即可。

   如果要指定插件的版本可以使用 plugin@0.3.1，因为一些插件可能不会随着 GitBook 版本的升级而升级。

### 五、发布到github

1. 在git上创建两个项目

   * `gb-source` 存放整个gitbook文件(执行gitbook init命令的文件夹文件)
   * `gb-pages` 存放`gb-source\_book`文件夹文件用于发布展示，在git上发布为GitHub Pages

2. 设置GitHub Pages

   * 在gb-pages的Settings下配置

   

