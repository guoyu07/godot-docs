.. _doc_instancing:

场景实例化(Instancing)
==========

原理阐述
---------

创建一个场景并将节点扔到里面对于小项目是适用的，但随着项目不断发展，用到越来越多的节点，整个项目很快就会演化成难以管理的状态。
为了解决这个问题，Godot运行一个项目分割成多个场景。这一点与其它游戏引擎的方式实际上有很大的不同，所以不要跳过这节的内容。

要点: 场景是树状组织的节点集合，有且仅有一个根节点。

.. image:: /img/tree.png

Godot中可以创建一个场景并将其保存到硬盘中，同时，可以创建多个场景并按需单纯存储。

.. image:: /img/instancingpre.png

然后，编辑已有场景或新场景时，其它场景可以被实例化为它的一部分：

.. image:: /img/instancing.png

上图中，场景B的实例被添加到场景。现在这个效果看起来有点怪，但本文的最后会构建一个完整的场景。

场景实例化的操作步骤
------------------------

为了学习如何进行场景的实例化，先下载一个演示项目： :download:`instancing.zip </files/instancing.zip>`.

解压该场景文件到任意目录，然后用'Import'按钮添加该场景到项目管理器中：

.. image:: /img/importproject.png

Simply browse to inside the project location and open the "engine.cfg"
file. The new project will appear on the list of projects. Edit the
project by using the 'Edit' option.

该项目包含两个场景： "ball.scn" 及 "container.scn"。“ball”场景中仅包含一个球体，“container”场景中有一个具象的碰撞检测体，球体可以被扔进去.

.. image:: /img/ballscene.png

.. image:: /img/contscene.png

打开“container”场景，然后选中根节点：

.. image:: /img/controot.png

再“推开”链接形状的按钮，这个就是场景实例化按钮！

.. image:: /img/continst.png

选择“ball”场景(ball.scn), 球体现在应该出现在坐标原点(0,0)，像下图这样将它移到场景中间区域：

.. image:: /img/continstanced.png

点“Play”看看!

.. image:: /img/playinst.png

被实例化的球体掉到了坑底。

补充
-------------

一个场景可以按照需要被实例化多个，你可以试试，或者直接复制（按ctrl-D或点复制按钮）：

.. image:: /img/instmany.png

然后再试试运行一次这个场景：

.. image:: /img/instmanyrun.png

这就是为什么叫场景被实例化更准确的原因！

编辑实例
-----------------

选中那些球体的其中一个拷贝，到属性编辑器那里我们可以让它弹跳更多次，找到“bounce”参数，将其设为“1.0”：

.. image:: /img/instedit.png

下一次这里将会出现一个"revert"按钮，意味着相对于原始场景该实例化场景的这个属性值发生了变化。即便原始场景中该值发生了改变，还是会按实例化场景的自定义值为准。如果要恢复该属性值为原始场景的值，点"revert"按钮就好了。

结论
----------

实例化看似简单，但它的内涵远不止你现在看到的这些，下一节内容会覆盖剩余的知识点..
