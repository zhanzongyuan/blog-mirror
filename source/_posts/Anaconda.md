---
title: Anaconda入门知识
tags:
  - Anaconda
  - conda
categories:
  - 计算机科学
description: Anaconda入门知识
mathjax: true
date: 2018-04-18 10:29:36
---



> references:
>
> -  https://www.zhihu.com/question/58033789
> - https://www.jianshu.com/p/2f3be7781451
> - anaconda官方文档：https://www.anaconda.com
> - conda官方文档：https://conda.io/docs/user-guide/tasks/index.html
>
> 安装Anaconda常见问题：https://zhuanlan.zhihu.com/p/34337889

## 1. Anaconda和conda概念

conda是包管理器和环境管理器，相当于结合了pip和virtualenv的功能

Anaconda在英文中是“蟒蛇”，Anaconda是一个包含180+的科学包及其依赖项的发行版本。

>  这里先解释下conda、anaconda这些概念的差别。`conda`可以理解为一个工具，也是一个可执行命令，其核心功能是**包管理**与**环境管理**。包管理与pip的使用类似，环境管理则允许用户方便地安装不同版本的python并可以快速切换。Anaconda则是一个**打包的集合**，里面预装好了conda、某个版本的python、众多packages、科学计算工具等等，所以也称为Python的一种发行版。其实还有Miniconda，顾名思义，它只包含最基本的内容——python与conda，以及相关的必须依赖项，对于空间要求严格的用户，Miniconda是一种选择。作者：PeterYuan链接：https://www.jianshu.com/p/2f3be7781451來源：简书著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

Anaconda 附带了一大批常用数据科学包，它附带了 conda、Python 和 150 多个科学包及其依赖项。因此你可以立即开始处理数据。

Anaconda 是在 conda（一个包管理器和环境管理器）上发展出来的。

## 2. conda使用

### 2.1 管理安装包

在数据分析中，你会用到很多第三方的包，而conda（包管理器）可以很好的帮助你在计算机上安装和管理这些包，包括安装、卸载和更新包。

```bash
conda list
conda upgrade    --all
conda search nums

# 管理包操作：安装 卸载 更新
conda install package_name
conda remove package_name
conda update package_name
```

conda 还会自动为你安装依赖项。例如，scipy 依赖于 numpy，因为它使用并需要 numpy。如果你只安装 scipy (conda install scipy)，则 conda 还会安装 numpy（如果尚未安装的话）。

### 2.2 管理环境

conda 可以为你不同的项目建立不同的运行环境。

0. 安装nb_conda用于notebook自动关联nb_conda的环境。
1. 创建环境

```bash
conda create -n env_name package_names
conda create -n py3 pandas # 要创建环境名称为 py3 的环境并在其中安装 numpy
```

2. 创建环境时，可以指定要安装在环境中的 Python 版本

```bash
conda create -n py3 python=3 
conda create -n py2 python=2
conda create -n py python=3.6 # 如果你要安装特定版本（例如 Python 3.6）
```

3. 进入环境

```bash
source activate my_env
```

4. 离开环境
```bash
source deactivate
```

5. 共享环境

```bash
conda env export > environment.yaml # 将你当前的环境保存到文件中包保存为YAML文件（包括Pyhton版本和所有包的名称）。
conda env update -f=/path/to/environment.yml # 其中-f表示你要导出文件在本地的路径，所以/path/to/environment.yml要换成你本地的实际路径
```

```bash
pip install -r /path/requirements.txt # 对于不使用conda
```

6. 列出环境

```bash
conda env list
```

7. 删除环境

```bash
conda env remove -n env_name
```
