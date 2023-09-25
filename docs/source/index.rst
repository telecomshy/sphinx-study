.. sphinx-study documentation master file, created by
   sphinx-quickstart on Thu Sep 14 09:11:04 2023.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

sphinx和reStructuredText私房手册
========================================

sphinx是一个工具，一个python三方包，安装sphinx以后运行几条指令就可以自动生成整个项目的全套文档（生成文档框架，然后进行修改）。
基本上，你看到的所有python三方包的官方文档都是使用sphinx生成的。

reStructuredText是python官方推荐的编写文档的一种语法，和流行的markdown类似，功能更强大。sphinx最早基于reStructuredText，
简单来说就是用reStructuredText语法编写 `.rst` 文件，然后利用sphinx生成静态的网页。

这个教程是我学习sphinx和reStructuredText的一些笔记，主要记录学习的一些心得，主要分为两部分，一部分是
记录 `官方文档 <https://www.osgeo.cn/sphinx/index.html>`_ 中没有提到的一些细节，或者说自己学习过程中的一些困惑。
另外，sphinx是基于reStructuredText，reStructuredText不太易学更难精，所以另一部分是通过例子，记录reStructuredText中
易错或者难以理解的一些概念。

.. toctree::
   :maxdepth: 2

   tutorials/sphinx
   tutorials/restructuredtext

