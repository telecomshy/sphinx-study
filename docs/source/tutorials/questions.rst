常见问题
===============

如何使用内联语法高亮？
~~~~~~~~~~~~~~~~~~~~~~~

语法高亮我们应该非常熟悉了，就是使用 ``.. code-block::`` 指令，但是如何在文本之中使用语法高亮呢？具体做法
参见 :ref:`code角色 <tutorials/restructuredtext:code>`

如何使指令全局可用？
~~~~~~~~~~~~~~~~~~~~~~~

参见 :ref:`rst_prolog配置 <tutorials/sphinx:rst_prolog>`

如何写符合规范的docstring？另外，为什么不同包的docstring看起来好像有所不同？
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


参见 :ref:`napoleon插件 <tutorials/sphinx:sphinx.ext.napoleon>`

如何查看当前所有可用的target?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

可以使用命令 ``python -m sphinx.ext.intersphinx https://docs.readthedocs.io/en/stable/objects.inv`` ，
查看项目所有可用的target。

objects.inv也可以是本地文件，一般在docs/build/html目录下。

某个函数或模块的reference太长了，有没有什么快捷的写法？
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

比如想要链接到 ``Queue`` 模块的 ``get`` 方法，正常写是这样： :rst:`:py:meth:\`queue.Queue.get\`` ，渲染
出来是 :py:meth:`queue.Queue.get` , 如果渲染结果只想显示 ``get`` ，则可以在前面加 ``~`` 号，写成
:rst:`:py:meth:\`~queue.Queue.get\`` ，渲染出来就是： :py:meth:`~queue.Queue.get` 。

虽然最终显示的结果变短了，但是写的时候还是要写完整的路径。如果是自己的项目文档，此时可以在前面加 ``.`` ，比如前面的
例子，可以写成 :rst:`:py:meth:\`.get\`` ， ``~`` 和 ``.`` 可以同时使用，写成 :rst:`:py:meth:\`~.get\``。

.. attention::

    只有自己的项目可以使用 ``.`` 简化，使用intersphinx引用的外部项目还是要写完整路径