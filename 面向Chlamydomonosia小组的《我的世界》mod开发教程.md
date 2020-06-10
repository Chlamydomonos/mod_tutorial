# 面向Chlamydomonosia小组的《我的世界》mod开发教程

## 绪论：什么是《我的世界》mod

众所周知，Minecraft是一个官方支持盗版的游戏。那么，究竟是什么维持了MC的经久不衰呢？我个人认为，丰富的mod系统正是维持MC活力的第一要素。既然你已经来到了这个教程，你大概已经知道一个mod的样子了。接下来，我们先介绍mod的发展史。  

很久很久以前，一群玩家对MC的内容产生了不满。那时，MC官方支持盗版的行为还没有出现。这群玩家毅然决定：反编译MC，修改其代码，做出自己想要的游戏！  

当然，mojang对MC的代码进行了混淆。玩家们反编译出代码时，只会看到以下天书：  

```java
class a{
    public a(b c, d e, int f ){
        this.g = h.i(c, e);
        this.j = k.l.m(f);
    }
}
```

但是，这并没有阻止最早的mod开发者。他们通过大胆猜想，严谨证明，找到了各个变量的含义。于是，最早的mod诞生了。  

接下来的一段时间里，mod的开发仍然需要开发者看懂这些天书般的代码。直到一些玩家自发地创造了一套通用的反混淆名称，并向所有mod开发者开放。这就是现在mod开发的基础——Minecraft Coder Pack (MCP)。MCP的出现，使mod开发难度骤降，大量优秀的mod涌现。建筑mod (Buildcraft)就是其中的典范。MCP本身是违法的，但mojang意识到，mod一定会大大增加MC的热度。因此，mojang一直默许MCP的存在。

然而，依靠MCP编写的mod依然存在一些问题。如果你曾安装过一些古老的mod，你会发现它们的安装步骤是这样的：

* 解压mod文件，得到大量.class文件
* 将这些.class文件压缩到.minecraft\mods\minecraft.jar中

安装困难并不是MCP的最大问题。对玩家而言，最显著的问题是：各开发者的mod并不互相兼容，导致玩家几乎不可能安装两个以上的mod。

对开发者而言，各类问题也阻挡着他们的脚步：

* MC很多地方的代码都是垃圾代码，不同开发者为实现自己的功能做出了不同的修改，导致mod之间深刻的不兼容性
* MC并没有一个完整的事件系统，各开发者需自己造轮子，更改MC核心代码

因此，为解决以上问题，我们现在使用的mod API——forge诞生了。

forge通过更改MC核心代码，创造了一套mod加载系统。这样，不同开发者的mod不会在加载阶段就显示出不兼容性。同时，forge加入了一些非常有用的扩展库，如事件系统，通用流体系统等，让各mod之间兼容性大幅提升。最后，forge允许各mod向其他开发者开放自己的API，实现真正的mod间联动。

现在，我将开始介绍mod的开发。

##　第一节：安装开发环境

Minecraft使用Java语言编写，因此，开发mod需要的最基础的工具，就是Java开发工具。你将需要：

* Java Development Kit 8 (jdk8)
* 一个Java IDE (对于Chlamydomonosia小组成员，应统一使用Intellij idea)

安装jdk后，你应百度Java环境变量配置教程，并配置好环境变量。

idea似乎有了官方中文版，可以百度切换语言的方法。

接下来，我们要安装绪论中所述的forge API：

* 访问[forge官网](https://files.minecraftforge.net/)![image-20200609181329480](面向Chlamydomonosia小组的《我的世界》mod开发教程.assets/image-20200609181329480.png)

* 点击侧栏中的1.12.2![image-20200609181610560](面向Chlamydomonosia小组的《我的世界》mod开发教程.assets/image-20200609181610560.png)

* 等待所有图标加载完成![image-20200609181547696](面向Chlamydomonosia小组的《我的世界》mod开发教程.assets/image-20200609181547696.png)

* 翻到下面的列表，点击14.23.5.2847行中的Mdk旁边的圆形按钮——**不要直接点击Mdk文字！**![image-20200609181734934](面向Chlamydomonosia小组的《我的世界》mod开发教程.assets/image-20200609181734934.png)

* 下载Mdk的zip文件后，解压到一个单独的纯英文路径

* 打开该文件夹，按住Shift键，右键点击空白区域，选择“在此处打开命令窗口”选项

* 输入以下命令:

  ```
  gradlew setupDecompWorkspace
  ```

* 这会下载大量文件到你的电脑，建议翻墙。[utsc_zzzz](https://fmltutor.ustc-zzzz.net/1.1-%E9%85%8D%E7%BD%AE%E4%BD%A0%E7%9A%84%E5%B7%A5%E4%BD%9C%E7%8E%AF%E5%A2%83.html)提供了一个只能用来安装forge的翻墙工具。

* 构建成功后，输入以下命令

  ```
  gradlew genIntellijRuns
  ```

  ```
  gradlew idea
  ```

  同样，每条命令输入之后，等待构建完成。

  如果构建失败，重新输入再试一次。

* 用idea把这个文件夹作为项目打开，开发环境安装完毕。

注意，以上所述命令中自动下载的文件默认保存在C盘的用户文件夹中，如果想更改路径，可以增加[环境变量](https://baijiahao.baidu.com/s?id=1652502091402613426&wfr=spider&for=pc)GRADLE_USER_HOME，值为你想更改的路径