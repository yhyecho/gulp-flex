## 一. 用Github托管网站
#### 1. 搭建个人／组织网站
* [pages地址](https://pages.github.com/)
* github允许我们创建个人/组织网站 和 项目网站
* 创建个人网站步骤
  1. 创建和自身用户名相同的仓库，eg: yhyecho.github.io
  2. 提交代码到当前仓库的主分支上
  3. 访问 [地址](https://yhyecho.github.io/) 查看效果
#### 2. 搭建Github项目站点
* 搭建项目站点步骤
  1. 新建任意项目flex
  2. 本地新建gh-pages分支并推送至远程仓库
  3. 在setting设置gh-pages为默认分支
  4. 访问 [地址](https://yhyecho.github.io/flex)查看效果
#### 3. coding.net托管页面
* 创建个人和项目主页 [地址](https://coding.net/help/doc/pages/index.html)

## 二. 安装nodejs和gulp
#### Nodejs为何学？如何学
* 主要学习node生态系统 [官网](https://nodejs.org/en/)
* npm和gulp的学习
  1. npm包管理工具 [官网](https://www.npmjs.com/)
  2. gulp前端自动化构建工具 [官网](http://gulpjs.com/)
* 动手，从实际项目中学习node
#### Nodejs安装
* nvm安装nodejs(安装方便，支持多版本管理)
  1. 安装nvm
  ```
  curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.29.0/install.sh | bash
  ```
  2. 查看都有哪些版本可以安装：
  ```
  nvm ls-remote
  ```
  3. 安装指定版本
  ```
  nvm install v5.10.1
  ```
  4. 设置默认版本
  ```
  nvm alias default 5.10.1
  ```
  5. 让 npm 使用淘宝镜像[地址](http://npm.taobao.org/)
  ```
  echo 'alias cnpm="npm --registry=https://registry.npm.taobao.org \
  --cache=$HOME/.npm/.cache/cnpm \
  --disturl=https://npm.taobao.org/dist \
  --userconfig=$HOME/.cnpmrc"' >> ~/.zshrc && source ~/.zshrc
  ```
* 安装构建工具gulp
  1. 全局安装gulp
  ```
  npm i -g gulp
  ```
  2. 查看gulp安装路径
  ```
  which gulp
  ```
#### gulp构建系统
* [视频链接](http://haoduoshipin.com/v/178)
#### 使用gulp-sass
* sass是css的一种扩展(更简单高效的书写css)
* 使用sass步骤
  1. 全局安装gulp
  ```
  cnpm i -g gulp
  ```
  2. 新建项目文件夹
  ```
  mkdir project
  cd project
  ```
  3. node项目初始化
  ```
  cnpm init -y
  ```
  4. 项目中安装gulp并保存进package.json中
  ```
  cnpm i -D gulp # -D 等价于 --save-dev
  ```
  5. 安装 gulp-sass
  ```
  cnpm i -D gulp-sass
  ```
  6. 进入项目目录，编写gulpfile.js文件
  ```js
  var gulp = require('gulp');
  var sass = require('gulp-sass');

  gulp.task('sass', function() {
    gulp.src('src/main.scss')
      .pipe(sass())
      .pipe(gulp.dest('dist/'));
  });
  ```
  7. 打开命令行，进入项目目录运行gulp task, 得到输出结果
  ```
  gulp sass
  
  这样每次运行 gulp sass 就可以把 main.scss 文件，编译输出为 main.css 了。
  ```

## 三. gulp的七十二变
#### sass的三大功能 [地址](https://www.sass.hk/guide/)
* 使用materialdesign取色盘[地址](https://www.materialpalette.com/pink/red)
  1. primary color (主色)
  2. accent color(强调色)
* 变量的使用
```scss
$nav-color: #F90; // 变量定义
nav {
  $width: 100px;
  width: $width;
  color: $nav-color;
}

//编译后
nav {
  width: 100px;
  color: #F90;
}
```
* 语句的嵌套
```scss
.hero {
  padding: 80px;
  .btn {
    width: 100px;
    margin: 20px auto;
    &:hover {
      background-color: lighten($nav-color, 10%);  // 使用函数 
      cursor: pointer;
    }
  }
}
```
* 文件的拆分
  1. 新建_common.scss
  2. 新建_sections.scss
  3. 在main.scss中导入所需要的样式
  ```scss
  @import "common";
  @import "sections";
  ```
* gulp构建多任务
```js
var gulp = require('gulp');
var sass = require('gulp-sass');

gulp.task('sass', function() {
  gulp.src('src/main.scss')
      .pipe(sass())
      .pipe(gulp.dest('dist/'));
});

gulp.task('copy-assets', function() {
  gulp.src('src/*.html')
      .pipe(gulp.dest('dist/'));
});

gulp.task('default', ['sass', 'copy-assets']);
```
#### autoprefixer 添加厂商前缀
* 在线工具[地址](http://autoprefixer.github.io/)
* autoprefixer 是基于 [caniuse.com](http://caniuse.com/) 的规则来添加厂商前缀的
* 在gulp中使用autoprefixer
  1. 安装gulp-autoprefixer
  ```
  cnpm i -D gulp-autoprefixer
  ```
  2. 更新gulpfile.js文件
  ```js
  var gulp = require('gulp');
  var sass = require('gulp-sass');
  var prefix = require('gulp-autoprefixer');

  gulp.task('sass', function(){
    gulp.src('src/main.scss')
        .pipe(sass())
        .pipe(prefix()) // 截住管道流, 添加厂商前缀
        .pipe(gulp.dest('dist/'));
  });

  gulp.task('copy-assets', function(){
    gulp.src('src/*.html')
        .pipe(gulp.dest('dist/'));
  });

  gulp.task('default', ['sass', 'copy-assets']);
  ```
  3. 代码测试
  ```scss
  display: flex;
  // 添加后
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  ```

#### css压缩
* 压缩 css 加快页面加载
  1. 在npm上搜索相应的包[gulp-minify-css](https://www.npmjs.com/package/gulp-minify-css)
  2. 安装
  ```
  npm i -D gulp-minify-css
  ```
  3. 在gulp中使用
  ```js
  var minify = require('gulp-minify-css')

  gulp.task('sass', function(){
    gulp.src('src/main.scss')
        .pipe(sass())
        .pipe(prefix())
        .pipe(minify()) // 压缩css
        .pipe(gulp.dest('dist/'));
  });
  ```
#### 图片压缩
* 使用imagemin来压缩图片
  1. 在npm上搜索相应的包[gulp-imagemin](https://www.npmjs.com/package/gulp-imagemin)和
  [imagemin-pngquant](https://www.npmjs.com/package/imagemin-pngquant)
  2. 安装
  ```
  cnpm i -D  gulp-imagemin imagemin-pngquant
  ```
  3. 在gulp中使用
  ```js
  var imagemin = require('gulp-imagemin');
  var pngquant = require('imagemin-pngquant');

  gulp.task('imagemin', function(){
    return gulp.src('src/images/*')
      .pipe(imagemin({
        progressive: true,
        svgoPlugins: [{removeViewBox: false}],
        use: [pngquant()]
      }))
      .pipe(gulp.dest('dist/images'));
  });
  ```
  4. 新增图片，进行压缩测试

## 四. 当材料设计遇到响应式开发
#### 4.1 为何要移动优先
* 由简入繁易, 由繁入简难
* 手机端屏幕大小有限，优先实现主要业务
* 利用媒介查询，覆盖原css样式
  ```scss
  @media (min-width: 420px) {
    .hero {
      background-img: url('big.png');
    }
  }
  ```
#### 4.2 定制自己的色盘
* 前端设计不是设计问题而是逻辑问题(颜色层次)
* [地址](http://www.materialpalette.com/cyan/pink)

#### 4.3 色盘上各种颜色的作用
* [uplabs.com](https://www.uplabs.com/material)
* [dribbble.com](https://dribbble.com/)
* 色盘上颜色的作用
  1. 主色(构成页面大背景。)
  2. 强调色(用来凸显一些重要操作或内容。)
  3. 其他颜色(突出内容主次层级关系。)
#### 4.4 什么是响应式设计
* 简单定义
  一份代码能够适应全部屏幕尺寸。
* 响应式三要素
  第一，弹性布局；第二，弹性图片；第三，media 查询。

#### 4.5 四种常见响应模式
* 什么是响应模式
  * 当屏幕拉伸达到零界点时，网页布局的变化模式
* 四种模式
  1. Column Drop 列下沉
      * 手机上每一个大块单独占据一行，随着屏幕尺寸拉伸会在同一行上形成多个 column 列
  2. Mostly Fulid
      * 基本上跟 Column Drop 一样，但是有一点点“固定布局“的特点：当到达一定宽度后，主体内容部分不再变宽，成为固定宽度。
  3. Layout Shifter
      * 变换式，也就是不必遵循原有内容顺序，可以根据最佳展示需要来调整大块顺序。  
  4. Off Canvas
      * 抽屉式，屏幕不够宽的时候，隐藏，通过按钮呼出。足够宽的屏幕上，始终显示。

#### 4.6 viewport 设置
* 问题是什么？
  * 任何设备都按照 960 的宽度来显示，造成很多设备上字体变成了原来的 1/3 。
* 解决方案
  ```html
    <meta name="viewport" content="width=device-width, initial-scale=1">
  ```

## 五. Flexbox来了
#### 5.1 flexbox时代来临
* Flexbox 解决了哪些些以前CSS没能解决好的问题
  * position
  * float
  * clear float
#### 5.2 项目准备
#### 5.3 类似手机app的移动版布局
* flex
* flex-shrink: 0;
* flex-grow: 1;
#### 5.4 使用font-awesome图标
* 处理gulp错误
* align-item: center;
* justify-content: space-between;
* 添加padding和border后整个元素尺寸改变
  ```scss
  box-sizing: border-box;
  ```
#### 5.5 gulp-wrap 使用布局文件
* 安装gulp-wrap
  ```
  npm i -D gulp-wrap
  ```
#### 5.6 browser-sync页面自动刷新
* 安装browserSync
  ```
  npm i -D browser-sync
  ```
* 修改gulpfile.js
  ```js

  ```
* 浏览器自动刷新  
## 六. Media查询
#### 6.1 使用媒体查询media-query
* 临界点
  ```scss
  @media (min-width: 600px) {
    // ...
  }
  ```
#### 6.2 响应式导航
* 根据设备宽度的改变覆盖原有css样式
#### 6.3 flexbox 流体网格
* flex-grid
* calc
  ```scss
  .card {
    margin: 20px;
    width: calc(50% -40px); // 计算宽度
  }
  ```
#### 6.4 总结
 

  













