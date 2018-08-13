# poll scm

如果你想每次git commit时自动执行该pipeline，有两种方法，一种是让Jenkins对git进行轮询，每分钟检查git仓库有没有更新，如下配置

![](/assets/importpollscm.png)

另一种方式是使用git提供的hook，该方式原理是git一旦提交，便会触发hook中的脚本，让脚本给Jenkins发送执行pipeline的指令，这种方式更优雅，但相应要做的事情更多一点，这里就不演示这种方法了，感兴趣的同学可以自己研究一下



