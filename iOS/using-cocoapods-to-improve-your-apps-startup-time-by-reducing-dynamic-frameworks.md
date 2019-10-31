### 用 Cocoapods 合并动态库，改善应用的启动时间

iOS 应用启动过程，main() 方法调用前，主要过程如下：

1. 内核加载可执行文件
2. load dylibs
3.	Rebase&bind
4. Objc setup(注册 Objc 类，将Category中的方法插入方法列表等) 
5. initializers (调用Objc类的 +load方法，C++ 的构造函数\_\_attribute__((destructor))等)


想要优化这些启动流程中的耗时，其中一项可以选择把动态库合并来减少启动时间。
项目推进过程中，变得越来越大，一般公司内部分模块都会跟据功能或者业务分成私有 Pods 集成到 Podfile 中。
如果Podfile 使用 `use_frameworks ` 指令，并且使用比较多的 pod ,启动时间尤其是 `pre-main` 阶段加载动态库的耗时。因为动态库逐个加载，相应的降低加载速度。

> 在每个动态库的加载过程中， dyld需要： <br>
	1. 分析所依赖的动态库 <br>
	2. 找到动态库的mach-o文件 <br>
	3. 打开文件 <br>
	4. 验证文件 <br>
	5. 在系统核心注册文件签名<br>
	6. 对动态库的每一个segment调用mmap()


苹果在2016年WWDC上的演讲[Optimizing App Startup Time](Optimizing App Startup Time) 中，推荐合并动态库已提升启动速度。

现在 [cocoapods-pod-merge](https://github.com/grab/cocoapods-pod-merge) 支持合并动态库。

终端执行：

>gem install cocoapods-pod-merge 

安装 `cocoapods-pod-merge `


##### 1. 使用 MergeFile 进行 pods 合并
插件引入一个 MergeFile 文件，内容：

<pre><code>group 'Networking'
	pod 'AFNetworking'
	pod 'SDWebImage'
end</code></pre>
MergeFile 做了两件事情
1. 定义一个名字为 Networking 的 group ，将会是合并后的 pod 名字。
2. 告诉 cocoapods-pod-merge 插件将  AFNetworking 和 SDWebImage 合并成 Networking。

##### 2. 更新 Podfile
使用 cocoapods-pod-merge 插件需要更新 Podfile 文件。
需要现在 Podfile 开始添加 `plugin 'cocoapods-pod-merge'`,并且修改 Podfile 以使用合并后的 pod

<pre><code>plugin 'cocoapods-pod-merge'

target 'MyApp'
	# pod 'AFNetworking' # Not needed anymore, since we'll use the merged Pod
	# pod 'SDWebImage' # Not needed anymore, since we'll use the merged Pod
	pod 'Networking', :path => 'MergedPods/Networking' # The merged pod
end</code></pre>







参考

- [iOS启动速度优化总结](https://blog.csdn.net/u013602835/article/details/96429977)
- [main () 之前的过程有哪些?](https://ioscaff.com/articles/191/what-are-the-processes-before-the-03-ios-interview-question-main)
- [点击 Run 之后发生了什么？](https://www.jianshu.com/p/d5cf01424e92)