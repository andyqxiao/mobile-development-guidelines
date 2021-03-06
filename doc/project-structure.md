# 项目的目录结构

## 摘要
* 项目代码的目录结构
* 项目的命名规范
* 编译输出的文件命名规范
* 正式签名文件和调试签名文件的使用

### 项目代码的目录结构

尽量使用新的Android Studio建议的目录结构

```
new-structure
├─ library-foobar
├─ app
│  ├─ libs
│  ├─ src
│  │  ├─ androidTest
│  │  │  └─ java
│  │  │     └─ com/futurice/project
│  │  └─ main
│  │     ├─ java
│  │     │  └─ com/futurice/project
│  │     ├─ res
│  │     └─ AndroidManifest.xml
│  ├─ build.gradle
│  └─ proguard-rules.pro
├─ fooLibrary
├─ barLibrary
├─ build.gradle
└─ settings.gradle
```

其中fooLibrary和barLibrary是app项目的引用项目，内部的目录结构与之类似。如果你必须使用Eclipse ADT作为开发工具，请使用以下的结构，并修改gradle文件，使其兼容Android Studio。

```
project
├─ app
│  ├─ assets
│  ├─ libs
│  ├─ res
│  ├─ src
│  │  └─ com/futurice/project
│  ├─ AndroidManifest.xml
│  ├─ build.gradle
│  ├─ project.properties
│  ├─ proguard-rules.pro
│  ├─ .classpath
│  └─ .projcet
├─ fooLibrary
└─ barLibrary
```

其中fooLibrary和barLibrary是app项目的引用项目，内部的目录结构与之类似。由于要兼容Android Studio的项目结构（可以同时在Eclipse ADT和Android Studio中编辑），所以引用项目（子项目）不能放在主项目app的目录之中。

### 项目的命名规范
- 项目名称：按Java类名规则，不要使用空格，但可以使用下划线，减号，如果AGDriverside，AGDriverside-New。（其实SVN、Git是支持含有空格的项目名，但有时空格会被转义，而且使用命令行输入路径时有诸多不便，所以不建议使用含有空格的项目名）
- 包名：按Java的规范

### 编译输出的文件命名规范
编译输出的文件采用“项目名称+版本号+编译类型+代码管理系统编号”的方式

编译类型如下：

- Dev或Test: 用于内部测试的版本
- Alpha: Alpha测试版本
- Beta: Beta测试版本
- RC: 发布版本

关于代码管理系统编号，如果你使用的是SVN这样的集中式管理系统，可以使用系统提供的唯一递增序列号，如果使用的是Git这样的分布式管理系统，它没有递增序列号，但有代表某一版本的Hash值，但这个Hash值一般很长，很难读，可以取它的前8位。

以下是一些规范的命名：

```
MyProject_1.0.2_Dev_r123.apk
MyProject_1.0.2_RC_r123.apk
MyProject_1.0.2_RC_abd232dd.apk
```

如果你不方便或无法去查询代码管理系统提供的版本号，最后的版本号也可以使用当前时间来代替，比如：

```
MyProject_1.0.2_Dev_2016-01-02-10-08.apk
```

这个apk文件是1.0.2的开发版本，采用Debug签名，编译输出时间是2016年01月02日上午10点08分。

### 正式签名文件和调试签名文件的使用

- 发布版本必须使用正式签名，比如含Alpha/Beta/RC作为编译输出类型的
- 调试包使用统一的调度签名

使用统一的调试签名的好处：

- 当在其他开发同事的设备上调试时，可以直接取代安装，而不是卸载后重新安装，不会清除之前的用户数据（特别是当调试情形不允许重新安装程序的时候），有利于合作开发
- 可以使用统一的配置参数，比如Google的地图开发与签名文件相关，微信SDK的集成也与签名相关

如何采用统一的签名文件：

- 如果你使用Eclipse ADT开发，可以打开Preferences，选择Android->Build选项，设置"Custom debug keystory"即可。或者在此界面查看"Default debug keystore"的路径，然后使用新的统一签名文件取代即可
- 如果你使用Android Studio，可以在build.gradle中设置。当然，你也可以修改全局配置来实现这个目的，但通常并不建议这么做。

```groovy
signingConfigs {
	debug {
		storeFile file('debug.keystore')
		keyAlias "androiddebugkey"
		keyPassword "android"
		storePassword "android"
	}
}
```

统一的签名文件 [下载](debug.keystore)

