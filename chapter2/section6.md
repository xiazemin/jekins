# Jenkins自动部署

有许多可用的插件可以用来构建文件创建成功后传送到相应的应用程序/Web服务器。在例子是“Deploy to container Plugin”。要使用此遵循以下步骤操作。

第1步 - 进入Manage Jenkins → Manage Plugins。



转至可用部分，找到插件“Deploy to container Plugin”，并安装该插件。重新启动Jenkins 服务器。



这个插件需要一个 war/ear 文件，在构建结束时将部署到正在运行的远程应用服务器。

Tomcat 4.x/5.x/6.x/7.x



JBoss 3.x/4.x



Glassfish 2.x/3.x



第2步 − 转到生成项目，然后单击配置选项。选择选项 “Deploy war/ear to a container”





第3步 − 在部署war/ear 到一个容器部分，输入服务器所需的详细信息，和需要部署的文件并点击保存按钮。这些步骤将不能保证构建后文件部署到容器能成功。

