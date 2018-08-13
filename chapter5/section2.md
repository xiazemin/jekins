# Jenkins 进程内脚本批准

# 

Jenkins和许多插件允许用户在Jenkins中执行Groovy脚本。这些脚本功能由以下提供：



脚本控制台。

Jenkins Pipeline。

扩展插件的电子邮件。

在Groovy插件 -使用“执行系统Groovy脚本”的步骤时。

版本1.60及更高版本的JobDSL插件。

为了保护Jenkins不会执行恶意脚本，这些插件在Groovy Sandbox中执行用户提供的脚本，可以限制可访问的内部API。然后，管理员可以使用脚本安全性插件提供的“进程内脚本批准”页面来管理在Jenkins环境中应该允许哪些不安全的方法（如果有的话）。



Jenkins 进程内脚本批准



开始

该脚本安全插件被自动安装后安装设置向导，虽然最初没有额外的脚本或操作批准使用。



这个插件的旧版本可能不安全使用。请查看脚本安全性插件页面 中列出的安全警告，以确保脚本安全性插件是最新的。

进程内脚本的安全性由两种不同的机制提供： Groovy Sandbox



Groovy Sandbox



 和 Script Approval。第一个Groovy Sandbox默认启用Jenkins Pipeline，允许用户提供的Scripted和Declarative Pipeline执行，而无需事先管理员干预。第二个“脚本批准”允许管理员批准或拒绝未分组的脚本，或允许Sandbox脚本执行其他方法。



在大多数情况下，Groovy Sandbox和 Script Security内建的已批准方法签名列表的组合将足够。强烈建议，如果绝对必要，管理员只会偏离这些默认值。



Groovy Sandbox

为了减少管理员的手动干预，默认情况下，大多数脚本将在Groovy Sandbox中运行，包括所有 Jenkins Pipeline。Sandbox只允许Groovy的一些方法被认为足够安全，以便在未经事先批准的情况下执行“不受信任”访问。使用Groovy Sandbox的脚本都受到相同的限制，因此由管理员编写的Pipeline将受到非管理用户授权的限制。



当脚本尝试使用Sandbox未经授权的功能或方法时，脚本将立即停止，如下所示，Jenkins Pipeline



Jenkins 进程内脚本批准



图1.未经授权的方法签名在运行时通过Blue Ocean被拒绝



在管理员通过“ 进程内脚本批准”页面批准方法签名之前，上述Pipeline将不会执行 。



除了添加批准的方法签名，用户还可以完全禁用Groovy Sandbox，如下所示。禁用Groovy Sandbox要求整个脚本必须经过管理员审核并手动批准。



Jenkins 进程内脚本批准



图2.禁用Pipeline的Groovy Sandbox

脚本批准

由管理员手动批准整个脚本或方法签名，为管理员提供了额外的灵活性，以支持更高级的进程内脚本编写。当Groovy Sandbox被禁用或者调用了内置列表以外的方法时，Script Security插件将检查经过管理员管理的已批准脚本和方法列表。



对于希望在Groovy Sandbox之外执行的脚本，管理员必须在“ 进程内脚本批准”页面中批准整个脚本：



Jenkins 进程内脚本批准



图3.批准一个unsandboxed Script Pipeline



对于使用Groovy Sandbox但是希望执行当前未经批准的方法签名的脚本也将被Jenkins停止，并且要求管理员在脚本被允许执行之前批准特定的方法签名：



Jenkins 进程内脚本批准



图4.批准新的方法签名



批准假设权限检查

脚本批准提供三个选项：批准，拒绝和“批准假设权限检查”。虽然前两者的目的是不言而喻的，但第三个要求需要对内部数据脚本能够访问的内容以及Jenkins函数中的权限如何进行一些额外的了解。



考虑访问该方法的脚本，该脚本 hudson.model.AbstractItem.getParent\(\)本身是无害的，并返回一个包含当前正在执行的流水线或作业的文件夹或根目录的对象。在该方法调用，执行hudson.model.ItemGroup.getItems\(\)（将列出文件夹或根项目中的项目）之后，需要该Job/Read权限。



这可能意味着批准hudson.model.ItemGroup.getItems\(\)方法签名将允许脚本绕过内置权限检查。



相反，通常更需要单击“ 批准”假设权限检查，这将导致脚本批准引擎允许方法签名，假设运行该脚本的用户具有执行该方法的Job/Read权限，例如此示例中的权限。

