---
标题: Java 环境
aliases: 
tags:
  - 技术/Mac/Java环境
创建时间: 2025-07-18 12:07
修改时间: 2025-08-06 22:27
---
### JDK环境变量

``` bash
# 编辑配置文件
vim ~/.bash_profile 
# zsh 下使用
vim ~/.zshrc

# Java 多jdk版本切换
export JAVA_7_HOME=/Library/Java/JavaVirtualMachines/jdk1.7.0_80.jdk/Contents/Home 
export JAVA_8_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_171.jdk/Contents/Home  
export JAVA_11_HOME=/Library/Java/JavaVirtualMachines/jdk-11.0.9.jdk/Contents/Home  
export JAVA_16_HOME=/Library/Java/JavaVirtualMachines/jdk-16.0.2.jdk/Contents/Home  

# 编辑一个命令jdk*，终端执行则切换至jdk*
alias jdk7="export JAVA_HOME=$JAVA_7_HOME"     
alias jdk8="export JAVA_HOME=$JAVA_8_HOME"       
alias jdk11="export JAVA_HOME=$JAVA_11_HOME"
alias jdk16="export JAVA_HOME=$JAVA_16_HOME"
# 最后安装的版本，这样当自动更新时，始终指向最新版本
export JAVA_HOME=`/usr/libexec/java_home`
```
