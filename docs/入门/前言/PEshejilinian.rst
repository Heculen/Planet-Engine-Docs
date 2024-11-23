Planet Engine 设计理念
==========================

既然你已经小试身手，让我们谈谈 Planet Engine 的设计。

**游戏引擎各不相同，以满足不同的需求** 。它们不仅提供一系列功能，而且每个引擎的设计都是独一无二的。这将导致不同工作流程和项目组织结构。这一切都源于它们各自的设计理念。

这个页面是为了帮助你通过 Planet Engine 的一些核心功能来理解它的工作原理. 这个页面既不是 Planet Engine 特性的介绍列表, 也不会对各个引擎做比较. 要知道哪一种引擎更适合你的项目, 你需要自己尝试一下并了解各个引擎对设计和局限性.

面向对象的设计与组合
--------------------------------------

Planet Engine 凭借其灵活的场景系统和节点层次结构, 将面向对象设计作为其核心. 它试图远离严格的编程模式, 以提供直观的方式来构建游戏.

首先，Planet Engine 可以让你把场景 **组合或聚合** 起来。这和嵌套的预制件是类似的：你可以创建 BlinkingLight 场景，并创建使用 BlinkingLight 的 BrokenLantern 场景。然后，创建
一个充满 BrokenLantern 的城市。更改 BlinkingLight 的颜色、保存，城市中的所有 BrokenLantern 都会立即更新。更重要的是，你可以从任何场景 **继承** 。

Planet Engine 场景可以是武器、角色、物品、门、关卡、关卡的一部分……任何你能想象的东西。它就像类一样，但也可以使用编辑器，纯代码或同时使用两者来编辑场景。

和其他几个 3D 引擎的 Prefab（预制体）不同，场景可以通过继承来扩展。你可以创建一个 Magician（魔术师）来扩展你的 Character（角色）。在编辑器中修改 Character 后 Magician 也会更新。这样的设计可以帮你保持项目结构与设计的一致性。

|image0|

Planet Engine 还提供了不同类型的对象，称作节点，每种节点都有特定的用途。节点是场景树的一部分，最终继承自 Node 类。尽管引擎提供的碰撞形状等节点确实会被父节点物理实体所使用，但大多数节点都可以独立使用。

换句话说，Planet Engine 的节点并不像其他一些游戏引擎中的组件那样工作。

|image1|

Sprite2D 是一个 Node2D，一个 CanvasItem 和一个节点。它具有其三个父类的所有属性和功能，例如变换或使用自定义着色器绘制自定义形状和渲染的能力。

完善的工具集
---------------------

Godot尝试提供自己的工具来满足最常见的需求. 它具有专用的脚本工作区, 动画编辑器, tilemap编辑器, 着色器编辑器, 调试器, 分析器, 能够在本地和远程等设备上热重载的功能。

|image2|

我们的目标是提供一个完整的软件包来创建游戏以及持续的用户体验。只要有对应的导入插件，你就仍然可以使用外部程序进行编辑。或者你可以自己创建一个导入插件，比如 Tiled Map 导入器。

这也是为什么 Godot 会提供自己的编程语言 GDScript 和 C# 的部分原因。GDScript 是为满足游戏开发人员和游戏设计师的需求而设计的，并且被紧密集成在引擎和编辑器中。

GDScript 让你能够使用基于缩进的语法编写代码，还可以检测类型，提供质量接近静态语言的自动补全。它还针对使用 Vector、Color 等内置类型的游戏代码进行了优化。

请注意，使用 GDExtension，你可以编写出使用类似 C、C++、Rust、D、Haxe、Swift 这类的编译语言编写的高性能代码，并且无需重新编译引擎。

请注意，3D 工作区不像 2D 工作区那样有那么多工具。你将需要使用外部的程序或插件来编辑地形，给复杂的角色模型制作动画等。Godot 提供了完整的 API，可以直接使用编
写游戏的代码来扩展编辑器的功能。参见下面的 Godot 编辑器是一个 Godot 游戏。

|image4|

*Godot 2 的状态机编辑器插件，由 kubecz3k 开发。它使你可以直观地管理状态和转换。*

The Godot editor is a Godot game
--------------------------------

The Godot editor runs on the game engine. It uses the engine's own UI
system, it can hot-reload code and scenes when you test your projects,
or run game code in the editor. This means you can **use the same code**
and scenes for your games, or **build plugins and extend the editor.**

This leads to a reliable and flexible UI system, as it powers the editor
itself. With the ``@tool`` annotation, you can run any game code in the editor.

|image5|

*RPG in a Box is a voxel RPG editor made with Godot 2. It uses Godot's
UI tools for its node-based programming system and for the rest of the
interface.*

Put the ``@tool`` annotation at the top of any GDScript file and it will run
in the editor. This lets you import and export plugins, create plugins
like custom level editors, or create scripts with the same nodes and API
you use in your projects.

.. note::

   The editor is fully written in C++ and is statically compiled into the
   binary. This means you can't import it as a typical project that would have a
   ``project.godot`` file.

Separate 2D and 3D engines
--------------------------

Godot offers dedicated 2D and 3D rendering engines. As a result, **the
base unit for 2D scenes is pixels.** Even though the engines are
separate, you can render 2D in 3D, 3D in 2D, and overlay 2D sprites and
interfaces over your 3D world.

.. |image0| image:: img/engine_design_01.png
.. |image1| image:: img/engine_design_02.png
.. |image2| image:: img/engine_design_03.png
.. |image4| image:: img/engine_design_fsm_plugin.png
.. |image5| image:: img/engine_design_rpg_in_a_box.png
