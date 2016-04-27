CocoaPods 的使用篇
===

[目录](/README.md) [返回上级](/ch02/index.md)

CocoaPods有什么用, 此处不解答。其在iOS项目中的重要性是不言而喻的, 看看github上的iOS开源项目就了解了。
对于一个老鸟来说如何学习使用该工具，是我要关注的。我的学习路径是这样的的:

-   快速开始
    快速了解工具的用途

-   功能与原理
    了解工具具备的功能与工作原理

-   高阶应用
    通过一个实例掌握工具的高阶应用

那对于CocoaPods而言,采用类似路径. 通过一个高阶实例掌握工具的核心功能, 基本上就完成了对工具的掌握。

## 1. 快速开始

### 1.1 安装

常规方法参考官网: [getting-started](https://guides.cocoapods.org/using/getting-started.html).

````
$ sudo gem install cocoapods

````
### 1.2 使用

-   创建一个基于单视图(Single View)的XCode项目 Demo. 确认项目的成功运行。关闭Demo项目, 进入 Demo 项目路径.

````
$: cd Demo
$: ls
Demo           Demo.xcodeproj
````

-   创建 Podfile 文件, 设置项目需要的 pod 依赖库.可以手工创建, 也可命令创建.

````
$: pod init
$: ls
Demo           Demo.xcodeproj Podfile
````
命令的成功调用的前提, 当前文件夹 存在XCode项目文件.

-   编辑 Podfile 文件

````
$: vim Podfile
````
文件中已经自动填写了项目名称, 添加必要的 pod 依赖库即可.

````
# Uncomment this line to define a global platform for your project
# platform :ios, '8.0'
# Uncomment this line if you're using Swift
# use_frameworks!

target 'Demo' do
    # 根据项目需要添加相应的 pod 依赖库
    pod 'AFNetworking', '~> 3.0'  
end
````

-   生成新的Demo工作区(workspace)项目

````
$: pod install
$: ls
Demo             Demo.xcodeproj   Demo.xcworkspace Podfile          Podfile.lock     Pods
````
如果当前目录中已经存在工作区(workspace)项目, 可以编辑 Podfile 设置指定工作区文件

````Podfile
workspace 'MyWorkspace'

````

-   打开Demo工作区(workspace)项目

````
$: open Demo.xcworkspace

````

以上就是整个 CocoaPods 在 XCode项目中的简单应用了。

## 2. pod 功能全接触

如果仅仅是学习快速开始, 还不足以了解 pod 命令的全功能. 要掌握 CocoaPods, 就必须知道 pod 命令包括了哪些功能.

````
$: pod
Usage:

    $ pod COMMAND

      CocoaPods, the Cocoa library package manager.

Commands:
                    
    + cache      Manipulate the CocoaPods cache
                 # 控制CocoaPods缓存

    + init       Generate a Podfile for the current directory.
                 # 在当前项目目录生成 Podfile 文件

    + install    Install project dependencies to Podfile.lock versions
                 # 根据 Podfile 文件配置安装项目依赖库

    + ipc        Inter-process communication
                 # 进程间通信？

    + lib        Develop pods
                 # 开发 pods 库

    + list       List pods
                 # 列举所有 pods 库

    + outdated   Show outdated project dependencies
                 # 显示项目中过期的依赖库

    + plugins    Show available CocoaPods plugins
                 # 像是可用的 CocoPods 插件

    + repo       Manage spec-repositories
                 # 管理 pod 库描述文件(spec)的仓库(repository)   

    + search     Search for pods.
                 # 查询 pods 库

    + setup      Setup the CocoaPods environment
                 # 建立 CocoaPods 环境

    + spec       Manage pod specs
                 # 管理 pod 库的描述文件(spec)

    + trunk      Interact with the CocoaPods API (e.g. publishing new specs)
                 # 与CocoaPods 官方仓库的交互接口

    + try        Try a Pod!
                 # 测试一个 Pod 库, 会根据置顶 Pod库 spec文件提供的仓库路径进行下载, 完成后打开测试项目。

    + update     Update outdated project dependencies and create new Podfile.lock
                 # 更新过气的项目依赖库
Options:

    --silent     Show nothing
    --version    Show the version of the tool
    --verbose    Show more debugging information
    --no-ansi    Show output without ANSI codes
    --help       Show help banner of specified command
```` 
以上是pod命令的帮助输出, 我对相应的子命令进行了注释说明. 如果对子命令的简要功能说明, 不清楚就可以通过:

 ````
 $: pod try --help 
 Usage:

    $ pod try NAME|URL

      Downloads the Pod with the given `NAME` (or Git `URL`), install its dependencies
      if needed and opens its demo project. If a Git URL is provided the head of the
      repo is used.

Options:

    --no-repo-update   Skip running `pod repo update` before install
    --silent           Show nothing
    --verbose          Show more debugging information
    --no-ansi          Show output without ANSI codes
    --help             Show help banner of specified command
 ````
了解子命令的详细功能说明. 有了 pod try 命令, 我们就可以快速的测试CocoaPods库中大量的依赖库了.

不妨马上测试一个pod库:

````
$: pod try WTSegment
````
随便选了一个, 大家可以根据自己喜好测试一个pod库, 通过pod开发者提供的样例程序快速了解pod在实际项目中的使用方法.

再看看 ipc 子命令, 因为 ipc 同 try 命令一样属于一个冷门命令:

````
$: pod ipc --help
Usage:

    $ pod ipc COMMAND

      Inter-process communication

Commands:

    + list                  Lists the specifications known to CocoaPods.
    + podfile               Converts a Podfile to YAML.
    + repl                  The repl listens to commands on standard input.
    + spec                  Converts a podspec to JSON.
    + update-search-index   Updates the search index.

Options:

    --silent                Show nothing
    --verbose               Show more debugging information
    --no-ansi               Show output without ANSI codes
    --help                  Show help banner of specified command
 ````
原来这个命令主要是方便将pod库描述文件(spec)进行转化格式用的。

至此, 介绍了两个偏冷门命令, 其实 pod try 命令是非常有用的命令, 建议大家多用。

在后续章节中我们通过一个高阶例子(创建私有仓库下的pod依赖库)来学习 pod repo 与 pod lib 命令。
