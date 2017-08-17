# **js文件和css文件gulp压缩打包**

## 一、node本地环境搭建和gulp模块安装

### 1、Node本地环境搭建

Gulp模块是基于node环境来运行，首先要在本地搭建node环境，下面是基于window系统来描述的，如果采用mac版和linux系统来安装部署的请自行百度。

#### 1.1、安装包下载

官网中下载msi安装包[https://nodejs.org/en/download/](https://nodejs.org/en/download)  ,官网中介绍两种版本：LTS和Current,LTS为稳定版。Current为当前版，后期更新频繁。建议新用户下载current，LTS是专门做项目来安装的。

#### 1.2、安装步骤

双击下载好的安装包，然后点下一步下一步一直执行。最后点击finish，这里相对简单，就不截图展示了

#### 1.3、检验结果

检验安装成功的结果是在window 输入cmd 弹出命令窗口，输入node –-version 和npm –v查看

![](/assets/import1.png)

### 2、gulp模块安装

npm是node 的包管理工具，检测npm是否安装成功cmd 弹出命令窗口输入npm –v 。可以利用它来安装gulp所需的包。由于国内有墙，npm安装的包的过程中有时候会非常缓慢，建议先安装淘宝的镜像npm的管理包cnpm  官网地址：[https://npm.taobao.org/](https://npm.taobao.org/)

![](/assets/import.png)

#### 2.1、全局安装gulp

![](/assets/1import.png)

#### 2.2、创建临时环境

这里的项目环境不是指你正式项目的环境，最好在桌面临时建个。执行cnpm install gulp --save-dev 这个时候你临时建立的运行环境中就多出了一个node\_modules文件夹，这就是存放node模块的地方。

**注意：看下node\_modules是否有gulp-uglify和gulp-minify-css模块，如果没有执行下：**

**cnpm install gulp-uglify**

**cnpm install gulp-minify-css**

![](/assets/import2.png)

## 二、gulp压缩js和css

在项目的根目录下新建gulpfile.js，里面写入此代码

var gulp = require\('gulp'\),  //引用gulp的模块

```
minifycss = require\('gulp-minify-css'\),  //引用gulp css压缩的模块

uglify = require\('gulp-uglify'\),   //引用gulp js压缩的模块
```

## 1、js压缩

还是在gulpfile.js 后面写入：

gulp.task\('script', function\(\) {

```
// 1. 找到js文件  \*.js 代表js文件夹中所有js文件

gulp.src\('js/\*.js'\)

// 2. 压缩文件，uglify\(\)要对应上面模块的名称引入一致

    .pipe\(uglify\(\)\)

// 3. 另存压缩后的文件，根目录下

    .pipe\(gulp.dest\('dist/js'\)\)
```

}\)

原始js文件：

![](/assets/2.png)

![](/assets/1.png)

**注意：最后在dist/js  看的是压缩后的文件  但是名称还是和没压缩的一致，最好重命名下min.js**

## 2、css压缩

知道会压缩js文件，css操作就会相对简单的多。

gulp.task\('css', function\(\) {

```
gulp.src\('css/\*.css'\)
```

// 2.这里的方法名和js不一样。

```
.pipe\(minifycss\(\)\)

    .pipe\(gulp.dest\('dist/css'\)\)
```

}\)

在当前项目根目录下cmd弹出的窗口中执行gulp css

步骤截图就不做了，跟上面的js一样。

