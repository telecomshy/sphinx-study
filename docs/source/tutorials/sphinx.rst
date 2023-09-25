sphinx私房手册
=========================

使用sphinx生成文档的基本套路是：

1. 安装sphinx
2. 使用sphinx生成文档框架
3. 进行自定义修改
4. 上传到 `RTD <https://readthedocs.org/dashboard/>`_ (或者上传到github，登陆RTD，关联github)
5. 构建生成在线文档

第一步就按照 `官方文档 <https://www.osgeo.cn/sphinx/index.html>`_ 一步一步操作就行，从
`入门 <https://www.osgeo.cn/sphinx/usage/quickstart.html#>`_
看到 `教程 <https://www.osgeo.cn/sphinx/tutorial/index.html>`_ 章节，应该没什么问题。主要的难点
是生成文档框架以后，如何进行自定义修改。

官方文档中，直接开始介绍reStructuredText语法，中间缺少一个对生成文档结构的介绍，这会让人觉得有点懵，起码
我当时在这里卡住了，学了一堆reStructuredText语法，感觉好像还是不知道从哪里下手。这就好像还不清楚房子的整
体结构，直接就开始搬砖，因此，我们首先看看生成的文档结构是怎样的：

文档结构
-------------------


常用配置
-------------------

- `configuration <https://www.sphinx-doc.org/en/master/usage/configuration.html>`_

html_sidebars
~~~~~~~~~~~~~~~~~~~~~

常用插件
--------------------

sphinx.ext.napoleon
~~~~~~~~~~~~~~~~~~~~~~

reStructuredText写docstrings看起来有点混乱，除了直接用reStructuredText写docstrings，常见的还有google风格和numpy风格。
这两种风格比较起来更简洁和清爽。比如，著名的pandas使用的就是numpy风格的docstrings。

`napoleon` 是一个预处理器，它可以把google或者numpy风格的docstrings转换成reStructuredText,

- `sphinx.ext.napoleon <https://www.sphinx-doc.org/en/master/usage/extensions/napoleon.html>`_
- `google风格的例子 <https://www.sphinx-doc.org/en/master/usage/extensions/example_google.html>`_
- `numpy风格的例子 <https://www.sphinx-doc.org/en/master/usage/extensions/example_numpy.html#example-numpy>`_

如果使用pycharm，可以在设置->工具->python集成工具->docstrings中，选择自动生成相应的风格。

常见问题
--------------------

Q1: 如何定制侧边栏
~~~~~~~~~~~~~~~~~~~~~~~

参考：`sphinx.ext.napoleon`_