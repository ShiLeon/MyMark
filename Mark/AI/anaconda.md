# 环境管理

```
conda create -n xxx python=3.11
conda remove --name xxx --all
activate xxx //激活环境
deactivate xxx
conda env list //显示所有conda环境
```

# 包管理

```
conda install xxx
conda uninstall xxx
conda update xxx

```

## 32 64位平台切换

```
set CONDA_FORCE_32BIT=1
conda env config vars set CONDA_FORCE_32BIT=1
```

