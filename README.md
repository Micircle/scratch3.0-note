# scratch 3.0 开发笔记

欢迎体验我们基于scratch3.0开发的IDE工具[慧编程(mBlock5)](https://ide.makeblock.com/#/)，也可以下载[PC或移动APP](http://www.mblock.cc/zh-home/software/?noredirect=zh-CN)，不仅完全支持scratch3.0的舞台角色功能，还可以使用它来控制硬件机器人，使用Python编程，以及体验AI，机器学习等扩展。

这里是在开发过程中的一些笔记，会不定期更新。

## scratch3.0 几个核心库介绍

## [scratch-gui](https://github.com/LLK/scratch-gui)
----

由React组件组成的UI界面
<img src='./scratch3.0.png'/>
简单介绍下目前可使用的功能
+ 菜单：
    + 多语言切换
    + 项目导出和导入（*支持导入scatch2的项目*）
    + 切换加速模式（*加速模式会使循环内积木块执行更快*）
    + 教程
+ 工作区
    + 代码：使用积木块操作舞台角色
    + 造型：添加或编辑角色的造型图片，支持上传svg、png、jpg等图片，以及摄像头
    + 声音：添加或编辑角色的声音，支持上传mp3、wav的声音，以及录音
    + 扩展：可以添加额外的积木分类，目前开放的有：音乐、画笔、谷歌翻译、视频侦测等
+ 舞台区
    + 绿旗：触发运行工作区中的绿旗积木块
    + 停止：停止所有运行中的积木
    + 放大、缩小、全屏：调整舞台大小
    + 舞台：使用webgl渲染背景、角色
    + 其他：显示勾选的变量和侦测值，问答交互
+ 角色区：
    + 显示、设置角色的信息：角色名、坐标、显示隐藏、大小、方向
    + 添加到舞台的角色列表：
        + 调整顺序
        + 删除角色
        + 复制角色
        + 导出角色（会将角色的声音、造型、积木一起导出为一个.sprite3文件）
        + 将积木、声音、造型拖拽分享到其他角色
    + 添加角色和背景：支持上传图片和.sprite3文件



## [scratch-blocks](https://github.com/LLK/scratch-blocks)
----

Scratch Blocks是基于谷歌[Blockly](https://github.com/google/blockly)开发的一个图形化js库，用积木块的形式来实现编程。

这里Scratch Blocks抛弃了Blockly中积木块转Python等编程语言的功能（Blockly中Generator的部分），通过抛事件的方式搭配Scratch VM来实现控制舞台渲染。

## [scratch-vm](https://github.com/LLK/scratch-vm)
----
Scratch VM 是一个运行Scratch Blocks代码块的引擎库。主要由以下功能：

+ 加载解析项目：通过Scratch GUI导入一个.sb2、.sb3的项目文件，解析项目文件，加载项目中用到的图片和声音，将项目角色渲染到舞台上，将项目中用到的积木块渲染到代码工作区。
+ 导出项目：将当前项目中的角色图片、声音、积木等项目信息和资源打包压缩成.sb3文件
+ 解析运行积木：
    + 定义每种类型积木执行时的方法
    + 通过监听代码工作区的事件，保存当前角色代码工作区对应的积木代码信息
    + 角色切换时，根据这些积木信息将该角色的积木块还原到工作区
    + 不断检测代码块的状态，在代码块被触发运行时，解析代码块信息，执行代码块对应的方法，更新舞台角色状态
+ 扩展管理：扩展被添加时，解析扩展数据，更新Scratch Blocks的toolbox，添加新的分类和积木
+ 调用Scratch Render提供的舞台渲染接口，更新舞台

## [scratch-render](https://github.com/LLK/scratch-render)
----
Scratch Render 是基于webgl的一个渲染引擎，主要用到了twgl库来操作webgl，定义了供Scratch VM调用的舞台渲染接口。
主要功能：
    + 根据svg、png的数据在canvas中渲染成图形
    + 更新角色图层的信息：大小、位置、角度、图层优先级、图形特效等
    + 画笔图层

## [scratch-audio](https://github.com/LLK/scratch-audio)
----
Scratch Audio 是用来解析声音、播放声音的库。

## 其他仓库
[scratch-paint](https://github.com/LLK/scratch-paint)：GUI中造型编辑的组件，y用到了paper.js，目前处理带有text标签的svg文件时会有bug，在windows环境中容易崩溃。

[scratch-svg-render](https://github.com/LLK/scratch-svg-render)：scratch处理svg资源的一个工具库，处理带有text标签的svg文件时会有bug。

[scratch-i10n](https://github.com/LLK/scratch-i10n)：scratch多语言库，包含了GUI和Scratch Blocks中用到的翻译信息。


还有些其他库，这里就不再介绍了。