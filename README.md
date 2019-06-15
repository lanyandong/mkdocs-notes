# Notes

使用MkDocs构建的笔记文档，构建部署流程大致如下：

环境：

- 支持Windows/Linux/macOS
- 需要安装 Python 2.7 +
- 需要 pip

1、安装MkDocs 

```
$ pip install mkdocs
```

运行 `mkdocs help` 可以检查是否正确安装，如果失败检查一下pip版本，需要升级到5.0以上，pip升级命令：

```
python -m pip install --upgrade pip
```



2、输入以下命令构建项目

```
$ mkdocs new project
```

`project` 是指项目名，你要在那个目录下创建，就切换到那个目录下  Windows的cmd 命令行和Linux的终端命令都是类似操作。

构建好的项目里有一个配置文件 `mkdocs.yml`, 和一个包含文档源码的 `docs` 文件夹. 在 `docs` 文件夹里包含了一个名为 `index.md` 的文档. 



3、启动内建服务器，进行预览

```
$ mkdocs serve
```

它会自动使用8000 端口，在浏览器中打开` <http://127.0.0.1:8000/> `就可以完成效果的预览。

之后就是修改配置文件 `mkdocs.yml` ，自己可以添加一些配置信息，包括站点名称，文档页面等，很简单。



4、使用以下命令,创建新的页面

```
$ curl 'jaspervdj.be/lorem-markdownum/markdown.txt' > docs/about.md
```



5、主要是修改配置文件 `mkdocs.yml`，这个决定了你的站点结构。

```yml
site_name: Learning Notes

nav:
- 主页: index.md
- 课程笔记:
        - 操作系统: notes/OS-notes.md
        - 计算机网络: notes/computer-network.md
        - 计算机组原: notes/computer-design.md
        - 系统结构: notes/system-structure.md
        - 软件工程: notes/software-engineering.md
        

- 数据结构-算法:
       - 数据结构: notes/data-structure.md
       - 算法分析: notes/algorithm-analysis.md

- 编程语言:
       - Java程序设计: notes/java.md
       - C++程序设计: notes/cpp.md
       - Python编程基础: notes/python.md
        
- 其他: about.md

theme: readthedocs
```



6、站点生成

```
$ mkdocs build
```

该命令创建了一个 `site` 新目录. 里面的文件就是你的站点资源文件，你可以把这些文件部署到服务器上或者github page上，就可以进行访问了。



7、参考文档

如果想要了解更多更详细的内容可以参考官方指南[MkDocs 中文文档](https://markdown-docs-zh.readthedocs.io/zh_CN/latest/)

