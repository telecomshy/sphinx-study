reStructuredText私房手册
==========================

块和列表
-------------------------

- 参考链接：`reStructuredText basics <https://www.osgeo.cn/sphinx/usage/restructuredtext/basics.html#rst-field-lists>`_

除了普通的块和列表，有几个特殊的块不太好理解：

定义列表 (`Definition Lists <https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#definition-lists>`_)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    这个定义不是动词是名称。比如下面的例子，`term` 是一个名词，`Definition of the term, which must be indented` 是对它的解释：

    .. code-block:: rst

        term (up to a line of text)
            Definition of the term, which must be indented

            and can even consist of multiple paragraphs

        next term
            Description.

    渲染出来是这样：

    term (up to a line of text)
        Definition of the term, which must be indented

        and can even consist of multiple paragraphs

    next term
        Description.

    注意，名词和它的解释之间没有空行。

引用块 (`Block Quotes <https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#block-quotes>`_)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    引用块顾名思义，就是引用别人的话，格式和定义列表很像，区别就是第一行和第二行之间有空行：

    .. code-block:: rst

        This is an ordinary paragraph, introducing a block quote.

            "It is my business to know things.  That is my trade."

            -- Sherlock Holmes

    渲染结果是：

    This is an ordinary paragraph, introducing a block quote.

        "It is my business to know things.  That is my trade."

        -- Sherlock Holmes

文字块 (`Literal Blocks <https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#literal-blocks>`_)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    所谓文字块，就是对任何标记不进行渲染，文字内容写出来是什么样，展示出来就怎么样。文字块和引用块很类似，区别是文字块后面是两个冒号。

    .. code-block:: rst

        This is a normal text paragraph. The next paragraph is a code sample::

           It is not processed in any way, except
           that the indentation is removed.

           It can span multiple lines.

        This is a normal text paragraph again.

    渲染的结果是：

    This is a normal text paragraph. The next paragraph is a code sample::

        It is not processed in any way, except
        that the indentation is removed.

        It can span multiple lines.

    This is a normal text paragraph again.



交叉引用
---------------------

- 参考链接：`cross referencing with sphinx <https://docs.readthedocs.io/en/stable/guides/cross-referencing-with-sphinx.html#explicit-targets>`_

所谓交叉引用 `cross reference` 就是一个链接。reStructuredText不仅可以链接一个URL，还可以链接到任意文档的任意位置，
甚至可以链接到其它项目的文档，功能非常强大。但不是很容易掌握。

外部URL
~~~~~~~~~~~~~~~~~~~~

如果要链接到外部的url，以下两种写法是一样的：

.. code-block:: rst

    Python website is: `<http://www.python.org/>`_
    Python website is: http://www.python.org/

渲染结果均为：Python website is `<http://www.python.org/>`_ 。如果不想直接显示URL，想以文字代替，则可以：

.. code-block:: rst

    Python website is: `python <http://www.python.org/>`_

渲染结果为：Python website is `python <http://www.python.org/>`_

显式target
~~~~~~~~~~~~~~~~~~~~

我们还可以把引用 `reference` 和目标地址 `target` 分开。比如，在文档中这样写：

``ref`` **角色**

``doc`` **角色**

除了可以链接到章节，还可以使用 ``doc`` 角色链接到文件，比如：

.. code-block:: rst

    链接到根目录的 `index.rst` 文件 :doc:`/index`

渲染结果为：链接到根目录的 `index.rst` 文件 :doc:`/index` ，可见，渲染出来的文字内容是目标文件的第一个标题。当然也可以用前面的方法，自定义
渲染内容。