telecomshy的sphinx学习笔记

交叉引用
=================

参考链接
-----------------

- `cross referencing with sphinx <https://docs.readthedocs.io/en/stable/guides/cross-referencing-with-sphinx.html#explicit-targets>`_

外部URL
-----------------

如果要链接到外部的url，以下两种写法是一样的：

.. code-block:: rst

    Python website is: `<http://www.python.org/>`_
    Python website is: http://www.python.org/

渲染结果均为：Python website is `<http://www.python.org/>`_ 。如果不想直接显示URL，想以文字代替，则可以：

.. code-block:: rst

    Python website is: `python <http://www.python.org/>`_

渲染结果为：Python website is `python <http://www.python.org/>`_

显式target
-------------------

我们还可以把引用 `reference` 和目标地址 `target` 分开。比如，在文档中这样写：

``ref`` **角色**

``doc`` **角色**

除了可以链接到章节，还可以使用 ``doc`` 角色链接到文件，比如：

.. code-block:: rst

    链接到 `index.rst` 文件 :doc:`/index`

渲染结果为：链接到根目录的 `index.rst` 文件 :doc:`/index` ，可见，渲染出来的文字内容是目的文件的第一个标题。当然也可以用前面的方法，自定义
渲染内容。