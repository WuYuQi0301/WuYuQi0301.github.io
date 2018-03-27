---
title: Unity 3D HomeWork1
date: 2018-03-21 12:00:04
categories:
- Unity
---
## 简答题
- 解释游戏对象(GameObjects)和资源(Assets)的区别和联系
    - 区别：
        - 游戏对象(GameObject)是场景中的所有实体的基类，可以视为一个空的容器。当以GameObject为父类的继承中添加子类特有的属性之后，就构成了在游戏中看到的形态和行为各异的角色和场景了。每个游戏对象实例都在游戏对象森林（Hierarchy）中有自己的位置。
        - 资源(Assets)包括游戏数据和脚本代码等等。
    - 联系：资源可以提供给现有的游戏对象调用，也可以创建新的游戏对象实例，两者共同作用形成游戏。

- 下载几个游戏案例，分别总结资源、对象组织的结构（指资源的目录组织结构与游戏对象树的层次结构）

- 编写一个代码，使用debug语句来验证[MonoBehaviour](https://docs.unity3d.com/ScriptReference/MonoBehaviour.html)基本行为或者事件的触发条件
    - 基本行为包括 Awake() Start() Update() FixedUpdate() LateUpdate()
    - 常用事件包括 OnGUI() OnDisable() OnEnable()

    ```c#

    ```
- 查找脚本手册，了解GameObject，Transform，Conponent对象
    + 分别翻译官方对三个对象的描述(Description)
        * **GameObjects**: Base class for all entities in Unity scenes. 游戏对象是Unity场景中所有实例的基类。
        * **Transform**: Position, rotation and scale of an object. Every object in a scene has a Transform. It's used to store and manipulate the position, rotation and scale of the object. Every Transform can have a parent, which allows you to apply position, rotation and scale hierarchically. 对象的位置、旋转和大小。每一个对象都有一个Transform来储存和操作它的位置、旋转和大小。每个Transform对象都有一个父类使得开发者可以将位置、旋转和大小的信息继承地应用。
        * **Components**: Base class for everything attached to GameObjects. Note that your code will never directly create a Component. Instead, you write script code, and attach the script to a GameObject. 任何与游戏对象有关联的对象的基类。代码并不会直接创建一个组件，而是通过把写好的脚本代码关联到一个游戏对象上。
        
    + 描述下图中table对象（实体）的属性、table的Transform的属性、table的部件
        ![homework](https://pan.baidu.com/s/1myqv_r1PAmFZDPofJdTk5A)
    + 用UML图描述三者的关系
        ![UML](https://pan.baidu.com/s/1ZZfPtp6HLmQo8vU7fWTvpg)
- 整理相关学习资料，编写简单代码验证以下技术的实现
    + 查找对象
    ```c#

    ```
    + 添加子对象
    ```c#

    ```
    + 遍历对象树
    ```c#

    ```
    + 清除所有对象
    ```c#

    ```
- 资源预设(Prefabs)与对象克隆(clone)
    + 预设（Prefabs）有什么好处？
        * 预置是资源的一种，用于创建实例。对预置的调整可以应用于所有由这个预置产生的实例，提高了代码复用性。
    + 预设与对象克隆 (clone or copy or Instantiate of Unity Object) 关系？
        * 对象克隆是创建了一个和原有对象相同的对象，但新对象和原有对象没有直接关系，修改其中一个另一个并不会被修改。
    + 制作 table 预制，写一段代码将 table 预制资源实例化成游戏对象
- 尝试解释组合模式(Composite Pattern / 一种设计模式)。使用BroadcastMessage()方法
    + 向子对象发送消息
