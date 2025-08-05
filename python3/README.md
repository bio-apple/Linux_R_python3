1.  指定镜像安装python模块,可用镜像阿里云:[https://mirrors.aliyun.com/pypi/](https://mirrors.aliyun.com/pypi/)

        pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple numpy

2.   常用python模块

**TensorFlow: an open source machine learning framework for everyone.** https://www.tensorflow.org/
    
    #直接安装tensorflow_decision_forests会自动兼容安装tensorflow、pandas、numpy
    pip3 install tensorflow_decision_forests --upgrade

**scikit-learn: A set of python modules for machine learning and data mining.** https://scikit-learn.org/stable/

    pip3 install -U scikit-learn

**seaborn: statistical data visualization**

    #会附带安装matplotlib
    pip3 install seaborn