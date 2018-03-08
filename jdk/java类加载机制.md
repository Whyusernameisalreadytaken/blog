# java类加载机制

## 类加载过程

类从被加载到虚拟机内存中开始,到卸载出内存为止,它的整个生命周期包括:加载(loading),验证(verification),准备(preparation),解析(resolution),初始化(initialization),使用(using),卸载(unloading)7个阶段.其中验证,准备,解析3个部分统称为链接(linking)

![这里写图片描述](http://img.blog.csdn.net/20160308184325593)

加载、验证、准备、初始化和卸载5个阶段的顺序是确定的，类的加载过程必须严格按照这种顺序进行。解析阶段在某些情况下可以在初始化之后开始。在该机制下，保证了java语言的运行时绑定的特性。

### 加载

1. 加载过程通过类的全限定类名来获取该类的二进制字节流（可以从本地磁盘、网络、动态生成等获取）。从本地磁盘获取时，java命令需要指定-classpath路径
2. 将字节流所代表的静态存储结构转化为方法区的运行时数据结构
3. 在内存中生成一个代表该类的Class对象，作为方法区这个类的信息访问入口

### 验证

1. 验证是链接阶段的第一步，这一阶段的目的是为了确保Class文件的字节流包含的信息符合当前虚拟机的要求，不会危害虚拟机自身安全（java安全语言的提现）
2. 文件格式校验：验证字节流是否符合Class文件格式的规范。例如是否以magic:0xCAFEBABE开头、字节流中的主次版本号是否在当前虚拟机的处理范围之内、常量池中的常量是否有不被支持的类型。

```java

E:\GitHub\future\jdk\target\classes\com\future\log>java LogClient
Error: A JNI error has occurred, please check your installation and try again
Exception in thread "main" java.lang.UnsupportedClassVersionError: LogClient has been compiled by a more recent version of the Java Runtime (class file version 53.0), this version of the Java Runtime only recognizes class file versions up to 52.0
        at java.lang.ClassLoader.defineClass1(Native Method)
        at java.lang.ClassLoader.defineClass(ClassLoader.java:763)
        at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
        at java.net.URLClassLoader.defineClass(URLClassLoader.java:467)
        at java.net.URLClassLoader.access$100(URLClassLoader.java:73)
        at java.net.URLClassLoader$1.run(URLClassLoader.java:368)
        at java.net.URLClassLoader$1.run(URLClassLoader.java:362)
        at java.security.AccessController.doPrivileged(Native Method)
        at java.net.URLClassLoader.findClass(URLClassLoader.java:361)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
        at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:335)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
        at sun.launcher.LauncherHelper.checkAndLoadMain(LauncherHelper.java:495)

```

产生上述异常的原因：文件使用javac（1.9）编译LogClient文件，使用java（1.8）虚拟机加载LogClient.class字节码文件，在验证阶段不通过

```java

E:\GitHub\future\jdk\target\classes\com\future\log>java X
Error: A JNI error has occurred, please check your installation and try again
Exception in thread "main" java.lang.ClassFormatError: Truncated class file
        at java.lang.ClassLoader.defineClass1(Native Method)
        at java.lang.ClassLoader.defineClass(ClassLoader.java:763)
        at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
        at java.net.URLClassLoader.defineClass(URLClassLoader.java:467)
        at java.net.URLClassLoader.access$100(URLClassLoader.java:73)
        at java.net.URLClassLoader$1.run(URLClassLoader.java:368)
        at java.net.URLClassLoader$1.run(URLClassLoader.java:362)
        at java.security.AccessController.doPrivileged(Native Method)
        at java.net.URLClassLoader.findClass(URLClassLoader.java:361)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
        at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:335)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
        at sun.launcher.LauncherHelper.checkAndLoadMain(LauncherHelper.java:495)
```

产生上述异常原因：手动编写X.class文件，使用java（1.8）虚拟机加载X.class文件导致文件格式校验不通过

![class](C:\Users\DELL\Desktop\class.png)







