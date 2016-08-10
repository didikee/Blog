## Android Ndk CPUs and Architectures 翻译 ##

### 1. CPU 与 设备构架 ###

当你使用原生代码(`native code`)，硬件方面的工作。NDK的让你确保你给你一个不同的ABI的可供选择编译合适的架构和CPU。

本节以说明如何针对特定开始 架构和CPU。然后，它提供了你需要瞄准的时候知道的信息 ARM CPU和体系结构的家庭。接着，它提供了关于其它的CPU和架构，它支持信息：NEON，86（32位和 64位），和 MIPS。最后，它解释了如何使用 cpufeatures 库，您的应用程序可以用它来 ​​查询一个给定的CPU和建筑有关他们所支持的可选功能。