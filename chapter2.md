# Jenkins单元测试

Jenkins提供了一个开箱即用功能来选择JUnit，并提供了一系列的插件进行单元测试等技术，一个例子是 MSTest 的.Net单元测试。如果你打下面的链接 [https://wiki.jenkins-ci.org/display/JENKINS/xUnit+Plugin](https://wiki.jenkins-ci.org/display/JENKINS/xUnit+Plugin) ，它会列出单元测试插件可用的列表。

在Jenkins中的JUnit测试

下面的例子将考虑

基于Junit的一个简单的 HelloWorldTest 类。

ANT作为构建工具使用 Jenkins 建立相应的类。

第1步- 转到 Jenkins 仪表盘，然后点击现有的HelloWorld项目，并选择配置\(Configure\)选项，如下图所示：

![](/assets/importT.png)

第2步 - 浏览到部分添加生成步骤和选择调用Ant选项。

第3步 - 单击高级\(Advanced \)按钮。

第4步 - 在构建文件部分，输入 build.xml 文件的位置。这里构建的文件位置是：D:\worksp\yiibai.com\jenkins\HelloWorldBuild.xml

第5步 - 接下来，单击该选项添加后期生成选项，然后选择“Publish Junit test result report”

第6步 - 在测试报告XML，进入如下图所示的位置。确保报表是其在 Hello World 项目工作区创建的文件夹中。“\*.xml” 主要是告诉Jenkins 这是由JUnit测试用例运行产生的结果XML文件。然后被转换成以后可以查看报告的 XML 文件。完成后，单击在最后保存\(Save\)选项。

第7步 - 保存后，可以点击“Build Now ”选项。

一旦构建完成后，构建的状态将显示，如果构建成功与否。在生成的输出信息，你现在会发现叫做测试结果\(Test Result\)附加部分。在我们的例子中，我们进入了一个负面的测试情况下，这样的结果只会失败，作为一个例子。

可以到控制台输出中看到更多的信息。但是更有趣的是，如果点击测试结果，将看到一个钻头的测试结果下来。

