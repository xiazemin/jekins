# chapter1

Jenkins是一个功能强大的应用程序，允许持续集成和持续交付项目，无论用的是什么平台。这是一个免费的源代码，可以处理任何类型的构建或持续集成。集成Jenkins可以用于一些测试和部署技术。

Jenkins是一种软件允许持续集成。Jenkins 安装在一台服务上也中央构建发生的地方。下面的流程图展示了Jenkins是如何工作的一个非常简单的工作流。

![](/assets/import.png)

Why Jenkins

伴随着Jenkins，有时人们还可能看到它与Hudson关联。Hudson是由 Sun Microsystems 开发的一个非常流行的开源，基于Java 的持续集成工具，后来被Oracle收购。Sun被Oracle收购之后，一个从 Hudson 源代码的分支由 Jenkins 创建出台。

什么是持续集成？

持续集成是一个开发的实践，需要开发人员定期集成代码到共享存储库。这个概念是为了消除发现的问题，后来出现在构建生命周期的问题。持续集成要求开发人员有频繁的构建。最常见的做法是，每当一个代码提交时，构建应该被触发。

系统要求

JDK    JDK 1.5 或以上

Memory    2 GB RAM \(推荐\)

Disk Space

没有最起码的要求。需要注意的是，因为所有的构建将保存在 Jenkins 机器上，它必须确保有足够的磁盘空间可用于构建存储。

Operating System Version

Jenkins可以安装在Windows, Ubuntu/Debian, Red Hat/Fedora/CentOS, Mac OS X, openSUSE, FReeBSD, OpenBSD, Gentoo 系统上

Java Container

WAR文件可以在支持 Servlet2.4/JSP2.0或更高版本的容器中运行。（一个例子是Tomcat 5）。

