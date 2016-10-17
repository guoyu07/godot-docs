.. _doc_scripting:

脚本编写(Scripting)
=========

简介
------------

关于无需编程即可创建视频游戏的那些工具的谈论有很多。对很多独立开发者来说，不用学习编程知识就是一个梦想。这种需求 - 游戏开发者、甚至在很多公司内部，希望对游戏流程拥有更多控制权，已经有很长一段时间了。

很多产品号称是无需编程的环境，但相对于传统的编码开发流程，这些产品的最终使用结果，经常是做不出完整的作品、或者是能做出也太复杂或太低效。编程的过程耗费太多时间。实际上，游戏引擎的通常趋势是在这种必须要通过编码来实现特定任务的地方，添加一些工具，来尽量减少编码量、提升开发速度。

基于这种理解，Godot针对这个目标采用了一些有效的设计决策。第一点也是最重要的一点就是场景系统。起初它的目标并不明显，但之后很好的发挥了作用：
减轻了程序员去设计架构代码的职责。

在使用场景系统设计游戏时，整个项目被划分成*互补的*场景 - 不是独立的。场景之间互补，而不是相互独立。后续会有大量示例，但请先记住这点。

对于已有了专业编程知识的人来说，这是一种与MVC不同的设计模式。Godot承诺：你丢弃MVC编程习惯并换用*场景即complement*模式后，还能拥有高效。

Godot也使用 `继承 <http://c2.com/cgi/wiki?EmbedVsExtend>`__
模式进行脚本编程，意味着脚本可以继承所有可用的引擎类。

GDScript
--------

:ref:`doc_gdscript` 是Godot环境内运行的一种动态类型脚本语言。它的设计目标为：

-  简单、熟悉、尽可能易于学习
-  代码可读性高、错误安全处理；语言多数借鉴自Python

开发者通常只需花几天学习它，两周内就能完全适应。

和大多数动态类型语言一样，更高的生产效率 - 易于学习、编码很快、无需编译等，意味着对应的是性能的降低，但引擎的大多数关键代码是由C++编写（矢量运算、物理学、数学、索引等），这使得游戏最后的性能比多数类型的游戏要好。

而且如果真有更高的性能要求，关键部分的代码可以用C++重写并暴露给脚本调用。这时，你只需要将GDScript类替换成C++类即可，不需要修改游戏其它地方。

编写场景脚本
-----------------

在此之前，请确保你已经读过 :ref:`doc_gdscript` 参考文档了。它是一种简单的语言，并且参考文档也很短，大致过一遍应该花不了几分钟。

场景设置
~~~~~~~~~~~

本文将开始编写一个简单的GUI场景。用“add
node”对话框创建下述的节点和层级：

- Panel

  * Label
  * Button

在场景数，看起来效果是这样：

.. image:: /img/scripting_scene_tree.png

在2D编辑器中要像这样：

.. image:: /img/label_button_example.png

最后保存场景，取名"sayhello.scn"

.. _doc_scripting-adding_a_script:

添加脚本
~~~~~~~~~~~~~~~

右键点击Panel节点，然后在右键菜单中选择"Add Script"：

.. image:: /img/add_script.png

弹出“script creation”对话框。对话框中可以选择语言、类名等。脚本文件中，GDScript是不用类名的，所以这个字段是不可编辑的。这个脚本相应的继承自“Panel”，即继承该Panel类型的节点（代码其实会自动填充的）。

填写脚本的路径名然后选择“Create”：

.. image:: /img/script_create.png

完成操作后，脚本被创建并添加到该节点下。在该节点下可以看到一个额外的图标及“script”属性：

.. image:: /img/script_added.png

打开脚本编辑器后，你就会发现已经自动填充了一段前面所述的模板代码：

.. image:: /img/script_template.png

当该节点（包括它所有的children）进入当前活跃状态的场景时，"_ready()"函数会被执行。记住哦：这个不是构造函数，构造函数是“_init()”。

脚本的角色
~~~~~~~~~~~~~~~~~~~~~~

脚本给节点添加了行为，它用于控制该节点以及其它节点(children, parent, siblings, 等)的功能。脚本的局部作用域仅是该节点；该节点的那些虚拟函数被该脚本捕捉。

.. image:: /img/brainslug.jpg

处理信号
~~~~~~~~~~~~~~~~~

在大多数GUI节点中都用到了信号(Signal)，其实其它节点也有。当一些特定类型的动作发生时，信号就会被“发射”出来，可以联系到任意脚本实例的任意函数。
这一步中，按钮的"pressed"信号会被联系到一个自定义函数。

编辑器中有连接信号到脚本的界面：选中场景树中的节点，然后选择“节点”选项卡，再选中其中的"Signals"选项卡。

.. image:: /img/signals.png

此时我们主要对"pressed"信号感兴趣。除了可用可视化界面来完成信号连接操作，也可以通过脚本代码来进行。

针对这种情况，Godot的程序员可能用得最多的一个函数：
:ref:`Node.get_node() <class_Node_get_node>`.
该函数使用路径来获取该场景中当前树状结构或任意地方的节点。

要获取到该按钮，需要用下述写法：

::

    get_node("Button")

下一步，当按钮被按下时，会添加一个回调 - 改变标签的文本内容：

::

    func _on_button_pressed():  
        get_node("Label").set_text("HELLO!")

最后，该按钮的"pressed"信号需要在_ready()函数中进行连接， :ref:`Object.connect() <class_Object_connect>`.

::

    func _ready():
        get_node("Button").connect("pressed",self,"_on_button_pressed")

脚本的最终内容大致如下：

::

    extends Panel

    # member variables here, example:

    # var a=2
    # var b="textvar"

    func _on_button_pressed():
        get_node("Label").set_text("HELLO!")

    func _ready():
        get_node("Button").connect("pressed",self,"_on_button_pressed")

运行场景，按下按钮后应该就能得到预期的结果：

.. image:: /img/scripting_hello.png

**注意:** get_node(path)仅会在节点的最直接的下一节范围内查找，即*Button*必须是*Panel*子一级节点，否则是访问不到的。假如*Button*是*Label*的子一级节点，代码就需要修改了：

::

    # not for this case
    # but just in case
    get_node("Label/Button") 

而且要记住，这里引用的是节点的名称而不是节点的类型名。
