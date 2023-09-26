reStructuredText私房手册
==========================

`标记规范`_ 部分主要是基于docutils官方文档 `reStructuredText Markup Specification <https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html>`_
，对一些不好理解的例子加以详细说明。

`交叉引用`_ 是reStructuredText非常常用，且最大的特点之一，同时也是不太好理解一块内容。这部分主要基于readthedocs的教程 `cross referencing with sphinx <https://docs.readthedocs.io/en/stable/guides/cross-referencing-with-sphinx.html#explicit-targets>`_
并且添加了一些自己的理解。

-----------------------------------

标记规范
+++++++++++++++++++++++++

reStructuredText语法乍一看有点复杂，但是细分下来，其实主要分为两大类，一类是文档元素，或者说块，所谓块，
可以理解成一段内容，按照不同的语法书写的块有不同的解读方式。比如：

.. code-block:: rst

    term
        Definition of the term, which must be indented

这样一行顶头，第二行缩进的块就是所谓 `定义列表(Explicit Markup Blocks)`_ 的东东，渲染的时候会把 ``term`` 加粗加黑。
而像下面这样的块：

.. code-block:: rst

    - item 1
    - item 2

就是所谓 ``子弹列表(bullet list)``, 渲染的时候就会将其渲染成无序列表的样子。

另外一类叫做内联标记，就是指文字中的一部分使用特定的方式解读，这部分字符就被称作内联标记，比如 ``**bold**``,
单词前后加两个星号，就表示文字加粗，整个 ``**bold**`` 就是所谓的内联标记。

除此之外，还有一些语法规则，比如空格使用，转义等等，这些属于规则，没有实体。

对reStructuredText的语法进行分类以后，再对照官方文档，学起来就会感觉轻松很多。

文档元素(Body Elements)
-------------------------

.. attention::

    我对官方文档的分类进行了一些调整，章节、分隔符我将其作为文档元素的一个子类，个人觉得这样概念上更清晰一些。

常见的无序有序列表，段落什么的就不列举了，很好理解。除了这些，reStructuredText还有以下几种列表或者块：

章节(Sections)
~~~~~~~~~~~~~~

reStructuredText中的章节可以对应markdown里的标题，它的语法很简单，但是功能作用比看上去要大的多。

定义列表(Explicit Markup Blocks)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

这个定义不是动词是名称。比如下面的例子，``term`` 是一个名词，指对术语的解释：

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

引用块(Block Quotes)
~~~~~~~~~~~~~~~~~~~~~

引用块顾名思义，就是引用别人的话，格式和定义列表很像，区别就是第一行和第二行之间有空行：

.. code-block:: rst

    This is an ordinary paragraph, introducing a block quote.

        "It is my business to know things.  That is my trade."

        -- Sherlock Holmes

渲染结果是：

This is an ordinary paragraph, introducing a block quote.

    "It is my business to know things.  That is my trade."

    -- Sherlock Holmes

文字块(Literal Blocks)
~~~~~~~~~~~~~~~~~~~~~~~

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

文字块后面是两个冒号，双冒号很智能，它的规则是:

1. 如果两个冒号单独一行，则渲染以后这一行不会显示。
2. 如果两个冒号前有空格，则最后渲染出来的结果不包含这两个冒号。
3. 如果两个冒号前是非空格，则最后渲染的结果只包含一个冒号。

字段列表(Field Lists)
~~~~~~~~~~~~~~~~~~~~~~

源码：

.. code-block:: rst

    :what: Field lists map field names to field bodies, like
       database records.  They are often part of an extension
       syntax.

    :how: The field marker is a colon, the field name, and a
          colon.

          The field body may contain one or more body elements,
          indented relative to the field marker.

渲染以后：

:what: Field lists map field names to field bodies, like
       database records.  They are often part of an extension
       syntax.

:how: The field marker is a colon, the field name, and a
      colon.

      The field body may contain one or more body elements,
      indented relative to the field marker.

选项列表(Field Lists)
~~~~~~~~~~~~~~~~~~~~~~

源码：

.. code-block:: rst

    -a            command-line option "a"
    -b file       options can have arguments
                  and long descriptions
    --long        options can be long also
    --input=file  long options can also have
                  arguments
    /V            DOS/VMS-style options too

渲染以后：

-a            command-line option "a"
-b file       options can have arguments
              and long descriptions
--long        options can be long also
--input=file  long options can also have
              arguments
/V            DOS/VMS-style options too

.. attention::

    选项和描述之间最少2个空格

显式标记块(Explicit Markup Blocks)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

显式标记块是reStructuredText不太好理解的一个概念。简单来说，任何最前面是 ``..`` 开头的块都是显式标记块，表示整个块需要用特殊方式进行解读。

脚注(Footnotes)
************************

脚注可以认为是 `交叉引用`_ 的一种，语法如下：

.. code-block:: rst

    .. [1] A footnote contains body elements, consistently
    indented by at least 3 spaces.

渲染以后：

.. [1] A footnote contains body elements, consistently
    indented by at least 3 spaces.

引用到脚注很简单, 写法是 ``脚注1 [1]_``，渲染以后就是脚注1 [1]_ 。

脚注可以自动编号，使用 ``#`` 开头即可：

.. code-block:: rst

    .. [#] A footnote contains body elements, consistently
        indented by at least 3 spaces.

渲染以后：

.. [#] A footnote contains body elements, consistently
    indented by at least 3 spaces.

脚注引用的写法为 ``脚注 [#]_``, 渲染以后就是 脚注 [#]_ 。``#`` 还可以跟文字说明。

.. code-block:: rst

    .. [#foot] A footnote contains body elements, consistently
        indented by at least 3 spaces.

文字只起个说明的作用，渲染出来还是数字编号：

.. [#foot] A footnote contains body elements, consistently
    indented by at least 3 spaces.

另外，使用 ``*`` 号可以自动生成不同的符号，如下：

.. code-block:: rst

    .. [*] This is the star one footnote.
    .. [*] This is the star two footnote.

渲染以后：

.. [*] This is the star one footnote.
.. [*] This is the star two footnote.

引用的时候统统使用 ``*`` 引用就可以了，比如：

    - 源码：``星星脚注1 [*]_``，渲染结果：星星脚注1 [*]_
    - 源码：``星星脚注2 [*]_``，渲染结果：星星脚注2 [*]_

不过注意，``*`` 号后面不能跟文字说明。另外，几种脚注可以混用，但是最好选用一种，免得混淆。

引用(Citations)
************************

引用和脚注很像，只不过使用文字而不是数字，比如：

.. code-block:: rst

    .. [CIT2002] This is the citation.  It's just like a footnote,
        except the label is textual.

渲染以后：

.. [CIT2002] This is the citation.  It's just like a footnote,
   except the label is textual.

引用的写法：``[CIT2002]_``，渲染结果：[CIT2002]_

超链接目标(Hyperlink Targets)
******************************

具体使用方法参考 `显式target`_

替换定义(Substitution Definitions)
***********************************

顾名思义，如果有一个对象（文本，图像等）在多个位置被引用，就可以用替换进行简化。比如：

.. code-block:: rst

    |dog|

    .. |dog| image:: dog.jpg

渲染以后：

    |dog|

    .. |dog| image:: dog.jpg

可见，图片替换了 ``|dog|``，其中 ``image::`` 是一个指令，关于指令请查看 `相应章节 <指令(Directives)>`_ 。

sphinx内置了三个替换定义，分别是 ``|release|``, ``|version|``, ``|today|``,它会根据sphinx的配置文件自动进行替换。

另外，在测试过程中，发现部分docutils的例子使用sphinx编译时报错，原因未知，留待以后补充。

指令(Directives)
******************

- `docutils指令文档 <https://docutils.sourceforge.io/docs/ref/rst/directives.html>`_
- `sphinx指令文档 <https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html>`_

指令是reStructuredText最强大的功能之一，也是最不好理解和掌握的特性。指令可以理解成通用的显式标记块，也就是说，上面所有的显式标记块，
什么注释啊，脚注啊，都是一种特殊的指令而已。

我们先看一个完整的指令是什么样子：

.. code-block:: rst

    .. function:: foo(x)
                  foo(y, z)
       :module: some.module.name

       Return a line of text input from the user.

上面这个指令，``function`` 被称为指令名称，``foo(x)`` 和 ``foo(y, z)`` 可认为是指令的参数，``:module`` 被称为指令的选项。
最后 ``Return ...`` 部分是指令的内容。

不同的指令，有完全不同的解读方式。比如上面这个指令，渲染出来是下面这个样子：

.. function:: foo(x)
              foo(y, z)
   :module: some.module.name

   Return a line of text input from the user.

reStructuredText原生支持的指令很多，sphinx对原生的reStructuredText又进行了扩展，添加了不少指令。可以点击上面的链接进行查看。

这里不对指令展开，免得分散注意力。常用的指令可以查看专题内容的 `常用指令`_ 部分。

注释(Comments)
******************

段落前面加两个冒号及空格，这个段落就成了注释，注释是说明性的文字，不会进行渲染：

.. code-block:: rst

    .. This is a comment

问题是，指令前面也是两个冒号开头，比如注释内容为 ``[comment] this is a comment!``, 此时sphinx会将其识别为前面提到过的
引用，解决方法很简单，两个点一行，注释内容单独一行就可以了，如下:

.. code-block:: rst

    ..
      [commnet] this is a comment!

另外，单独两个冒号被称为空注释。空注释用于一个比较微妙的场景，比如下面的定义列表：

.. code-block:: rst

    This is
        a definition list.

        This is a block quote.

``This is a block quote`` 本意是前面有缩进的新的段落。但是上面的写法，渲染出来的结果，却成了定义列表的一部分：

This is
    a definition list.

    This is a block quote.

此时，在定义列表后可以添加一个空注释，表示定义列表的终结：

.. code-block:: rst

    This is
        a definition list.

    ..

        This is a block quote.

此时，``This is a block quote`` 就不再是定义列表的一部分，而是一个单独的引用块（前面有缩进的新段落）：

This is
    a definition list.

..

    This is a block quote.

内联标记(Inline Markup)
------------------------

在一段文本中，可能部分文字需要进行特殊的解读。这部分需要特殊解读的文字就被成为内联标记。比如字体加粗，``**bold**`` 就是一种内联标记。

一共有9种内联标记，又可以分为两类，一类是起始字符和结束字符相同的，一共5种：

- 斜体： ``*``
- 粗体： ``**``
- 解释性文本： `````, 反引号，解释性文本和角色有关，具体查看 `角色(Role)`_ 章节。
- 内联纯文本： ``````
- 替换引用： ``|``

另外三种起始字符和结束字符不一样：

- 内联的内部target： ``_``` 开头， ````` 结尾，内部定义一个target，参考 `交叉引用`_ 章节
- 脚注引用： ``[`` 开头， ``]_`` 结尾，参考 `脚注(Footnotes)`_
- 超链接引用： ````` 开头， ```_`` 结尾，参考 `交叉引用`_ 章节

最后一种，是普通的超链接，比如一个url，这种无需额外的起始和结束字符。

识别规则
~~~~~~~~~~~~~~~

内联标记的识别规则基本上符合直觉，也就是说一般情况下不会写错。但是有几点需要注意：

1. 内联标记的起始字符前，结束字符后需要是空格或者特定的ASCII字符。方便起见，都用空格吧。
2. 如果有字符要紧接着内联标记，需要使用 ``\`` 进行转义，比如：

    .. code-block:: rst

        Python ``list``\s use square bracket syntax.

    本意是list后面仅接一个s，list是内联标记，但如果按照第一条规则，内联标记后面要接空格，那么渲染出来是这样：

    Python ``list`` s use square bracket syntax.

    可见，list和s之间多了一个空格，要去掉这个空格，则可以像上面那样，紧接一个转义的 ``\`` 。渲染结果如下：

    Python ``list``\s use square bracket syntax.

角色(Role)
~~~~~~~~~~~~~~~

- `docutils角色文档 <https://docutils.sourceforge.io/docs/ref/rst/roles.html>`_
- `sphinx角色文档 <https://www.sphinx-doc.org/en/master/usage/restructuredtext/roles.html>`_

上面提到了前后用反引号包括起来的文本叫可解释文本，所谓可解释文本，意思是需要用特定的方式去解读。那么，具体咋个解读呢？
这就需要在前面或者后面添加一个角色标记。不同的角色，意味着不同的解读方式。比如：``:strong:`bold```，渲染出来就是 :strong:`bold` 。

可见，和指令类似，角色是个通用的玩意，上面提到的各种内联标记，基本上都有与之对应的角色。

可能有时候你会看到 ```text``` 这样的写法，文本前后只有反引号，没有设置角色。此时，其实有一个默认的角色在起作用。
sphinx官方文档说，默认角色不会对文本进行任何额外的处理，不过我使用的sphinx_rtd_theme主题，渲染以后都成了斜体，不
知道是主题的原因还是默认就是这样。

可以在conf配置文件中自定义默认的角色，参考配置 :ref:`tutorials/sphinx:default_role` 。

-----------------------------------

重点专题
++++++++++++++++++++++

交叉引用
----------------------

所谓交叉引用(cross reference)就是一个链接。reStructuredText不仅可以链接一个URL，还可以链接到任意文档的任意位置，
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

我们还可以把引用(reference)和目标地址(target)分开。比如，在文档中这样写：

``ref`` **角色**

``doc`` **角色**

除了可以链接到章节，还可以使用 ``doc`` 角色链接到文件，比如：

.. code-block:: rst

    链接到根目录的index.rst文件 :doc:`/index`

渲染结果为：

链接到根目录的index.rst文件 :doc:`/index`

可见，渲染出来的文字内容是目标文件的第一个标题。当然也可以用前面的方法，自定义渲染内容。

常用指令
----------------------

这里我们不详细解读语法细节，主要讨论指令用于什么场景，但是每个指令会给出官方文档的链接。

toctree
~~~~~~~~~~~~~~~~~~~~

- `官方文档 <https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-toctree>`_

toctree是一个sphinx的扩展指令，可以说它是sphinx最重要的一个指令。它的作用是把指令内容中列举的所有文档
的章节标题抽取出来形成一个目录，并且这个目录会同时呈现在文档的主页面和侧边栏。

常用角色
----------------------

code
~~~~~~~~~~~~~~

正常情况下，``code`` 角色渲染出来的结果和内联纯文本是一样的。但是 ``code`` 可以搭配 ``role`` 指令使用。比如，我希望内联高亮python
语句，则先定义一个 ``role``:

.. code-block:: rst

    .. role:: py(code)
       :language: python

接下来就可以使用 ``py`` 这个角色了, 比如： ``:py:`lambda x: x * x``` ， 渲染出来就是 :py:`lambda x: x * x` 。注意，指令定义
必须在使用角色之前，否则会报错。

如果不想每个rst文件这么定义一遍，而是定义一个全局的角色，可以配置conf文件的 :ref:`rst_prolog <tutorials/sphinx:rst_prolog>` 条目。

