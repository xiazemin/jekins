# Jenkins邮件通知

Jenkins 配备了一个开箱工具，添加一个电子邮件通知的构建项目。

第1步 - 配置SMTP服务器。 转到 Manage Jenkins → Configure System。转到电子邮件\(E-mail\)通知部分，并输入所需的SMTP服务器和用户的电子邮件后缀细节。



第2步- 配置在Jenkins 项目的收件人 - 当配置任何 Jenkins 建设项目，就在添加收件人将会收到电子邮件通知在不稳定性或断裂构建的时候。然后点击保存\(Save\)按钮。



除了默认，也有通知的插件可在市场上找到。 一个例子是来自Tikal 知识库其允许发送作业状态的通知在JSON和XML格式的通知插件。此插件启用端点进行配置，如下图所示。



下面是每个选项的细节 -

"Format" − 这是通知有效载荷格式，可以是JSON或XML。



"Protocol" − 协议用于发送通知消息，HTTP，TCP或UDP。



"Event" − 作业事件触发通知：工作开始，工作已完成，作业完成或所有活动\(默认选项\)。



"URL" − URL发送通知。它采用"http://host" 的形式对HTTP协议，“host:port” 的TCP和UDP协议。



"Timeout" − 超时毫秒的默认发送通知请求为：30秒。

