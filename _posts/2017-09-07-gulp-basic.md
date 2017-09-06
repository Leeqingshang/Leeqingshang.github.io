---
layout: post
title: gulp初步尝试
category: blog
description: gulp初步尝试
---


**gulp是什么**

就我个人的理解来说，gulp最开始是个提供简单功能搭建的工具。它本身不提供任何功能，所用的功能支持由插件提供，而功能的使用构建由使用者来制定。我需要使用less，就require一个gulp-less插件，然后在gulpfile.js主文件里调用这个插件来实现我的需求。这里所谓的调用就是新建一个task，gulp的主体就是一个个task以流的方式组合实现的。

**使用**

gulp依赖于node，所以要在安装好，然后安装gulp，全局活项目本地安装随便。然后过程中容易出错，多看错误提示就好了。

按照自己的需求来加载各种插件，但要注意是否已经下载支持。

**一些其他的**

据说以后的趋势是webpack,跟gulp不同，webpack是把所有的资源以模块的方式绑定到一起。我感觉应该gulp应该是零散件，对于小型/个人项目是比较合适的。而webpack可能是整体的类似fis那样的东西，但应该没那么强大，比较fis对应的企业级的开发。



**安装准备**

`npm init`

建立package.js文件，你好我也好


**安装gulp**

全局安装，使得你可以在命令行窗口使用gulp命令

`npm install --global gulp-cli`

安装gulp本地环境支持

`npm install gulp --save-dev`

使用`gulp -v`命令版本信息，检验是否安装成功


**使用gulp**


gulp有多个任务模块构建成流式的输入输出结构

你所做的，就是依据自身需求写各种task


首先，在项目目录下*新建一个gulpfile.js的文件*，也就是gulp的任务流文件，我们整个操作基本都写字这个文件里面


然后,引入gulp

`const gulp = require('gulp');`

构建任务

    task('test-task',function(){
        // place code for your default task here
        console.log('test-task start!')
    });
    
这样，就写好了一个最简单的任务，task接受两个参数，一个任务名称，一个返回方法。

直接`gulp test-task`执行，就可以看到我们预先设置好的输出结果。


当然，我们实际开发过程中的操作肯定要复杂些，下面介绍gulp的api，以及一些常用的插件的使用

**API**

`gulp.src(globs[, options])`

读取一个文件流名称或者文件流名称数组，返回对应的文件流。简单粗暴的说就是读取文件，后续可以对这个文件流进行`pipe()`拼接执行其他操作

*文件流也包括具体的文件地址，或是筛选出来的多个文件，比如`*.js`*

`gulp.dest(path[, options])`

把一个文件流导出到目的地。参数为一个文件夹地址或者多个目录组成的数组。必须要先有文件流输入才能起作用。当传入的目录不存在时会被创建

`gulp.task（name [，deps] [，fn]）`

构建任务，参数为当前任务名，一个任务列表数组（可选），执行函数。如果你设置了任务列表，代表着执行当前任务之前要先执行数组中的任务。


`gulp.watch(glob[, opts, cb])`

监听一个或多个文件，参数为文件文件/文件流地址，后面可选参数为当文件内容改变时要执行的任务列表数组，或者也可以是一个普通的执行函数。

`gulp.run(task[opts,..])`

运行一个或者多个任务

[参考官方API](https://github.com/gulpjs/gulp/blob/master/docs/API.md)


**常用插件**

`gulp-sass`

对SCSS文件进行编译，转换成CSS的输出流，类似的还有gulp-less

`gulp-uglify`

js文件压缩

`gulp-minify-css`

css 文件压缩

`gulp-imagemin`

图片压缩

`gulp-autoprefixer`

自动添加css兼容前缀

`gulp-concat`

合并js文件

`gulp-cache`

图片缓存

`gulp-connect`

建立一个本地的运行环境

另外还有一些

    gulp-jshint//js代码校验提示
    gulp-notify//更改提醒
    del        //清除文件
    

我们做个简单的例子：

    gulp.task('connect',function(){
      connect.server({
        root:'./',
        livereload:true,
        port:2008
      })
    });

    gulp.task('css',function(){
      gulp.src('./src/scss/*.scss')
        .pipe(plumber())
        .pipe(sass())
        .pipe(gulp.dest('./dist/css'))
        .pipe(connect.reload());
    });
    
    gulp.task('watch',function(){
      gulp.watch('./src/scss/*.scss',['css'])
    });
    
    gulp.task('default',['connect','watch'])


这样就做好了一个根据sass文件更改自动更新的环境


接下来说下我用过的一些插件的理解

**gulp-watch**

这个插件看名字就知道是监听，但`gulp.watch()`已经有监听功能了，那与之有什么区别呢？

首先，`gulp.watch()`它并不具备具体过滤识别功能，比如说

`gulp.watch('./src/*.js',['task1'])`

这个时候，目录src下的所有js文件都会被重载，而使用`gulp-watch`可以进行过滤监听，只有发生改动的文件才会被传递到下一步操作。这对监听文件量大的项目来说是很有用的
    
    var gulp = require('gulp');
    var sass = require('gulp-sass');
    var watch = require('gulp-watch');
    var plumber = require('gulp-plumber');
    gulp.task('css',function(){
        gulp.src('./scss/*.scss')
        .pipe(watch('./scss/*.scss'))//watch必须有参数，不然会报错
        .pipe(plumber())
        .pipe(sass())
        .pipe(gulp.dest('./dist/css'))
    })
 
 
上面的任务中，`gulp-watch`就起到了过滤器的作用

当然，你也可以用来替代`gulp.watch`:

    watch('./scss/*.scss',function(){
         //在这里写你的操作代码
    })
    
当然，你会发现写法会有些不同，主要是它并不像原生watch一样支持数组参数形式.我们可以用处理任务写成函数来执行。

    gulp.watch('./src/*.scss',['task1','task2'])
    
    function task1(){
       //任务task1的操作
    }
    
    function task2(){
       //任务task1的操作
    }
    watch('./src/*.scss',function(){
         task1();
         task2();
    })

当然，这样一看比较麻烦。所以最好是你确定你真的需要使用`gulp-watch`的时候才使用。

另外，还可以用`gulp-watch`结合`gulp-filter`来针对不同的操作类型来执行不同的处理程序。比如我监听一个目录，新建文件时是一个处理程序，修改文件又是另外一个处理程序，[参见文档](https://github.com/floatdrop/gulp-watch/blob/master/docs/readme.md#filtering-custom-events)


**gulp-plumber**

形成一个稳定的流，不会因为一些错误而中断服务

**gulp-load-plugins**

可以根据你的package.json里面的‘devDependencies’的内容加载插件，你就不需要在gulpfile.js文件的开头写那么一大堆引入，大概就是像下面的例子这样

    const gulp = require('gulp');
    const plugins = require('gulp-load-plugins')();
    gulp.task('css',function(){
            gulp.src('./css/*')
            .pipe(plugins.sass())
    })
    
    
    
总结：gulp有非常多的插件，甚至你可以根据自己的需要自己写一个插件，当然一般来说没有必要。另外，这只是在本地搭建一个环境。

试用一个新的插件前最好对它有足够的了解，以免掉坑里。




