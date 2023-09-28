reStructuredText私房手册
==========================

`标记规范`_ 部分主要是基于docutils官方文档 `reStructuredText Markup Specification <https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html>`_
，对一些不好理解的例子加以详细说明。

`重点专题`_ 是在标记规范里不好展开，或者某一个特定主题的内容。其中 `交叉引用`_ 是reStructuredText非常常用，且最强大的功能之一，
但不是很好理解。因此单独成章放在重点专题里，这一章主要基于readthedocs的教程
`cross referencing with sphinx <https://docs.readthedocs.io/en/stable/guides/cross-referencing-with-sphinx.html#explicit-targets>`_
并且添加了自己的理解。

-----------------------------------

标记规范
+++++++++++++++++++++++++

刚学习reStructuredText的时候感觉rst的语法复杂又怪异，远不如markdown简单，简单看了几遍，总是记不住。

等熟悉以后发现，其实细分下来，主要就两大类，一类是文档元素，或者说块，所谓块，可以理解成一段内容，按照不同的语法书写的块有不同的解读方式。比如：

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

对rst的语法进行分类以后，梳理清楚脉络，再对照官方文档，学起来就轻松多了。

文档元素(Body Elements)
-------------------------

.. attention::

    我对官方文档的分类进行了一些调整，章节、分隔符我将其作为文档元素的一个子类，个人觉得这样概念上更清晰一些。

常见的无序有序列表，段落什么的就不列举了，很好理解。除了这些，reStructuredText还有以下几种列表或者块：

章节(Sections)
~~~~~~~~~~~~~~~~

reStructuredText中的章节可以对应markdown里的标题，它的语法很简单，但是功能作用比看上去要大的多。

文字下面使用 ``=``, ``-``, ``~``, ``*``, ...等作为下划线的就是章节，相同的下划线对应相同的层级。从上到下
读取，先读取到的层级高。

另外，章节的标题天生就是 `隐式target`_ ，也就是说可以作为 `交叉引用`_ 的目标位置。

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

显式标记块是rst不好理解的一个概念。简单来说，任何最前面是 ``..`` 开头的块都是显式标记块，表示整个块需要用特殊方式进行解读。

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

参考专题 `显式target`_

替换定义(Substitution Definitions)
***********************************

顾名思义，如果有一个对象（文本，图像等）在多个位置被引用，就可以用替换进行简化。比如：

.. code-block:: rst

    |dog|

    .. |dog| image:: ../imgs/dog.jpg

渲染以后：

    |dog|

    .. |dog| image:: ../imgs/dog.jpg

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

而最常用的指令 :rst:`.. code-block::` ，用来显示一段代码：

.. code-block:: rst

    .. code-block:: python

        def add(x, y):
            return x + y

渲染出来是这个样子：

.. code-block:: python

    def add(x, y):
        return x + y

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

分割线(Transitions)
~~~~~~~~~~~~~~~~~~~~~~~~

reStructuredText的分隔线很简单，``--------`` 前后加空白行即可。

内联标记(Inline Markup)
------------------------

在一段文本中，可能部分文字需要进行特殊的解读。这部分需要特殊解读的文字就被成为内联标记。比如字体加粗，``**bold**`` 就是一种内联标记。

一共有9种内联标记，又可以分为两类，一类是起始字符和结束字符相同的，一共5种：

- 斜体： ``*``
- 粗体： ``**``
- 解释性文本： `````, 反引号，解释性文本和角色息息相关，具体查看 `角色(Role)和域(Domain)`_ 章节。
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

1. 内联标记的起始符号前，结束符号后需要是空格或者特定的ASCII字符。方便起见，都用空格吧。
2. 如果有字符要紧接着内联标记，需要使用 ``\`` 进行转义，比如：

    .. code-block:: rst

        Python ``list``\s use square bracket syntax.

    本意是list后面仅接一个s，list是内联标记，但如果按照第一条规则，内联标记后面要接空格，那么渲染出来是这样：

    Python ``list`` s use square bracket syntax.

    可见，list和s之间多了一个空格，要去掉这个空格，则可以像上面那样，紧接一个转义的 ``\`` 。渲染结果如下：

    Python ``list``\s use square bracket syntax.

角色(Role)和域(Domain)
~~~~~~~~~~~~~~~~~~~~~~~~

- `docutils角色文档 <https://docutils.sourceforge.io/docs/ref/rst/roles.html>`_
- `sphinx角色文档 <https://www.sphinx-doc.org/en/master/usage/restructuredtext/roles.html>`_
- `sphinx Domains <https://www.sphinx-doc.org/en/master/usage/restructuredtext/domains.html>`_

上面提到了前后用反引号包括起来的文本叫可解释文本，所谓可解释文本，意思是需要用特定的方式去解读。那么，具体咋个解读呢？
这就需要在前面或者后面添加一个角色标记。不同的角色，意味着不同的解读方式。比如：``:strong:`bold```，渲染出来就是 :strong:`bold` 。

可见，和指令类似，角色是个通用的玩意，上面提到的各种内联标记，基本上都有与之对应的角色。

可能有时候你会看到 ```text``` 这样的写法，文本前后只有反引号，没有设置角色。此时，其实有一个默认的角色在起作用。
sphinx官方文档说，默认角色不会对文本进行任何额外的处理，不过我使用的sphinx_rtd_theme主题，渲染以后都成了斜体，不
知道是主题的原因还是默认就是这样。

可以在conf配置文件中通过 ``default_role`` 自定义默认的角色，参考配置 :ref:`tutorials/sphinx:default_role` 。

说到角色，还有个域(Domain)概念也需要了解。域名是sphinx的扩展，原生的reStructuredText是没有的。它是在角色和
指令前面再加一个标记，用以区分不同的语言。

比如说，下面这个python函数：

.. code-block:: rst

    .. py:function:: pyfunc(x)

       :param int x: an int
       :returns: double x

渲染出来是这样：

.. py:function:: pyfunc(x)

   :param int x: an int
   :returns: double x

:rst:`:py:` 就是指令的域，表示这是一个python函数（可以省略，默认就是python）。但如果现在是一个javascript的函数呢？
此时，可以在之前前面加上 :rst:`:js:` 域，表示使用的语言是javascript，比如：

.. code-block:: rst

    .. js:function:: jsfunc(x)

       :param int x: an int
       :returns: double x

.. js:function:: jsfunc(x)

   :param int x: an int
   :returns: double x

不光指令有域，角色也有域，基本上，和域相关的指令和角色是成对的。比如，:rst:`:py:meth:\`os.getcwd\``, 域+角色就链接
到了python的官方文档，渲染以后结果是： :py:func:`os.getcwd` 。

.. attention::

    指令域前面不用加冒号，角色域前面要加冒号

.. note::

    如何链接到其它项目的文档，参考 :ref:`tutorials/sphinx:sphinx.ext.intersphinx`

为了不分散注意力，这里仅介绍概念，具体角色请参考 `常用角色`_ 章节。

-----------------------------------

重点专题
++++++++++++++++++++++

交叉引用
----------------------

所谓交叉引用，就是链接。首先学习一个术语，在sphinx中，所有交叉引用都是由reference和target构成的，两者是成对的，我们点击reference，
就会跳转到target定义的位置。sphinx中，有两种不同的reference，一种是反引号后面加一个下划线。

.. code-block:: rst

    Python website is: `http://www.python.org/`_

另外一种是使用 ``:ref:`` 角色。 比如： :rst:`:ref:\`api\`` 这样的一个写法，其中，反引号包含的文本被称为可解释文本，
表示要用特殊的方法去解读，前面的 ``:ref:`` 是解读的方法，表示后面反引号包含的内容是一个引用。这个 ``:ref:`` 就被称为角色。

target分为三种，分别是：外部url，显式target，隐式target。

sphinx的交叉引用非常强大，只要定义好了reference和target，不仅可以在同一个文档中，还可以在不同文档中跳转。
甚至利用intersphinx插件，还可以跳转到其它开源项目文档中的target。要注意的是，反引号的方式，只能跳转到同一个文档的target，
如果要跳转到其它文档的target，只能使用ref角色。

接下来，我们基于不同的target，学习如何使用sphinx的交叉引用。

.. _outside url:

外部URL
~~~~~~~~~~~~~~~~~~~~

外部url很简单，直接写url就好了：

.. code-block:: rst

    Python website is: http://www.python.org/

渲染出来结果是： Python website is: http://www.python.org/

如果你想显示文本内容而不是直接的链接，就好像markdown的 ``[python](http://www.python.org)`` ，你可以这样写：

.. code-block:: rst

    Python website is: `python <http://www.python.org/>`_

渲染出来是这样：Python website is: `python <http://www.python.org/>`_ ， ````` 是反引号， 两个反引号囊括起来的文本，
在reStructuredText中，被称为解释性文本。表示整个文本，需要使用特定的方式去解读，后面加一个 ``_``，则表示这个文本是一个链接。

显式target
~~~~~~~~~~~~~~~~~~~~

我们还可以把引用(reference)和目标地址(target)分开。比如，在文档中这样写：

.. code-block:: rst

    Python website is: `python`_

如果是单个单词，可以直接写成：

.. code-block:: rst

    Python website is: python_

这里的 ``python_`` 被称为target标签（reference），它是指向文档其它部分的指针。然后，我们可以在文档的其它任何地方，添加一个
target标签，写法是：

.. code-block:: rst

    .. _python: http://www.python.org/

如果我们不是跳转到外部url，而是在文档内部或者文档之间跳转。则冒号后面可以不加任何内容，:rst:`.. _python:` 。此时表示一个
target标签。

注意，如果冒号后面没有内容，则target标签后面不能接普通的段落，必须接章节，定义段落，脚注，代码块，定义列表这些可以用来定位的元素，
就像这样：

.. code-block:: rst

    .. _outside url:

    外部URL
    ----------------

否则编译的时候会抛出错误。此时，点击引用标签，会跳转到后面的target标签。

``ref`` **角色**

我们经常会在别人的文档里看到这种写法：:rst:`:ref:\`python\`` ，在解释性文本之前或者之后，有类似 ``:ref:`` 的标记，这个标记
被称为解释性文本的角色（role），相当于指明用什么方式去解读反引号里面的文本。 ``:ref:`` 表示，后面的文本是一个引用。

``python`` 后面加一个 ``_`` 表示这是一个引用，:rst:`:ref:\`python\`` 是在解释性文本之前加 ``:ref:``
角色也表示是一个引用，这两者之前有什么区别呢？

1. 两者渲染的内容不一样。

    比如：现在在 ``外部URL`` 这个章节标题前定义一个target标签， :rst:`.. _outside url:` ，现在使用
    :rst:`:ref:\`outside url\`` 进行引用，渲染结果为： :ref:`outside url` ，渲染的结果是章节标题。而
    :rst:`\`outside url\`_` ，渲染的结果是 `outside url`_ 。

2. 两者可以引用的范围不一样

    后面加 ``_`` 的方式只能引用同一个文档内部的target标签，而 ``:ref:`` 角色不但可以引用文档内部标签，
    还可以引用其它任意文档的标签。

另外，sphinx的 :ref:`tutorials/sphinx:sphinx.ext.autosectionlabel` 插件，可以自动为所有的章节添加显式的
target标签。

``doc`` **角色**

除了可以链接到章节，还可以使用 ``doc`` 角色链接到文件，比如：

.. code-block:: rst

    链接到根目录的index.rst文件 :doc:`/index`

渲染结果为：

链接到根目录的index.rst文件 :doc:`/index`

可见，渲染出来的文字内容是目标文件的第一个标题。当然也可以用前面的方法，自定义渲染内容。

隐式target
~~~~~~~~~~~~~~~~~~~~~

所有的章节标题，脚注，引用(citations)都是隐式的target标签，也就是说，不需要用 :rst:`.. _target:` 这样的语法标记
就可以直接引用。但是注意：

    1. 隐式target只能 :rst:`\`target\`_` 这种方式可以引用。 ``:ref:`` 角色的方式不能引用的，必须加显式的target标签。
       也就是说，隐式target只能在文档内部被引用，不能直接引用其它文档的隐式target。
    2. 相同的隐式target名称，以及相同的隐式和显式target的名称会产生冲突。冲突处理规则有点麻烦，也不需要特意的去记，如果
       有冲突，会有提示。所以记住不要有相同的target名称就好了。

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

正常情况下，``code`` 角色渲染出来的结果和内联纯文本是一样的。``code`` 一般是搭配 ``role`` 指令使用。比如，我希望内联高亮python
语句，则先定义一个 ``role``:

.. code-block:: rst

    .. role:: py(code)
       :language: python

接下来就可以使用 ``py`` 这个角色了, 比如： ``:py:`lambda x: x * x``` ， 渲染出来就是 :py:`lambda x: x * x` 。注意，指令定义
必须在使用角色之前，否则会报错。

如果不想每个rst文件这么定义一遍，而是定义一个全局的角色，可以配置conf文件的 :ref:`rst_prolog <tutorials/sphinx:rst_prolog>` 选项。

any
~~~~~~~~~~~~~

``any`` 是sphinx扩展角色，它相当于是一个交叉引用的搜索引擎，会自动搜索所有的target。设置了 ``any``, 相当于可以省略 ``:term:``,
``py:mod:``, ``:ref:``, ``:doc:`` 等等角色前缀。

``any`` 一般被当作默认的角色，设置默认角色参考 :ref:`rst_prolog <tutorials/sphinx:rst_prolog>` 配置。

比如，当前项目已经设置了使用 ``any`` 为默认角色，则要链接到python官方文档 *os* 包的 *getcwd* 函数，则可以直接写成 :rst:`\`os.getcwd\``,
不需要加前缀写成 :rst:`:py:func:\`os.getcwd\`` ，渲染以后就是 `os.getcwd` 。

