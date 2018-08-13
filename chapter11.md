# poll scm

如果你想每次git commit时自动执行该pipeline，有两种方法，一种是让Jenkins对git进行轮询，每分钟检查git仓库有没有更新，如下配置

![](/assets/importpollscm.png)

另一种方式是使用git提供的hook，该方式原理是git一旦提交，便会触发hook中的脚本，让脚本给Jenkins发送执行pipeline的指令，这种方式更优雅，但相应要做的事情更多一点

跑自动化用例每次用手工点击jenkins出发自动化用例太麻烦了，我们希望能每天固定时间跑，这样就不用管了，坐等收测试报告结果就行。

一、定时构建语法

\* \* \* \* \*

\(五颗星，中间用空格隔开）

第一颗\*表示分钟，取值0~59

第二颗\*表示小时，取值0~23

第三颗\*表示一个月的第几天，取值1~31

第四颗\*表示第几月，取值1~12

第五颗\*表示一周中的第几天，取值0~7，其中0和7代表的都是周日

1.每30分钟构建一次：

H/30 \* \* \* \*

2.每2个小时构建一次

H H/2 \* \* \*

3.每天早上8点构建一次

0 8 \* \* \*

4.每天的8点，12点，22点，一天构建3次

0 8,12,22 \* \* \*

（多个时间点，中间用逗号隔开）

5.问题来了：每个月的1-7号一天构建一次咋写呢？

二、Build periodically

1.Build periodically：周期性进行项目构建，这个是到指定的时间必须触发构建任务

2.比如我想在每天的9点，17点，朝九晚五各构建一次，在Build periodically里设置如下

3.上面红色字体：Spread load evenly by using ‘H 9,17 \* \* \*’ rather than ‘0 9,17 \* \* \*’，这句话大概意思就是说，用这个语法会比后后面那个好：H 9,17 \* \* \*

4.下一次构建时间是05时48分06秒，然后再下次是09时48分06秒

二、Poll SCM

1.Poll SCM:定时检查源码变更（根据SCM软件的版本号），如果有更新就checkout最新code下来，然后执行构建动作

2.如果我想每隔30分钟检查一次源码变化，有变化就执行

三、Job关联

1.举个案例场景，比如我下面Job1是web项目打包并发布的构建任务，我想每次打完包发布后，然后触发自动化测试Job2的构建。

（当然发布后，一般会等几分钟才会完全加载完成，再下一次构建的时候，可以用python加个脚本sleep几分钟）

2.构建触发器勾选Build after other projects are built，Projects to watch输入Job1的名称

（这里可以输入多个依赖的jobs，多个job中间用逗号隔开）

3.下面有三个选择，一般默认第一个就行

Trigger only if build is stable：构建稳定时触发

Trigger even if the build is unstable ：构建不稳定时触发

Trigger even if the build fails ： 构建失败的时候触发

4.上面设置好后，启动第一个Job完成后，就能接着启动第二个Job了

四、另外两种

1.触发远程构建 \(例如,使用脚本）

2.GitHub hook trigger for GITScm polling： 这个是管理github上代码有变动时构建

最后这2个一般用的也少，了解下就行

注：Build periodically和Poll SCM两者是可以结合起来使用的

Poll SCM：定时检查源码变更，如果有更新就checkout最新code下来，然后执行构建动作。

如果没有更新就不会执行构建

Build periodically：周期进行项目构建（源码是否发生变化没有关系）

![](/assets/importbuildTrig.png)

