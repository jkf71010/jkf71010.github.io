# docker安装

### 运行环境

​	操作系统：centos 8.1 64位

​	硬件环境：阿里云1u2G

### 安装过程

1.   使用官方安装脚本自动安装：

   ```
   curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
   ```

2. 安装命令

   ```
   # step 1: 安装必要的一些系统工具
   sudo yum install -y yum-utils device-mapper-persistent-data lvm2
   # Step 2: 添加软件源信息
   sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
   # Step 3: 更新并安装 Docker-CE
   sudo yum makecache fast
   sudo yum -y install docker-ce
   # Step 4: 开启Docker服务
   sudo systemctl start docker
   
   注意：其他注意事项在下面的注释中
   # 官方软件源默认启用了最新的软件，您可以通过编辑软件源的方式获取各个版本的软件包。例如官方并没有将测试版本的软件源置为可用，你可以通过以下方式开启。同理可以开启各种测试版本等。
   # vim /etc/yum.repos.d/docker-ce.repo
   #   将 [docker-ce-test] 下方的 enabled=0 修改为 enabled=1
   # 卸载旧版本
   # sudo yum remove docker  docker-common docker-selinux docker-engine
   # 安装指定版本的Docker-CE:
   # Step 1: 查找Docker-CE的版本:
   # yum list docker-ce.x86_64 --showduplicates | sort -r
   #   Loading mirror speeds from cached hostfile
   #   Loaded plugins: branch, fastestmirror, langpacks
   #   docker-ce.x86_64            17.03.1.ce-1.el7.centos            docker-ce-stable
   #   docker-ce.x86_64            17.03.1.ce-1.el7.centos            @docker-ce-stable
   #   docker-ce.x86_64            17.03.0.ce-1.el7.centos            docker-ce-stable
   #   Available Packages
   # Step2 : 安装指定版本的Docker-CE: (VERSION 例如上面的 17.03.0.ce.1-1.el7.centos)
   # sudo yum -y install docker-ce-[VERSION]
   # 注意：在某些版本之后，docker-ce安装出现了其他依赖包，如果安装失败的话请关注错误信息。例如 docker-ce 17.03 之后，需要先安装 docker-ce-selinux。
   # yum list docker-ce-selinux- --showduplicates | sort -r
   # sudo yum -y install docker-ce-selinux-[VERSION]
   
   # 通过经典网络、VPC网络内网安装时，用以下命令替换Step 2中的命令
   # 经典网络：
   # sudo yum-config-manager --add-repo http://mirrors.aliyuncs.com/docker-ce/linux/centos/docker-ce.repo
   # VPC网络：
   # sudo yum-config-manager --add-repo http://mirrors.could.aliyuncs.com/docker-ce/linux/centos/docker-ce.repo
   ```

3. docker-compose安装

   ```
   #从github上更新docker-compose
   curl -L https://github.com/docker/compose/releases/download/1.24.0-rc1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
   #验证docker-compose版本
   docker-compose --version
   ```

### CentOS 8 安装注意事项

```
# 安装过程中报错
Error: 
 Problem: package docker-ce-3:19.03.12-3.el7.x86_64 requires containerd.io >= 1.2.2-3, but none of the providers can be installed
  - cannot install the best candidate for the job
  - package containerd.io-1.2.10-3.2.el7.x86_64 is filtered out by modular filtering
  - package containerd.io-1.2.13-3.1.el7.x86_64 is filtered out by modular filtering
  - package containerd.io-1.2.13-3.2.el7.x86_64 is filtered out by modular filtering
  - package containerd.io-1.2.2-3.3.el7.x86_64 is filtered out by modular filtering
  - package containerd.io-1.2.2-3.el7.x86_64 is filtered out by modular filtering
  - package containerd.io-1.2.4-3.1.el7.x86_64 is filtered out by modular filtering
  - package containerd.io-1.2.5-3.1.el7.x86_64 is filtered out by modular filtering
  - package containerd.io-1.2.6-3.3.el7.x86_64 is filtered out by modular filtering
  
 # 执行
 dnf install https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm
 
 # 然后重新执行docker安装命令：
 	yum install  docker-ce 
```



[​返回上级](https://jkf71010.github.io/ )​