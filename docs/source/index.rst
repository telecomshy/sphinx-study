.. sphinx-study documentation master file, created by
   sphinx-quickstart on Thu Sep 14 09:11:04 2023.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

sphinx和reStructuredText私房手册
========================================

sphinx是一个工具，一个python三方包，安装sphinx以后运行几条指令就可以自动生成项目文档模板，并且可以自动抽取代码中的docstring，
生成静态的html页面。基本上，你看到的所有python三方包的官方文档都是使用sphinx生成的。

reStructuredText是python官方推荐的编写文档的一种语法，和流行的markdown类似，功能更强大。sphinx最早基于reStructuredText，
简单来说就是用reStructuredText语法编写 *.rst* 文件，然后利用sphinx生成静态网页。另外，sphinx对原生的reStructuredText语法进行
了扩展，添加了很多角色, :ref:`指令 <tutorials/rst:指令(Directives)>`, 这点需要注意。

这个教程是我学习sphinx和reStructuredText的笔记，主要分为两部分：

一部分是主要是关于sphinx的具体操作，常见的配置，常用的插件，以及记录使用过程中遇到的一些问题。

另外一部分主要是关于reStructuredText语法，reStructuredText不太容易掌握，因此这部分主要通过具体的例子，记录reStructuredText中
易错或者难以理解的一些概念。

.. toctree::
   :maxdepth: 2

   tutorials/sphinx
   tutorials/rst
   tutorials/questions

