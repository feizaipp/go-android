# go-android
## 搭建环境
### 安装 go 环境

* 从[官网下载](https://golang.google.cn/dl/)
* 解压缩 tar -C /usr/local -xzf go1.12.9.linux-amd64.tar.gz
* 配置环境变量， go 的环境变量通过 go env 命令查看

### 安装 gomobile

* 从 github 上下载源码 git clone https://github.com/golang/mobile.git
* 编译 go build golang.org/x/mobile/cmd/gomobile
* 初始化环境 gomobile init ，该命令会在 $GOPATH/bin 目录下生成 gobind 可执行文件，将 gobind 命令的路径加到环境变量中

### 安装 NDK
* 下载 android-ndk ，解压缩
* 配置环境变量 export ANDROID_NDK_HOME=/home/zpehome/work/ndk/android-ndk-r20

### 安装 SDK
* 下载 android-sdk ，解压缩
* 配置环境变量 export ANDROID_HOME="/home/zpehome/work/test/android/android-sdk-linux" export PATH="$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/platform-tools:$PATH"
* 更新 ./tools/android  update sdk --no-ui

## 编译 .arr
将源码中的 hello 文件夹拷贝到 $GOPATH/src 目录，执行下面命令编译：

> gomobile bind -target=android hello ，编译成功后在  $GOPATH/src 产生 hello.arr 文件，这个文件最后提供给 APP 使用，将该文件放到 Android 工程的 libs 目录即可。

## 修改 build.gradle 文件，使用 hello.arr

> 增加 repositories 项；在 dependencies 项中添加最后一行。
```
repositories {
    flatDir {
        dirs 'libs'
    }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'androidx.appcompat:appcompat:1.0.2'
    implementation 'androidx.constraintlayout:constraintlayout:1.1.3'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'androidx.test:runner:1.1.1'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.1.1'
    implementation(name:'hello', ext:'aar')
}
```

## 修改 Mainctivity 代码

> 使用 import 引入 hello 模块，直接调用 hello 模块中的接口
```
import hello.Hello;
String greetings = Hello.greetings("Android and Gopher");
mTextView.setText(greetings);
```
