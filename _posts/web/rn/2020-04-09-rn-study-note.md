---
layout: article
title: 'React Native 入门学习笔记'
date: 2020.04.09 22:09:05
key: web_20200409_1
category:
  - Web
  - note
tags:
  - 前端开发
  - React Native
---

最近在学习 React Native，这里记录下遇到的问题以及解决方案，方便后期查阅，笔者使用的系统为 MacOS。

<!-- more -->

### 环境搭建

由于之前用 AS 做过一些安卓原生开发，所以 JAVA 以及 Android SDK 的环境搭建就直接略过了，关于环境搭建，[官网的文档](https://reactnative.cn/docs/getting-started)写的非常详细，其他部分跟着做问题不大。

由于之前安装的 Node 是稳定版，看到文档中要求 Node 要求 v12 版本以上，所以我需要先升级 Node 版本，使用`sudo n latest`安装最新版本时出现了报错`killed 9`，折腾半天最后在官网下载最新 Node.js 安装包得以解决，算是遇到的第一个问题吧。

到此，环境搭建部分算是完成了。

### 初始化项目

接下来就是必不可少的 Hello World 环节，RN 提供了 CLI 初始化项目，本以为很简单，结果各种踩坑。

1. 首先，通过官方提供的命令`npx react-native init AwesomeProject`创建一个 RN 项目  
```shell
ry09iudeMacBook-Pro:reactNative ry09iu$ npx react-native init HelloWordRN
...
✔ Downloading template
✔ Copying template
✔ Processing template
⠼ Installing CocoaPods dependencies (this may take a few minutes)
```
2. 接下来一直卡在`Installing CocoaPods dependencies`，网上有博主说是 pod 源的问题，停掉 npx 初始化命令，修改 AwesomeProject/ios/Podfile 文件，在最后一行加入`source 'https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git'`，修改后在 AwesomeProject/ios/ 路径下执行`pod install`，几分钟后报如下错误  

```shell

ry09iudeMacBook-Pro:ios ry09iu$ pod install
Analyzing dependencies
Fetching podspec for `DoubleConversion` from `../node_modules/react-native/third-party-podspecs/DoubleConversion.podspec`
Fetching podspec for `FBLazyVector` from `../node_modules/react-native/Libraries/FBLazyVector`
Fetching podspec for `FBReactNativeSpec` from `../node_modules/react-native/Libraries/FBReactNativeSpec`
Fetching podspec for `Folly` from `../node_modules/react-native/third-party-podspecs/Folly.podspec`
Fetching podspec for `RCTRequired` from `../node_modules/react-native/Libraries/RCTRequired`
Fetching podspec for `RCTTypeSafety` from `../node_modules/react-native/Libraries/TypeSafety`
Fetching podspec for `React` from `../node_modules/react-native/`
Fetching podspec for `React-Core` from `../node_modules/react-native/`
Fetching podspec for `React-CoreModules` from `../node_modules/react-native/React/CoreModules`
Fetching podspec for `React-RCTActionSheet` from `../node_modules/react-native/Libraries/ActionSheetIOS`
Fetching podspec for `React-RCTAnimation` from `../node_modules/react-native/Libraries/NativeAnimation`
Fetching podspec for `React-RCTBlob` from `../node_modules/react-native/Libraries/Blob`
Fetching podspec for `React-RCTImage` from `../node_modules/react-native/Libraries/Image`
Fetching podspec for `React-RCTLinking` from `../node_modules/react-native/Libraries/LinkingIOS`
Fetching podspec for `React-RCTNetwork` from `../node_modules/react-native/Libraries/Network`
Fetching podspec for `React-RCTSettings` from `../node_modules/react-native/Libraries/Settings`
Fetching podspec for `React-RCTText` from `../node_modules/react-native/Libraries/Text`
Fetching podspec for `React-RCTVibration` from `../node_modules/react-native/Libraries/Vibration`
Fetching podspec for `React-cxxreact` from `../node_modules/react-native/ReactCommon/cxxreact`
Fetching podspec for `React-jsi` from `../node_modules/react-native/ReactCommon/jsi`
Fetching podspec for `React-jsiexecutor` from `../node_modules/react-native/ReactCommon/jsiexecutor`
Fetching podspec for `React-jsinspector` from `../node_modules/react-native/ReactCommon/jsinspector`
Fetching podspec for `ReactCommon` from `../node_modules/react-native/ReactCommon`
Fetching podspec for `Yoga` from `../node_modules/react-native/ReactCommon/yoga`
Fetching podspec for `glog` from `../node_modules/react-native/third-party-podspecs/glog.podspec`
Downloading dependencies
Installing CocoaAsyncSocket (7.6.4)
Installing CocoaLibEvent (1.0.0)
Installing DoubleConversion (1.1.6)
Installing FBLazyVector (0.62.2)
Installing FBReactNativeSpec (0.62.2)
Installing Flipper (0.33.1)
Installing Flipper-DoubleConversion (1.1.7)
Installing Flipper-Folly (2.2.0)
Installing Flipper-Glog (0.3.6)
[!] /bin/bash -c 
set -e
\#!/bin/bash
\# Copyright (c) Facebook, Inc. and its affiliates.
\#
\# This source code is licensed under the MIT license found in the
\# LICENSE file in the root directory of this source tree.

set -e

PLATFORM_NAME="${PLATFORM_NAME:-iphoneos}"
CURRENT_ARCH="${CURRENT_ARCH}"

if [ -z "$CURRENT_ARCH" ] || [ "$CURRENT_ARCH" == "undefined_arch" ]; then
    \# Xcode 10 beta sets CURRENT_ARCH to "undefined_arch", this leads to incorrect linker arg.
    \# it's better to rely on platform name as fallback because architecture differs between simulator and device

    if [[ "$PLATFORM_NAME" == *"simulator"* ]]; then
        CURRENT_ARCH="x86_64"
    else
        CURRENT_ARCH="armv7"
    fi
fi

export CC="$(xcrun -find -sdk $PLATFORM_NAME cc) -arch $CURRENT_ARCH -isysroot $(xcrun -sdk $PLATFORM_NAME --show-sdk-path)"
export CXX="$CC"

\# Remove automake symlink if it exists
if [ -h "test-driver" ]; then
    rm test-driver
fi

./configure --host arm-apple-darwin

\# Fix build for tvOS
cat << EOF >> src/config.h
/* Add in so we have Apple Target Conditionals */
\#ifdef __APPLE__
\#include <TargetConditionals.h>
\#include <Availability.h>
\#endif
/* Special configuration for AppleTVOS */
\#if TARGET_OS_TV
\#undef HAVE_SYSCALL_H
\#undef HAVE_SYS_SYSCALL_H
\#undef OS_MACOSX
\#endif
/* Special configuration for ucontext */
\#undef HAVE_UCONTEXT_H
\#undef PC_FROM_UCONTEXT
\#if defined(__x86_64__)
\#define PC_FROM_UCONTEXT uc_mcontext->__ss.__rip
\#elif defined(__i386__)
\#define PC_FROM_UCONTEXT uc_mcontext->__ss.__eip
\#endif
EOF

\# Prepare exported header include
EXPORTED_INCLUDE_DIR="exported/glog"
mkdir -p exported/glog
cp -f src/glog/log_severity.h "$EXPORTED_INCLUDE_DIR/"
cp -f src/glog/logging.h "$EXPORTED_INCLUDE_DIR/"
cp -f src/glog/raw_logging.h "$EXPORTED_INCLUDE_DIR/"
cp -f src/glog/stl_logging.h "$EXPORTED_INCLUDE_DIR/"
cp -f src/glog/vlog_is_on.h "$EXPORTED_INCLUDE_DIR/"

checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for arm-apple-darwin-strip... no
checking for strip... strip
checking for a thread-safe mkdir -p... ./install-sh -c -d
checking for gawk... no
checking for mawk... no
checking for nawk... no
checking for awk... awk
checking whether make sets $(MAKE)... yes
checking whether make supports nested variables... yes
checking for arm-apple-darwin-gcc... /Library/Developer/CommandLineTools/usr/bin/cc -arch armv7 -isysroot 
checking whether the C compiler works... no
xcrun: error: SDK "iphoneos" cannot be located
xcrun: error: SDK "iphoneos" cannot be located
xcrun: error: SDK "iphoneos" cannot be located
xcrun: error: unable to lookup item 'Path' in SDK 'iphoneos'
/Users/ry09iu/Library/Caches/CocoaPods/Pods/Release/Flipper-Glog/0.3.6-1dfd6/missing: Unknown `--is-lightweight' option
Try `/Users/ry09iu/Library/Caches/CocoaPods/Pods/Release/Flipper-Glog/0.3.6-1dfd6/missing --help' for more information
configure: WARNING: 'missing' script is too old or missing
configure: error: in `/Users/ry09iu/Library/Caches/CocoaPods/Pods/Release/Flipper-Glog/0.3.6-1dfd6':
configure: error: C compiler cannot create executables
See `config.log' for more details
```
3. 网上的解决方案有说执行`sudo xcode-select --switch /Applications/Xcode.app`，尝试后无法解决，后来发现我压根儿没安装 Xcode，在 AppStore 上安装 Xcode，却被告知必须升级到最新的 MacOS 10.15.x 版本，之前听说新版系统会有很多问题，但现在我不得不将系统更新到 MacOS Catalina，经过半个多小时的更新终于完成

4. 更新系统后安装 Xcode，开始执行`pod install`，直接就报错了
```shell
ry09iudeMacBook-Pro:ios ry09iu$ pod install
-bash: /usr/local//bin/pod: /System/Library/Frameworks/Ruby.framework/Versions/2.3/usr/bin/ruby: bad interpreter: No such file or directory
```
5. 网上有博主提供的解决方案是需要升级 Ruby，可通过 brew 更新

```shell

ry09iudeMacBook-Pro:ios ry09iu$ brew upgrade ruby
Error: Another active Homebrew update process is already in progress.
Please wait for it to finish or terminate it to continue.
==> Upgrading 1 outdated package:
ruby 2.6.2 -> 2.7.1
==> Upgrading ruby 2.6.2 -> 2.7.1 
==> Installing dependencies for ruby: openssl@1.1 and readline
==> Installing ruby dependency: openssl@1.1
==> Downloading https://homebrew.bintray.com/bottles/openssl@1.1-1.1.1f.catalina.bottle.tar.gz
==> Downloading from https://akamai.bintray.com/72/724cd97c269952cdc28e24798e350fcf520a32c5985aeb26053ce006a09d8179?__gd
curl: (56) LibreSSL SSL_read: SSL_ERROR_SYSCALL, errno 54
Error: Failed to download resource "openssl@1.1"
Download failed: https://homebrew.bintray.com/bottles/openssl@1.1-1.1.1f.catalina.bottle.tar.gz
Warning: Bottle installation failed: building from source.
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
Error: An exception occurred within a child process:
  CompilerSelectionError: openssl@1.1 cannot be built with any available compilers.
Install GNU's GCC:
  brew install gcc
```

看样子应该是网络问题，重新执行了一次，稍等片刻后成功升级新版 Ruby

```shell

ry09iudeMacBook-Pro:ios ry09iu$ brew upgrade ruby
Error: Another active Homebrew update process is already in progress.
Please wait for it to finish or terminate it to continue.
==> Upgrading 1 outdated package:
ruby 2.6.2 -> 2.7.1
==> Upgrading ruby 2.6.2 -> 2.7.1 
==> Installing dependencies for ruby: openssl@1.1 and readline
==> Installing ruby dependency: openssl@1.1
==> Downloading https://homebrew.bintray.com/bottles/openssl@1.1-1.1.1f.catalina.bottle.tar.gz
==> Downloading from https://akamai.bintray.com/72/724cd97c269952cdc28e24798e350fcf520a32c5985aeb26053ce006a09d8179?__gd
==> Pouring openssl@1.1-1.1.1f.catalina.bottle.tar.gz
==> Caveats
A CA file has been bootstrapped using certificates from the system
keychain. To add additional certificates, place .pem files in
  /usr/local/etc/openssl@1.1/certs

and run
  /usr/local/opt/openssl@1.1/bin/c_rehash

openssl@1.1 is keg-only, which means it was not symlinked into /usr/local,
because macOS provides LibreSSL.

If you need to have openssl@1.1 first in your PATH run:
  echo 'export PATH="/usr/local/opt/openssl@1.1/bin:$PATH"' >> ~/.bash_profile

For compilers to find openssl@1.1 you may need to set:
  export LDFLAGS="-L/usr/local/opt/openssl@1.1/lib"
  export CPPFLAGS="-I/usr/local/opt/openssl@1.1/include"

==> Summary
🍺  /usr/local/Cellar/openssl@1.1/1.1.1f: 8,057 files, 18MB
==> Installing ruby dependency: readline
==> Downloading https://homebrew.bintray.com/bottles/readline-8.0.4.catalina.bottle.tar.gz
==> Downloading from https://akamai.bintray.com/6a/6ae1c8e7c783f32bd22c6085caa4d838fed7fb386da7e40ca47b87ec9b1237d6?__gd
==> Pouring readline-8.0.4.catalina.bottle.tar.gz
==> Caveats
readline is keg-only, which means it was not symlinked into /usr/local,
because macOS provides BSD libedit.

For compilers to find readline you may need to set:
  export LDFLAGS="-L/usr/local/opt/readline/lib"
  export CPPFLAGS="-I/usr/local/opt/readline/include"

==> Summary
🍺  /usr/local/Cellar/readline/8.0.4: 48 files, 1.5MB
==> Installing ruby
==> Downloading https://homebrew.bintray.com/bottles/ruby-2.7.1.catalina.bottle.tar.gz
==> Downloading from https://akamai.bintray.com/7f/7f5ed2afb15b25f9616b617ecca2ee376eb67cf6c105790df22221b7be9c1ef9?__gd
==> Pouring ruby-2.7.1.catalina.bottle.tar.gz
==> Caveats
By default, binaries installed by gem will be placed into:
  /usr/local/lib/ruby/gems/2.7.0/bin

You may want to add this to your PATH.

ruby is keg-only, which means it was not symlinked into /usr/local,
because macOS already provides this software and installing another version in
parallel can cause all kinds of trouble.

If you need to have ruby first in your PATH run:
  echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.bash_profile

For compilers to find ruby you may need to set:
  export LDFLAGS="-L/usr/local/opt/ruby/lib"
  export CPPFLAGS="-I/usr/local/opt/ruby/include"

==> Summary
🍺  /usr/local/Cellar/ruby/2.7.1: 20,372 files, 33.2MB
Removing: /usr/local/Cellar/ruby/2.6.2... (19,342 files, 32.3MB)
==> Checking for dependents of upgraded formulae...
==> No dependents found!
==> Caveats
==> openssl@1.1
A CA file has been bootstrapped using certificates from the system
keychain. To add additional certificates, place .pem files in
  /usr/local/etc/openssl@1.1/certs

and run
  /usr/local/opt/openssl@1.1/bin/c_rehash

openssl@1.1 is keg-only, which means it was not symlinked into /usr/local,
because macOS provides LibreSSL.

If you need to have openssl@1.1 first in your PATH run:
  echo 'export PATH="/usr/local/opt/openssl@1.1/bin:$PATH"' >> ~/.bash_profile

For compilers to find openssl@1.1 you may need to set:
  export LDFLAGS="-L/usr/local/opt/openssl@1.1/lib"
  export CPPFLAGS="-I/usr/local/opt/openssl@1.1/include"

==> readline
readline is keg-only, which means it was not symlinked into /usr/local,
because macOS provides BSD libedit.

For compilers to find readline you may need to set:
  export LDFLAGS="-L/usr/local/opt/readline/lib"
  export CPPFLAGS="-I/usr/local/opt/readline/include"

==> ruby
By default, binaries installed by gem will be placed into:
  /usr/local/lib/ruby/gems/2.7.0/bin

You may want to add this to your PATH.

ruby is keg-only, which means it was not symlinked into /usr/local,
because macOS already provides this software and installing another version in
parallel can cause all kinds of trouble.

If you need to have ruby first in your PATH run:
  echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.bash_profile

For compilers to find ruby you may need to set:
  export LDFLAGS="-L/usr/local/opt/ruby/lib"
  export CPPFLAGS="-I/usr/local/opt/ruby/include"
```

6. 重新执行`pod install`，报错依旧
```shell
ry09iudeMacBook-Pro:ios ry09iu$ pod install
-bash: /usr/local//bin/pod: /System/Library/Frameworks/Ruby.framework/Versions/2.3/usr/bin/ruby: bad interpreter: No such file or directory
```
7. 后来终于找到解决方案，`sudo gem install -n /usr/local/bin cocoapods —pre`，更新后查看 pod，终于不报错了
```
ry09iudeMacBook-Pro:ios ry09iu$ pod --version
1.9.1
```
8. 接下来重新执行`pod install`，又遇到第5个问题

```
xcrun: error: SDK "iphoneos" cannot be located
xcrun: error: SDK "iphoneos" cannot be located
xcrun: error: SDK "iphoneos" cannot be located
xcrun: error: unable to lookup item 'Path' in SDK 'iphoneos'
/Users/ry09iu/Library/Caches/CocoaPods/Pods/Release/Flipper-Glog/0.3.6-1dfd6/missing: Unknown `--is-lightweight' option
Try `/Users/ry09iu/Library/Caches/CocoaPods/Pods/Release/Flipper-Glog/0.3.6-1dfd6/missing --help' for more information
configure: WARNING: 'missing' script is too old or missing
configure: error: in `/Users/ry09iu/Library/Caches/CocoaPods/Pods/Release/Flipper-Glog/0.3.6-1dfd6':
configure: error: C compiler cannot create executables
See `config.log' for more details
```

但这次通过`sudo xcode-select --switch /Applications/Xcode.app`成功解决，继续执行

```shell
ry09iudeMacBook-Pro:ios ry09iu$ pod install 
Analyzing dependencies
Fetching podspec for `DoubleConversion` from `../node_modules/react-native/third-party-podspecs/DoubleConversion.podspec`
Fetching podspec for `Folly` from `../node_modules/react-native/third-party-podspecs/Folly.podspec`
Fetching podspec for `glog` from `../node_modules/react-native/third-party-podspecs/glog.podspec`
Downloading dependencies
Installing CocoaAsyncSocket (7.6.4)
/usr/local/lib/ruby/gems/2.7.0/gems/cocoapods-1.9.1/lib/cocoapods/downloader/cache.rb:114: warning: Using the last argument as keyword parameters is deprecated; maybe ** should be added to the call
/usr/local/lib/ruby/gems/2.7.0/gems/cocoapods-1.9.1/lib/cocoapods/downloader/request.rb:61: warning: The called method `slug' is defined here
/usr/local/lib/ruby/gems/2.7.0/gems/cocoapods-1.9.1/lib/cocoapods/downloader/cache.rb:100: warning: Using the last argument as keyword parameters is deprecated; maybe ** should be added to the call
/usr/local/lib/ruby/gems/2.7.0/gems/cocoapods-1.9.1/lib/cocoapods/downloader/request.rb:61: warning: The called method `slug' is defined here
Installing CocoaLibEvent (1.0.0)
Installing DoubleConversion (1.1.6)
Installing FBLazyVector (0.62.2)
Installing FBReactNativeSpec (0.62.2)
Installing Flipper (0.33.1)
Installing Flipper-DoubleConversion (1.1.7)
Installing Flipper-Folly (2.2.0)
Installing Flipper-Glog (0.3.6)
Installing Flipper-PeerTalk (0.0.4)
Installing Flipper-RSocket (1.1.0)
Installing FlipperKit (0.33.1)
Installing Folly (2018.10.22.00)
Installing OpenSSL-Universal (1.0.2.19)
Installing RCTRequired (0.62.2)
Installing RCTTypeSafety (0.62.2)
Installing React (0.62.2)
Installing React-Core (0.62.2)
Installing React-CoreModules (0.62.2)
Installing React-RCTActionSheet (0.62.2)
Installing React-RCTAnimation (0.62.2)
Installing React-RCTBlob (0.62.2)
Installing React-RCTImage (0.62.2)
Installing React-RCTLinking (0.62.2)
Installing React-RCTNetwork (0.62.2)
Installing React-RCTSettings (0.62.2)
Installing React-RCTText (0.62.2)
Installing React-RCTVibration (0.62.2)
Installing React-cxxreact (0.62.2)
Installing React-jsi (0.62.2)
Installing React-jsiexecutor (0.62.2)
Installing React-jsinspector (0.62.2)
Installing ReactCommon (0.62.2)
Installing Yoga (1.14.0)
Installing YogaKit (1.18.1)
Installing boost-for-react-native (1.63.0)
Installing glog (0.3.5)
Generating Pods project
/usr/local/lib/ruby/gems/2.7.0/gems/nanaimo-0.2.6/lib/nanaimo/writer/pbxproj.rb:13: warning: Using the last argument as keyword parameters is deprecated; maybe ** should be added to the call
/usr/local/lib/ruby/gems/2.7.0/gems/nanaimo-0.2.6/lib/nanaimo/writer.rb:35: warning: The called method `initialize' is defined here
Integrating client project

[!] Please close any current Xcode sessions and use `firstrnapp3.xcworkspace` for this project from now on.
Pod installation complete! There are 47 dependencies from the Podfile and 37 total pods installed.
```

大功告成啦！

### 运行应用

运行应用相对比较简单，通过`yarn android`或者`yarn react-native run-android`编译并运行应用，但我遇到了如下报错  

```shell
To reload the app press "r"
To open developer menu press "d"

events.js:288
      throw er; // Unhandled 'error' event
      ^

Error: EMFILE: too many open files, watch
    at FSEvent.FSWatcher._handle.onchange (internal/fs/watchers.js:127:28)
Emitted 'error' event on NodeWatcher instance at:
    at NodeWatcher.checkedEmitError (/Users/ry09iu/Documents/web-dev/reactNative/firstrnapp3/node_modules/sane/src/node_watcher.js:143:12)
    at FSWatcher.emit (events.js:311:20)
    at FSEvent.FSWatcher._handle.onchange (internal/fs/watchers.js:133:12) {
  errno: -24,
  syscall: 'watch',
  code: 'EMFILE',
  filename: null
}
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! firstrnapp3@0.0.1 start: `react-native start`
npm ERR! Exit status 1
npm ERR! 
npm ERR! Failed at the firstrnapp3@0.0.1 start script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/ry09iu/.npm/_logs/2020-04-09T15_44_30_090Z-debug.log
```

手机上成功安装了应用，打开是红屏，提示无法连接开发者服务，曾尝试修改系统监控文件最大值无效，后来看到某位博主的解决方案，删除项目目录下的 node_modules 文件夹，然后重新执行`npm install`安装依赖，最终成功解决。
