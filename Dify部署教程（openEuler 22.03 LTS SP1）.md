
### 环境要求

在安装 Dify 之前，请确保您的机器满足以下最低系统要求：

> 博主系统架构为aarch64

- CPU >= 2 Core
- RAM >= 4GB
- Docker  >= 19.03
- Docker Compose >= 1.25.1 

### 配置yum源

openEuler默认yum源较慢这里替换为华为的源

~~~bash

# 配置华为yum源

cat >/etc/yum.repos.d/openEuler.repo <<EOF
#generic-repos is licensed under the Mulan PSL v2.
#You can use this software according to the terms and conditions of the Mulan PSL v2.
#You may obtain a copy of Mulan PSL v2 at:
#    http://license.coscl.org.cn/MulanPSL2
#THIS SOFTWARE IS PROVIDED ON AN "AS IS" BASIS, WITHOUT WARRANTIES OF ANY KIND, EITHER EXPRESS OR
#IMPLIED, INCLUDING BUT NOT LIMITED TO NON-INFRINGEMENT, MERCHANTABILITY OR FIT FOR A PARTICULAR
#PURPOSE.
#See the Mulan PSL v2 for more details.

[OS]
name=OS
baseurl=https://mirrors.huaweicloud.com/openeuler/openEuler-22.03-LTS-SP1/OS/$basearch/
enabled=1
gpgcheck=1
gpgkey=https://mirrors.huaweicloud.com/openeuler/openEuler-22.03-LTS-SP1/OS/$basearch/RPM-GPG-KEY-openEuler

[everything]
name=everything
baseurl=https://mirrors.huaweicloud.com/openeuler/openEuler-22.03-LTS-SP1/everything/$basearch/
enabled=1
gpgcheck=1
gpgkey=https://mirrors.huaweicloud.com/openeuler/openEuler-22.03-LTS-SP1/everything/$basearch/RPM-GPG-KEY-openEuler

[EPOL]
name=EPOL
baseurl=https://mirrors.huaweicloud.com/openeuler/openEuler-22.03-LTS-SP1/EPOL/main/$basearch/
enabled=1
gpgcheck=1
gpgkey=https://mirrors.huaweicloud.com/openeuler/openEuler-22.03-LTS-SP1/OS/$basearch/RPM-GPG-KEY-openEuler

[debuginfo]
name=debuginfo
baseurl=https://mirrors.huaweicloud.com/openeuler/openEuler-22.03-LTS-SP1/debuginfo/$basearch/
enabled=1
gpgcheck=1
gpgkey=https://mirrors.huaweicloud.com/openeuler/openEuler-22.03-LTS-SP1/debuginfo/$basearch/RPM-GPG-KEY-openEuler

[source]
name=source
baseurl=https://mirrors.huaweicloud.com/openeuler/openEuler-22.03-LTS-SP1/source/
enabled=1
gpgcheck=1
gpgkey=https://mirrors.huaweicloud.com/openeuler/openEuler-22.03-LTS-SP1/source/RPM-GPG-KEY-openEuler

[update]
name=update
baseurl=https://mirrors.huaweicloud.com/openeuler/openEuler-22.03-LTS-SP1/update/$basearch/
enabled=1
gpgcheck=1
gpgkey=https://mirrors.huaweicloud.com/openeuler/openEuler-22.03-LTS-SP1/OS/$basearch/RPM-GPG-KEY-openEuler

[update-source]
name=update-source
baseurl=https://mirrors.huaweicloud.com/openeuler/openEuler-22.03-LTS-SP1/update/source/
enabled=1
gpgcheck=1
gpgkey=https://mirrors.huaweicloud.com/openeuler/openEuler-22.03-LTS-SP1/source/RPM-GPG-KEY-openEuler
EOF

 
~~~

 添加docker源

~~~bash

 yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo


~~~
> 说明：`docker-ce.repo` 中用 `$releasever` 变量代替当前系统的版本号，该变量在 `CentOS` 中有效，但在 `openEuler` 中无效，所以将该变量直接改为`7`。

生成缓存及索引

~~~bash
yum makecache
~~~

### 安装docker和docker-compose

列出可安装的docker版本
~~~
 yum list docker-ce --showduplicates|sort -r
~~~
下载想要安装的版本，这里下载最新的
~~~
yum install docker-ce -y
~~~
下载docker-compose
> 这里要用dnf命令下载
~~~bash
dnf install -y docker-compose-plugin
~~~

### 配置docker

配置docker镜像源

> 默认源无法下载所以这里添加docker镜像源

~~~bash
cat >/etc/docker/daemon.json<<EOF
{
  "registry-mirrors": [
        "https://hub.geekery.cn/",
        "https://ghcr.geekery.cn"
        ]
}
EOF
~~~

启动docker并设置开机自启动
~~~bash
systemctl enable docker --now
~~~

### 安装Dify

安装git工具

~~~bash
yum -y install git
~~~

下载dify

~~~bash
 git clone https://github.com/langgenius/dify.git
~~~

安装dify

> 如docker-compose版本为V1，请使用docker-compose up -d 命令

~~~bash
cd dify/docker
cp .env.example .env
docker compose up -d
~~~

等待所有容器启动后访问 [http://localhost/install](http://localhost/install)进入 Dify 控制台并开始初始化安装操作。
![image](https://github.com/simba-cn/-/blob/main/Pasted%20image%2020241019160034.png)
