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