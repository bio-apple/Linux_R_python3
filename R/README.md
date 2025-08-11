# CentOS 8 上安装 RStudio Server

## 1.系统准备
<pre>
#确保系统上的所有软件包都是最新版本
sudo dnf update

#安装 EPEL 仓库
dnf install epel-release -y

#安装必要的开发工具和依赖
dnf groupinstall -y "Development Tools"
dnf install -y gcc-c++ readline-devel zlib-devel bzip2-devel pcre2-devel libcurl-devel openssl-devel libxml2-devel gdal-devel freetype-devel libpng-devel libtiff-devel libjpeg-devel harfbuzz-devel fribidi-devel
dnf install -y hdf5 hdf5-devel python3-pip python3-devel libjpeg-turbo libjpeg-turbo-devel cmake proj-devel sqlite-devel cairo-devel geos geos-devel
</pre>

## 2.安装R
<pre>dnf install -y R-4.2.0</pre>

## 3.安装RStudio Server：https://posit.co/download/rstudio-server/
<pre>
wget https://download2.rstudio.org/server/rhel8/x86_64/rstudio-server-rhel-2025.05.1-513-x86_64.rpm
sudo yum install rstudio-server-rhel-2025.05.1-513-x86_64.rpm

#在停止服务的状态验证安装完成性
systemctl stop rstudio-server
sudo rstudio-server verify-installation
</pre>

## 4.配置R路径/etc/rstudio/rserver.conf
<pre>rsession-which-r=/staging/software/anaconda3/envs/sesame/bin/R</pre>

## 5.确认端口未被占用
<pre>
#确保 RStudio Server 默认端口（8787）未被其他进程占用。使用以下命令检查端口：
sudo netstat -tuln | grep 8787

#如果端口已被占用，可以更改 RStudio Server 使用的端口。在 /etc/rstudio/rserver.conf 中添加以下内容来更改端口：
www-port=8788

#检查防火墙状态
systemctl status firewalld

#若未运行，可先启动
sudo systemctl start firewalld
sudo systemctl enable firewalld

#开放 8787 端口
firewall-cmd --permanent --add-port=8787/tcp
firewall-cmd --reload

#确认端口是否开放
firewall-cmd --list-ports
</pre>

## 6.常用rstudio-server命令
<pre>
#启用 rstudio-server 服务：创建开机自动启动的符号链接
systemctl enable rstudio-server

#启动服务器
systemctl start rstudio-server

#查看服务器状态
systemctl status rstudio-server 

#重启RStudio Server
systemctl restart rstudio-server

#停止服务
systemctl stop rstudio-server
</pre>

## 7.SELinux 可能限制访问，临时关闭测试(非必须)
<pre>
#关闭 SELinux
setenforce 0

#查看 SELinux 状态
getenforce

常见状态：
Enforcing（强制执行）
Permissive（警告模式，不阻止，只记录）
Disabled（关闭）
</pre> 

## 8.如果遇到登录或启动 R session 的问题，可以看日志：
<pre>
sudo journalctl -u rstudio-server -f

#或者
cat /var/log/rstudio/rstudio-server.log | tail -n 20
</pre>

## 9.SSSD (System Security Services Daemon)，它是一个用于管理与远程身份验证服务（如 LDAP 或 Kerberos）交互的守护进程
<pre>
#清除缓存文件
rm -rf /var/lib/sss/db/*

#然后重启 SSSD 服务
systemctl restart sssd

#检查服务状态
systemctl status sssd

#升级 SSSD
dnf upgrade sssd
</pre>

## 10.允许 root 登录
<pre>
#添加 auth-minimum-user-id=0  到/etc/rstudio/rserver.conf文件
</pre>

## 11.查看进程
<pre>ps aux | grep rserver</pre>

# 资源链接

[R语言学习资源:Cookbook for R](http://www.cookbook-r.com)

[R语言学习资源:tidyverse](https://www.tidyverse.org/)

[R语言学习资源:ggplot2](https://ggplot2-book.org)

[R绘制热图heatmap](https://flowingdata.com/2010/01/21/how-to-make-a-heatmap-a-quick-and-easy-solution/)

[RColorBrewer画图颜色选择](http://colorbrewer2.org/)

[linux:install rstudio server](./rstudio_server/README.md)


## Common Methods for Installing R Packages
方法一:

    options(BioC_mirror="http://mirrors.ustc.edu.cn/bioc/")
    source("http://www.bioconductor.org/biocLite.R")
    biocLite("GDCRNATools")
    Rscript -e "BiocManager::install(\"ComplexHeatmap\")"

方法二：

    Rscript -e "install.packages(c(\"BiocManager\"))"

方法三：

    R CMD INSTALL EnrichmentBrowser_2.4.5_CA_edit.tar.gz

## Popular R Packages for Bioinformatics

**Olink**:Facilitate Analysis of Proteomic Data from Olink<br>
https://cran.r-project.org/web/packages/OlinkAnalyze/
<pre>install.packages("OlinkAnalyze")</pre>

**WGCNA**:Weighted Gene Co-expression Network Analysis<br>
https://cran.r-project.org/web/packages/WGCNA/index.html
<pre>install.packages("BiocManager")
BiocManager::install("WGCNA")</pre>

**sesame:**:SEnsible Step-wise Analysis of DNA MEthylation BeadChips<br>
https://bioconductor.org/packages/release/bioc/html/sesame.html
<pre>BiocManager::install("sesame")</pre>

**Seurat**:R toolkit for single cell genomics<br>
https://satijalab.org/seurat/
<pre>install.packages('Seurat')</pre>

**DESeq2**:Differential gene expression analysis based on the negative binomial distribution<br>
https://bioconductor.org/packages/release/bioc/html/DESeq2.html
<pre>BiocManager::install("DESeq2")</pre>

**edgeR**:Empirical Analysis of Digital Gene Expression Data in R<br>
https://bioconductor.org/packages/release/bioc/html/edgeR.html
<pre>BiocManager::install("edgeR")</pre>

**clusterProfile**:A universal enrichment tool for interpreting omics data<br>
https://bioconductor.org/packages/release/bioc/html/clusterProfiler.html
<pre>BiocManager::install(“clusterProfiler")</pre>

**vegan**:COrdination methods, diversity analysis and other functions for community and vegetation ecologists<br>
https://cran.r-project.org/web/packages/vegan/index.html
<pre>BiocManager::install("vegan")</pre>

**devtools**:Tools to Make Developing R Packages Easier<br>
https://cran.r-project.org/web/packages/devtools/index.html
<pre>install.packages("devtools")</pre>

**methylKit**:DNA methylation analysis from high-throughput bisulfite sequencing results<br>
https://www.bioconductor.org/packages/devel/bioc/html/methylKit.html
<pre>BiocManager::install("methylKit")</pre>

**epialleleR**:Fast, Epiallele-Aware Methylation Caller and Reporter<br>
https://www.bioconductor.org/packages/release/bioc/html/epialleleR.html
<pre>BiocManager::install("epialleleR")</pre>

