## 软件安装

*RPM Fusion 是为 Fedora、RHEL（包括 CentOS、AlmaLinux、Rocky Linux 等）准备的一大软件库，主要提供Red Hat 系统中未包含但是非常流行且有自由许可（或者有法律风险，但是由社区提供）的软件。*

    dnf install -y epel-release
    dnf install -y https://download1.rpmfusion.org/free/el/rpmfusion-free-release-8.noarch.rpm
    dnf install -y https://download1.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-8.noarch.rpm

**清华镜像:CentOS/RHEL(8/9) 安装RPMFusion 软件仓库 https://mirrors.tuna.tsinghua.edu.cn/help/rpmfusion/**

<pre>yum localinstall --nogpgcheck https://mirrors.tuna.tsinghua.edu.cn/rpmfusion/free/el/rpmfusion-free-release-8.noarch.rpm https://mirrors.tuna.tsinghua.edu.cn/rpmfusion/nonfree/el/rpmfusion-nonfree-release-8.noarch.rpm</pre>  

*安装exfat插件*

    dnf install -y fuse-exfat exfatprogs

*安装中文语言包*
    
    dnf install glibc-langpack-zh -y

*Linux安装高版本gcc 9、10、11、12*

    #简单一键安装
    sudo dnf install gcc-toolset-10* -y
    
    #默认安装在:/opt/rh/gcc-toolset-10/,需要设置软件链接这一部分很重要
    #https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/developing_c_and_cpp_applications_in_rhel_9/assembly_additional-toolsets-for-development-rhel-9_developing-applications#ref_specifics-of-annobin-in-gcc-toolset-12_gcc-toolset-12
    cd /opt/rh/gcc-toolset-12/root/usr/lib/gcc/architecture-linux-gnu/10/plugin
    ln -s annobin.so gcc-annobin.so
    
    #命令行临时启用指定版本的gcc-toolset-N
    scl enable gcc-toolset-10 'bash'

*安装docker*
    
    #安装系统依赖
        sudo yum install -y yum-utils device-mapper-persistent-data lvm2

    #设置yum源(默认)
        sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

    #阿里镜像源
        sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
        sudo yum makecache fast

    #安装完成后即可查看
        /etc/yum.repos.d/docker-ce.repo

    #查看可安装的docker版本
        dnf list --showduplicates docker-ce

    #默认安装
        dnf -y install docker-ce docker-ce-cli containerd.io

    #默认的docker data 数据会存放在/var/lib/docker，修改存储地址
        rsync -aqxP /var/lib/docker /staging/docker
        touch /etc/docker/daemon.json #如果不存在
        在/etc/docker/daemon.json文件中添加
        {"data-root": "/staging/docker/",
        "experimental": true}
    #设置docker pull的镜像源 选择网址：https://github.com/dongyubin/DockerHub中可用的链接添加到/etc/docker/daemon.json文件中例如：
        {
          "registry-mirrors": [
            "https://dytt.online",
            "https://lispy.org"，
          ]
        }
    #启动docker
        systemctl start docker
    #设置开机自启动
        systemctl enable docker.service

## 安装glibc以及gcc

    dnf groupinstall "Development Tools"
    dnf install gcc gcc-c++ make kernel-headers glibc-devel

## vim 基本命令查找和替换
(命令模式)移动光标
```{.cs}
在vim界面，命令模式下光标移动方法::

    :set  nu     //显示行号
    :set nonu    //取消显示行号
    n+        //向下跳n行
    n-         //向上跳n行
    nG        //跳到行号为n的行
    G           //跳至文件的底部
    g         //跳转到文件头部
```

(插入模式)按键进入编辑插入模式
```{.cs}
    a      //在当前光标位置的右边添加文本
    i       //在当前光标位置的左边添加文本
    A     //在当前行的末尾位置添加文本
    I      //在当前行的开始处添加文本(非空字符的行首)
    O     //在当前行的上面新建一行
    o     //在当前行的下面新建一行
    R    //替换(覆盖)当前光标位置及后面的若干文本
    J    //合并光标所在行及下一行为一行(依然在命令模式)
```
(命令模式)删除和复制，在vim中除了在编辑模式下修改文件，命令模式的时候可以删除和复制
```{.cs}
    x         //删除当前字符
    nx         //删除从光标开始的n个字符
    dd       //删除当前行
    ndd      //向下删除当前行在内的n行
    u        //撤销上一步操作
    U        //撤销对当前行的所有操作
    yy       //将当前行复制到缓存区，也可以用 "ayy 复制，"a 为缓冲区，a也可以替换为a到z的任意字母，可以完成多个复制任务。
    nyy      //将当前行向下n行复制到缓冲区，也可以用 "anyy 复制，"a 为缓冲区，a也可以替换为a到z的任意字母，可以完成多个复制任务。
    yw       //复制从光标开始到词尾的字符。
    nyw      //复制从光标开始的n个单词。
    y^       //复制从光标到行首的内容。  VPS侦探
    y$       //复制从光标到行尾的内容。
    p        //粘贴剪切板里的内容在光标后，如果使用了前面的自定义缓冲区，建议使用"ap 进行粘贴。
    P        //粘贴剪切板里的内容在光标前，如果使用了前面的自定义缓冲区，建议使用"aP 进行粘贴。
```
(命令模式)搜索和替换，命令模式下(esc退出插入模式)
```{.cs}
    /keyword     //向光标下搜索keyword字符串，keyword可以是正则表达式
    ?keyword     //向光标上搜索keyword字符串
    n           //向下搜索前一个搜素动作
    N         //向上搜索前一个搜索动作

    *(#)      //当光标停留在某个单词上时, 输入这条命令表示查找与该单词匹配的下(上)一个单词. 同样, 再输入 n 查找下一个匹配处, 输入 N 反方向查找.

    g*(g#)        //此命令与上条命令相似, 只不过它不完全匹配光标所在处的单词, 而是匹配包含该单词的所有字符串.

    :s/old/new      //用new替换行中首次出现的old
    :s/old/new/g         //用new替换行中所有的old
    :n,m s/old/new/g     //用new替换从n到m行里所有的old
    :%s/old/new/g      //用new替换当前文件里所有的old
```

添加用户有sudo命令权限：

    adduser <用户名> sudo

Linux下创建用户

<pre>useradd -s /bin/sh -g group1 –G adm,root gem -m fanyucai -d /home/fanyucai</pre>

    -s 用户解释器
    -g 用户所属用户组
    -G 用户所属的其他组
    -m 用户登录名
    -d  用户目录