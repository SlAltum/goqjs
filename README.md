这是quickjs的cgo封装，github上有很多quickjs的cgo封装，这里参考的是[https://github.com/lithdew/quickjs](https://github.com/lithdew/quickjs)，由于原项目已经很久没有更新了，使用的quickjs代码也比较老，故而创建了这个仓库。 \
这里使用的动态链接库libquickjs.dll是使用mingw64编译器版本如下
```
GNU Make 4.4
Built for Windows32
Copyright (C) 1988-2022 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```
gcc版本为13.1.0，详细编译过程参考我的csdn博客[windows系统下编译quickjs总结](http://t.csdn.cn/ZIJQS) \
之所以使用动态链接库，是因为quickjs支持将c编写的动态链接库作为ES6模块供JavaScript导入，而c module需要依赖quickjs库，如果使用静态库编译，会导致每个c module当中都集成了一份quickjs的二进制包，而作为JavaScript解释器的执行程序也需要集成quickjs库。这样一来不仅使得文件体积膨胀，而且不同二进制文件中依赖的静态库可能不一样，增加了出错几率。使用动态链接库则可以解决这个问题。 \
值得一提的是，用go语言编译出的二进制可执行文件包体大小高达4M，而原生的quickjs生成的可执行文件即使是用静态库编译也只有区区2M，这里笔者推测是因为go编译时将一些标准库和数学库也编译进二进制文件导致的（这是go语言的优点之一，不管项目多么复杂，都只编译成一个单独的二进制可执行文件）。这便意味着如果使用go编写c module，依然无法解决之前提出的问题，当然笔者接触go语言的时间其实也不长，比较菜，如果哪位大佬有什么好的想法，欢迎在issue中留言。\
PS：go build出来的二进制文件要和dll放在同一目录下才能执行。