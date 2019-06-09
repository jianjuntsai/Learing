# Anaconda

[参考网站](https://www.zhihu.com/question/58033789?sort=created)

查看已经安装的内容

```
conda list
```

升级

```
conda upgrade  --all
```

安装包

```
conda install package_name
```

卸载包

```
conda remove package_nammes
```

更新包

```
coonda update package_name
```

创建环境

```
conda create -n env_name package_names
```



## Managing Environments

### 1.Create a new environment and install a package in it

We will name the environment snowflakes and install the package BioPython. At the Anaconda Prompt or in your terminal window, type the following:

```
conda create --name [the name of environment] biopython
```

Conda checks to see what additional packages ("dependencies") Biopython will need, and asks if you want to proceed:

```
Proceed ([y]/n)? y
```
Type "y" and press Enter to proceed.

### 2.To use, or "activate" the new environment, type the following

conda activate snowflakes

### 3.To see a list of all your environments, type:

```
conda info --envs
```

### 4.Change your current environment back to the default (base): conda activate

```
conda activate
```

## Managing Python
When you create a new environment, conda installs the same Python version you used when you downloaded and installed Anaconda. If you want to use a different version of Python, for example Python 3.5, simply create a new environment and specify the version of Python that you want.

### Create a new environment named "snakes" that contains Python 3.5:

```
conda create --name snakes python=3.5
```
### Activate the new environment:

```
source activate snakes
```

### Deactivate the snakes environment and return to base environment: conda activate

```
source activate
```

[以上内容来自](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html#managing-environments)



