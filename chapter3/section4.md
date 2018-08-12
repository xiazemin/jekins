# Jenkins安全

在Jenkins中，可在 Jenkins 实例建立用户和他们的相关权限。默认情况下，不希望每个人都能够在 Jenkins 中定义工作或其他管理任务。因此，Jenkins 必须有一个安全配置的能力。



要配置 Jenkins 安全，请执行以下步骤。

第1步 - 点击“Manage Jenkins”，选择“Configure Global Security”选项。



第2步 - 点击启用安全\(Enable Security\)选项。举个例子，假设我们希望 Jenkins 来维持它的用户自己的数据库，所以在安全域\(Security Realm\)，选择“Jenkin's own user database” 选项。

默认情况下，希望有一个中央管理员可以在系统中定义用户，从而确保 “Allow users to sign up” 选项选中。现在剩下的可以不用管了，点击保存\(Save\)按钮就可以。



第3步 - 这里会提示您添加第一个用户。作为一个例子，我们建立一个管理系统的用户。



第4步 - 现在是时候来创建系统中的用户了。现在，当你转到 Manage Jenkins，向下滚动，会看到一个“Manage Users”选项。单击该选项。



第5步 - 就像你定义管理员用户，开始创建其他系统的用户。作为一个例子，我们只是创建一个用户名为“user”。



第6步 - 现在是时候来设置授权，基本上是可以访问什么。转到 Manage Jenkins → Configure Global Security.



现在在授权部分，单击 ‘Matrix based security’





步骤7 - 如果没有看到用户在用户组列表中，输入用户名，并将其添加到列表中。然后给予适当的权限给用户。

一旦定义了相关的授权，点击保存\(Save\)按钮。

Jenkins 安全现在已经设置。

注 - 对于Windows AD身份验证，Active Directory插件添加到Jenkins。



