### 代码分支
- master: 开发源码
- gh-pages: 网站源码

### 将jupyter自动发布到Github Pages

环境搭建
- 利用anconda创建一个干净的开发环境
```
conda create -n jupyter_env python=3.7.0
activate jupyter_env
pip install mkdocs
```

- 创建一个Github Pages仓库

- 创建一个mkdocs项目
```
mkdocs new pydataAnalysis
git init
git remote add origin git@github.com:DepInjoy/pydataAnalysis.git
```

- 修改mkdocs.yml配置文件
```
pages:
- {Home: index.md}
site_name: 数据分析和挖掘
theme: readthedocs
```

- 创建Jupyter配置文件
```
# 创建配置文件和代码的存放路径
mkdir config jupyters
# 生成配置文件
jupyter notebook --generate-config
cp C:/Users/username/.jupyter/jupyter_notebook_config.py config/
```

- 修改Jupyter配置文件
在配置文件的末尾追加：
```
def output_post_save(model, os_path, contents_manager):
	pass
c.FileContentsManager.root_dir = 'jupyters'
c.FileContentsManager.post_save_hook = output_post_save
```

- 运行jupyter
```
jupyter notebook --config config/jupyter_notebook_config.py
```

- .ipynb转化为md,并复制到docs目录下

- 将添加的配置文件添加至mkdocs.yml[参考博客](https://markdown-docs-zh.readthedocs.io/zh_CN/latest/)

```
pages:
- {Home: index.md}
- {Untitled: Untitled.md}
```



- 编译
```
mkdocs build --clean
```

- 本地查看
```
mkdocs serve
```
远程只需要将site推送上去即可。