---
title: Unity 3D HomeWork1
date: 2018-03-21 12:00:04
categories:
- Unity
---

HomeWork1 中基本概念部分。

 简答题
### 解释游戏对象(GameObjects)和资源(Assets)的区别和联系
- 区别：
    - 游戏对象(GameObject)是场景中的所有实体的基类，可以视为一个空的容器。当以GameObject为父类的继承中添加子类特有的属性之后，就构成了在游戏中看到的形态和行为各异的角色和场景了。每个游戏对象实例都在游戏对象森林（Hierarchy）中有自己的位置。
    - 资源(Assets)包括游戏数据和脚本代码等等。
- 联系：资源可以提供给现有的游戏对象调用，也可以创建新的游戏对象实例，两者共同作用形成游戏。

### 编写一个代码，使用debug语句来验证[MonoBehaviour](https://docs.unity3d.com/ScriptReference/MonoBehaviour.html)基本行为或者事件的触发条件
- 基本行为包括 Awake() Start() Update() FixedUpdate() LateUpdate()
- 常用事件包括 OnGUI() OnDisable() OnEnable()

```c
    public class InitBeh : MonoBehaviour {
        private int awakeCount;
        private int startCount;
        private int updateCount;
        private int fixedUpdateCount;
        private int laterUpdateCount;
        private int onGUICount;

        // Use this for initialization
        void Start() {
            if(startCount++ < 3)
                Debug.Log("Start ");
        }

        // Update is called once per frame
        void Update () {
            if(updateCount++ < 3)
                Debug.Log("Update ");
        }

        //First run in project
        private void Awake() {
            awakeCount = 1;
            startCount = updateCount = fixedUpdateCount = laterUpdateCount = onGUICount = 0;
            Debug.Log("Awake ");
            awakeCount++;
        }

        private void FixedUpdate() {
            if (fixedUpdateCount++ < 3)
                Debug.Log("FixedUpdate ");
        }
        private void LateUpdate() {
            if(laterUpdateCount++ < 3)
                Debug.Log("LateUpdate ");
        }
        private void OnGUI() {
            if(onGUICount++ < 3)
                Debug.Log("OnGUI ");
        }
        private void OnDisable() {
            Debug.Log("OnDisable");
        }
        private void OnEnable() {
            Debug.Log("OnEnable");
        }
    }
```


**输出结果**  
    ![FirstPart](http://i4.bvimg.com/618639/321710527c288258.png)
    ![Second](http://i4.bvimg.com/618639/d3183b375074539a.png)
    ![Third](http://i4.bvimg.com/618639/f6ed3b5832dffafe.png)

**总结**  
|Method|Description|  
|------|----------:|  
|Awake|当一个脚本实例被载入时(when script object is initialised)调用|  
|Start|在所有Update函数之前被调用一次(when a script is enabled)|  
|Update|当行为启用时，其Update在每一帧被调用| 
|FixedUpdate|当行为启用时，其FixedUpdate在每一**时间片**被调用|  
|LateUpdate|当行为启用时，在每一帧的Update函数之后执行|  
|OnGUI|用于渲染和处理GUI时间，一帧可能调用许多次|  
|OnDisable|行为失效或者对象销毁时调用|  
|OnEnable|对象使能时候调用|  

### 查找脚本手册，了解GameObject，Transform，Conponent对象
- 分别翻译官方对三个对象的描述(Description)  
    - **GameObjects**: Base class for all entities in Unity scenes. 游戏对象是Unity场景中所有实例的基类。
    - **Transform**: Position, rotation and scale of an object. Every object in a scene has a Transform. It's used to store and manipulate the position, rotation and scale of the object. Every Transform can have a parent, which allows you to apply position, rotation and scale hierarchically. 对象的位置、旋转和大小。每一个对象都有一个Transform来储存和操作它的位置、旋转和大小。每个Transform对象都有一个父类使得开发者可以将位置、旋转和大小的信息继承地应用。
    - **Components**: Base class for everything attached to GameObjects. Note that your code will never directly create a Component. Instead, you write script code, and attach the script to a GameObject. 任何与游戏对象有关联的对象的基类。代码并不会直接创建一个组件，而是通过把写好的脚本代码关联到一个游戏对象上。
        
- 描述下图中table对象（实体）的属性、table的Transform的属性、table的部件
        ![homework](https://pan.baidu.com/s/1myqv_r1PAmFZDPofJdTk5A)
- 用UML图描述三者的关系
        ![UML](https://pan.baidu.com/s/1ZZfPtp6HLmQo8vU7fWTvpg)

### 写简单代码验证以下技术的实现
- 查找对象 : 只有对象在active状态才能找到 & 别在每帧都调用的函数中使用 
```c
    //by name of object
    //if gameObject named "chair1" does not exist, return null
    var chair1 = GameObject.Find("/table/chair1");

    //obtain single gameObject by tag
    var chair1 = GameObject.FindWithTag("chair1");

    //obtain multiple gameObjects by tag
    //return a list of gameObject
    var chair = GameObject.FindGameObjectsWithTag("chair");  
    
    //obtain single gameObject by type
    var table = GameObject.FindObjectOfType(Type table);

    //obtain multiple gameObject by type
    var tables = GameObject.FindObjectsOfType(Type table);
```
    
- 添加子对象
    ```c
    GameObject.CreatePrimitive(PrimitiveType);
    ```
    
- 遍历对象树

    ```c
    foreach（Transform t in transform）｛
        //body
    ｝
    ```
- 清除所有对象  
{% endhighlight %}
    ```c
    GameObject obj= transform.FindChild("table").gameObject;
    foreach (Transform child in obj.transform) {  
        GameObject.Destroy(child.gameObject);  
    }  
    ```

### 资源预设(Prefabs)与对象克隆(clone)
- 预设（Prefabs）有什么好处？
    - 预置是资源的一种，用于创建实例。对预置的调整可以应用于所有由这个预置产生的实例，提高了代码复用性。
- 预设与对象克隆 (clone or copy or Instantiate of Unity Object) 关系？
    -  对象克隆是创建了一个和原有对象相同的对象，但新对象和原有对象没有直接关系，修改其中一个另一个并不会被修改。
- 制作 table 预制，写一段代码将 table 预制资源实例化成游戏对象

### 尝试解释组合模式(Composite Pattern / 一种设计模式)。使用BroadcastMessage()方法
- 向子对象发送消息
