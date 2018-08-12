# Jenkins docker

在 Docker 中下载并运行 Jenkins

有好几个可用的 Jenkins 镜像。

建议使用 jenkinsci/blueocean 镜像（在 Docker Hub 仓库）。这个镜像包含 Jenkins 的适用于生产环境的 长期支持版本，并集成了 Blue Ocean 插件和特性。这意味着不需要再单独安装 Blue Ocean 了。

每次 Blue Ocean 发布新版本时，jenkinsci/blueocean 镜像都会发布。可用在 tags 页面看到之前发布的镜像列表。

当然也有其他的可用的 Jenkins 镜像（在 Docker Hub 的 jenkins/jenkins 中）。然而，那些不包含 Blue Ocean，需要通过 Jenkins 的 \[Manage Jenkins\]\([https://jenkins.io/doc/book/managing\](https://jenkins.io/doc/book/managing%29\) &gt; \[Manage Plugins\]\([https://jenkins.io/doc/book/managing/plugins\](https://jenkins.io/doc/book/managing/plugins%29\) 页面安装。更多资料参考 Getting started with Blue Ocean。

2.1.2.1 在 macOS 和 Linux 上

1. 打开终端窗口

2. 下载 jenkinsci/blueocean 镜像并用 docker run命令在 Docker 中运行为容器：

docker run \

-u root \

--rm \

-d \

-p 8080:8080 \

-p 50000:50000 \

-v jenkins-data:/var/jenkins\_home \

-v /var/run/docker.sock:/var/run/docker.sock \

jenkinsci/blueocean

参数：

--rm：可选。在 Docker 容器（jenkinsci/blueocean 的实例）关闭时自动删除。

-d：可选，detached 后台模式。后台运行 jenkinsci/blueocean 容器并输出容器的 ID。如果不指定这个选项，Docker 会把日志输出到终端窗口。

-p 8080:8080：publishes 发布端口。将发布的 jenkinsci/blueocean 容器的 8080 端口映射到宿主机的 8080 端口。第一个数字代表宿主机的端口，最后一个数字代表容器的端口。因此如果你指定 -p 49000:8080，表示可以通过访问本地主机的 49000 端口来访问 Jenkins。

-p 50000:50000：可选。将发布的 jenkinsci/blueocean 容器的 50000 端口映射到宿主机的 50000 端口。只有在其他主机设置了一个或多个基于 JNLP 的 Jenkins 代理程序，而这些代理程序又与 jenkinsci/blueocean 容器（作为主 Jenkins 服务器“Jenkins master”）进行交互时才需要这个配置。默认情况下，基于 JNLP 的 Jenkins 代理通过 TCP 端口 50000 与 Jenkins master 进行通信。可以参考 Configure Global Security 页面更改 Jenkins master 上的端口号。如果要更改 Jenkins 主机的 JNLP 代理的 TCP 端口值（假设是 51000），那就需要重新运行Jenkins（通过 docker run ... 命令）并指定此“publishes”选项 -p 52000：51000，其中最后一个值与 Jenkins master 上的这个更改值相匹配，第一个值是 Jenkins master 的宿主机上的端口号，基于 JNLP 的 Jenkins 代理通过它（52000）与 Jenkins master 进行通信。

-v jenkins-data:/var/jenkins\_home：可选，但是建议使用。将容器的目录 /var/jenkins\_home 映射到 Docker volume 卷并命名为 jenkins-data。如果这个卷不存在，那么这个 docker run 命令会自动创建卷。如果你希望每次重启 Jenkins 时能持久化 Jenkins 的状态，则必须使用这个参数。如果没有指定，则每次 Jenkins 重启时都会初始化为新的实例。

注意：jenkins-data 卷可以通过 docker volume create 命令独立创建：

docker volume create jenkins-data

除了将容器的目录 /var/jenkins\_home 映射到 Docker 卷外，也可以映射到宿主机的本地文件系统，例如，通过 -v $HOME/jenkins:/var/jenkins\_home 可以将容器的目录 /var/jenkins\_home 映射到宿主机 $HOME 目录下的 jenkins 子目录中，通常是 /Users/&lt;your-username&gt;/jenkins 或 /home/&lt;your-username&gt;/jenkins。

-v /var/run/docker.sock:/var/run/docker.sock：可选。/var/run/docker.sock 表示 Docker 守护进程监听的 Unix 套接字。这个映射使得 jenkinsci/blueocean 容器可以和 Docker 守护进程通信，如果 jenkinsci/blueocean 容器需要实例化其他 Docker 容器时这种通信是必须的。如果使用 docker 参数（例如 agent { docker { ... } }）运行语法中包含 代理 部分的声明式管道，则此选项是必需的。更多资料参考 Pipeline Syntax 页面。

jenkinsci/blueocean：jenkinsci/blueocean 镜像本身。如果运行 docker run 命令时还没有下载这个镜像，docker 会自动下载。此外，如果在你上次运行这个命令之后镜像有更新的话，运行这个命令时会自动下载新发布的镜像。

注意：Docker 镜像也可以通过 docker pull 命令单独下载：docker pull jenkinsci/blueocean。

注意：如果复制并粘贴以上命令片段不起作用，请尝试复制并粘贴这个无注释的版本：

docker run \

-u root \

--rm \

-d \

-p 8080:8080 \

-p 50000:50000 \

-v jenkins-data:/var/jenkins\_home \

-v /var/run/docker.sock:/var/run/docker.sock \

jenkinsci/blueocean

2.1.4 通过 Docker logs 访问 Jenkins console 日志

有时候你会需要访问 Jenkins console 日志，例如 Post-installation setup wizard 中的 Unlocking Jenkins。



如果执行上面的 docker run ... 命令时没有通过 -d 选项指定为后台模式，那么 Jenkins console 日志可以在运行 Docker 命令的终端轻松获取到。



否则，可以通过 docker logs 访问 jenkinsci/blueocean 容器中的 Jenkins console 日志：



docker logs &lt;docker-container-name&gt;



其中 &lt;docker-container-name&gt; 可以通过 docker ps 命令获取。如果在执行上面的 docker run ... 命令时指定了 --name jenkins-blueocean 选项，可以这样使用 docker logs 命令：



docker logs jenkins-blueocean



2.1.5 访问 Jenkins 根目录

有时候你会需要访问 Jenkins 根目录，例如检查 workspace 子目录中的 Jenkins 构建详情。



如果将 Jenkins 根目录（/var/jenkins\_home）映射到机器的本地文件系统（通过上面的 docker run ... 命令），则可以直接通过本地机器的终端或命令行窗口访问这个目录的内容。



其他情况下，如果在 docker run ... 命令中指定了 -v jenkins-data:/var/jenkins\_home，可以通过 docker exec 命令在 jenkinsci/blueocean 容器的终端或命令行窗口访问 Jenkins 根目录的内容：



docker exec -it &lt;docker-container-name&gt; bash



跟上面一部分的例子一样，&lt;docker-container-name&gt; 可以通过 docker ps 命令得到。如果在 docker run ... 命令中使用了 --name jenkins-blueocean 选项（参考 Accessing the Jenkins/Blue Ocean Docker container），则可以直接使用 docker exec 命令：



docker exec -it jenkins-blueocean bash

