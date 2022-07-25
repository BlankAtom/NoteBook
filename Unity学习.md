# Unity2D

1、物体的运动

```c#

// 物体的坐标和旋转角都是3向量组
Vector3 pos = new Vector3(0, 1f, 0);
Vector3 angle = new Vector3(0, 0, 45f);

// 世界位置
// localPosition 相对位置
transform.position = pos;

// eulerAngles 世界角度
// localEulerAngles 相对角度
transform.eulerAngles = angle;

// 移动
// Space.Self 相对位置
// Space.World 世界坐标
transform.Translate(0, 0, 0, Space.Self);

// 物体的方向变换
void Update(){
    if (this.transform.position.y > 10 || this.transform.position.y < -10)
    {
        upDirection = !upDirection;
        Vector3 v3 = new Vector3(0, 0, upDirection ? 0 : 180);
        transform.localEulerAngles = v3;
    }

    float step = 2f * Time.deltaTime;
    transform.Translate(0, step, 0, Space.Self);
}
```



2、向量

获取向量的模长：

`Vector.magnitude`



获取向量的单位向量

`Vector.normalize`



向量夹角

`存在正负：Vector.SignedAngle(起始向量，最终向量，旋转轴)`

`不存在正负：Vector.Angle()`





3、屏幕坐标

`Screen.width`

`Screen.height`

获取一个物体的屏幕坐标

`Vector3 pos = Camera.main.WorldToScreenPoint(worldPos)



4、屏幕边界（左右

`Vector3 sp = Camera.main.WorldToScreenPoint(transform.postion)`

`sp.x > Screen.width`

`sp.x<0`



5、参数

可以在脚本中定义值类型参数和引用类型参数（精灵参数、GameObject参数）



## 鼠标事件

```c#

// 鼠标向量
var mousePosition = Input.mousePosition;
// 主角向量
var position = transform.position;
// 屏幕坐标
var screenPoint = Camera.main.WorldToScreenPoint(position);
// 获取新的向量
var newDirect = mousePosition - screenPoint;
newDirect.z = 0f;

// 旋转
newDirect = newDirect.normalized;
transform.up = newDirect;
// transform.Rotate(newDirect);

// 向着鼠标方向移动，点击加速
var moveSpeed = Input.GetMouseButton(0) ? fastSpeed : speed;
transform.Translate(Vector3.up * moveSpeed * Time.deltaTime); 
```





## 动态创建实例

```c#
// Instantiate

/**
 * Get bullet from properties --> Instantiate(bullet) --> transform of bullet
 *
 */

GameObject go = Instantiate(bullet);
go.transform.Translate();
// go.transform.position = Vector3();


// 销毁
// Destroy(object)
```





## 定时器

`InvokeRepeating`

## 碰撞检测

> 在MonoBehavior类中，有碰撞时的回调函数：
>
> `OnCollisionEnter()`：发生碰撞时触发一次
>
> `OnCollisionExit()`：碰撞解除时触发一次
>
> `OnCollisionStay()`：整个碰撞过程持续调用



## 接触检测

> 勾选Box Collider中的`is Trigger`，那么物体就不会和其他刚体发生碰撞，而是会穿过去，并触发接触函数回调。
>
> `OnTriggerEnter`
>
> `OnTriggerExit`
>
> `OnTriggerStay`



## 精灵的大小

```c#
Sprite sprite = this.images[index];
float imgWidth = sprite.rect.width;
float scale = 100 / imgWidth;
xxx.transform.localScale = new Vector3(scale, scale, scale)
```



## 声音的播放

```c#
AudioSource audio = GetComponent<AudioSource>();
audio.play();
audio.playOnShot() // 重开一个播放
```





## 延时

`Invoke(methodName, seconds)`

`CancelInvoke`取消延时

`IsInvoking`是否正在延时



详细的参看官方api



## 消息调用

```c#
// 1. Normal
GameObject main = GameObject.Find("GameController");
MyGame mygame = main.GetComponent<MyGame>();
myGame.AddScore(1);

// 2. Message send
GameObject main = GameObject.Find("GC");
main.SendMessage("MethodName", parameters);
// 如果没有找到这个方法，会输出异常信息
// 同步调用，不是异步
```





# Api 笔记

## Update、FixedUpdate、LateUpdate

### `MonoBehaviour.Update`

每一帧都会调用



### `MonoBehaviour.FixedUpdate` 固定更新

每一帧都会调用，处理Rigidbody时，需要用其替代Update。在物理处理，例如给刚体增加作用力时，必须在FixedUpdate中固定帧，而不是Update，二者的帧长不同。



### `MonoBehaviour.LateUpdate`  晚于更新

在所有Update函数调用后被调用。

可以用于调整脚本的执行顺序。

例如物体在Update中移动时，跟随物体的相机可以在LateUpdate中实现。



### Update和FixedUpdate的区别

`Update`和当前平台的帧数有关，而FixedUpdate是依据真实时间的，所以处理物理逻辑的时候要把代码放在`FixedUpdate`而不是`Update`。

`Update`是在每次渲染新的一帧的时候才会调用，也就是说，这个函数的更新频率和设备的性能有关以及被渲染的物体（可以认为是三角形的数量）。在性能好的机器上可能fps 30，差的可能小些。这会导致同一个游戏在不同的机器上效果不一致，有的快有的慢。因为`Update`的执行间隔不一样了。

而`FixedUpdate`，是在固定的时间间隔执行，不受游戏帧率的影响。有点想`Tick`。所以处理Rigidbody的时候最好用`FixedUpdate`。

PS：

> `FixedUpdate`的时间间隔可以在项目设置中更改，Edit->ProjectSetting->time 找到`Fixedtimestep`。就可以修改了。



### Update和LateUpdate的区别

在圣典里LateUpdate被解释成一句话：LateUpdate是在所有Update函数调用后被调用。这可用于调整脚本执行顺序。例如:当物体在Update里移动时，跟随物体的相机可以在LateUpdate里实现。这句我看了云里雾里的，后来看了别人的解释才明白过来。

LateUpdate是晚于所有Update执行的。例如：游戏中有2个脚步，脚步1含有Update和LateUpdate，脚步2含有Update，那么当游戏执行时，每一帧都是把2个脚步中的Update执行完后才执行LateUpdate 。虽然是在同一帧中执行的，但是Update会先执行，LateUpdate会晚执行。

现在假设有2个不同的脚本同时在Update中控制一个物体，那么当其中一个脚本改变物体方位、旋转或者其他参数时，另一个脚步也在改变这些东西，那么这个物体的方位、旋转就会出现一定的反复。如果还有个物体在Update中跟随这个物体移动、旋转的话，那跟随的物体就会出现抖动。 如果是在LateUpdate中跟随的话就会只跟随所有Update执行完后的最后位置、旋转，这样就**防止了抖动**。





# Unity 3D

## 原理

### 协程

[从 各种点 理解Unity中的协程 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/59619632)



### GL

[Unity3D研究 GL详解_Htlas的博客](https://blog.csdn.net/Htlas/article/details/79748512)



### 游戏暂停

[Unity3D研究院之Time.timeScale、游戏暂停（七十四） | 雨松MOMO程序研究院 (xuanyusong.com)](http://www.xuanyusong.com/archives/2956)





## 移动

### 1. RigidBody.MovePosition()

```C#
void Update() {
    float h = Input.GetAxis("Horizontal");
    float v = Input.GetAxis("Vertical");

    RigidBody.MovePosition(transform.position + new Vector3(h, 0, v) * speed * Time.deltaTime);
}
```



### 2. transform.Translate()

```c#
transform.Translate(new Vector3(h, 0, v) * speed * Time.deltaTime);
```



### 3. Vector3.MoveTowards()

```C#
// 鼠标位置
Vector3 mousePos = Input.mousePosition;
// 转为射线
Ray ray = Camera.main.ScreenPointToRay(mousePos);
// 记录碰撞
RaycastInfo hitInfo;
// 产生射线
bool isCast = Physics.Raycast(ray, out hitInfo, 1000, inputMast);
if(isCast){
    targetPos = hitInfo.point;
}

Vector3　pos = Vector3.MoveTowards(this._transform.position, targetPos, speed * Time.deltaTime);

this._transform.position = pos;
```



### 其他

1、贴地移动

```C#
RaycastHit hitInfo;
Vector3 desUp = Vector3.up;
var isCast = Physics.Raycast(towards, Vector3.down, out hitInfo);
if (isCast)
{
    towards.y = (hitInfo.point + Vector3.up).y;
    desUp = hitInfo.normal;
}
......
transform.up = Vector3.Slerp(transform.up, desUp, Time.deltaTime * speed);
```







## 车的设计

### 1、速度

```C#
// 一般使用三种方式实现速度
// 1、为刚体Rigid Body增加方向力，使其按照力的方向发生位移和加速。
// 优势是方便受力分析和多力的叠加

// 2、为刚体直接添加一个方向上的速度值，velocity

// 3、当使用Wheel Colliders 的时候，可以给单个Wheel增加Motor Toque值，使其加速，增加 Brake Toque值，使其制动。
```





### 2、平衡杆

```c#
// 使用平衡杆系统，对游戏中的汽车模型进行物理优化，使其在进行物理碰撞或者转弯的时候，不至于在不高的速度之下发生翻车。

void FixedUpdate()
{
    WheelHit hit;
    var tl = 1f;
    var tr = 1f;

    var groundedLeft = left.GetGroundHit(out hit);
    if (groundedLeft)
    {
        tl = (-left.transform.InverseTransformPoint(hit.point).y - 
              left.radius) / left.suspensionDistance;
    }

    var groundedRight = right.GetGroundHit(out hit);
    if (groundedRight)
    {
        tr = (-right.transform.InverseTransformPoint(hit.point).y - 
              right.radius) / right.suspensionDistance;
    }

    // Complete 
    var antiRollForce = (tl - tr) * antiRoll;

    if (groundedLeft)
    {
        rigidbody.AddForceAtPosition(left.transform.up * -antiRollForce, 
                                     left.transform.position);
    }

    if (groundedRight)
    {
        rigidbody.AddForceAtPosition(right.transform.up * antiRollForce, 
                                     right.transform.position);
    }

}

```



### 3、获取输入

```C#
vertical = Input.GetAxis("Vertical");
horizontal = Input.GetAxis("Horizontal");
```



### 4、模型同步



### 5、车辆质心

```c#
Rigidbody.centerOfMass = massCenter.localPosition;
```





## 组件使用

### Input System

[Unity 如何使用新输入系统 New Input System_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1AJ41187)



## 机制设计



### Projector

1. 传递给Projector材质的RenderTexture必须是clamp模式，但是如果阴影到了RenderTexture边缘的像素，因为是Clamp的原因，地板就会出现整个长条形的阴影，解决方案可以通过给projector材质添加mask图来处理边缘的像素；

2. RenderTexture其实我们只需要表示产生阴影物体的位置，所以Camera使用的ReplaceShader可以使用最简单的shader，只写入一个通道值就可以了；



### 游戏内自定义按键的设计方法 一

点击按钮接收按键信息：

```C#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.EventSystems;
using UnityEngine.UI;


public class Mybutton : MonoBehaviour, IPointerExitHandler, IPointerClickHandler
{
    public KeyCode curinput = KeyCode.None;
    public Text Desplaytext;

    public Text tip;//提示

    bool isstart = false;//当前是否是修改状态

    private void Awake()
    {
        tip.enabled = false;
    }

    public void OnPointerClick(PointerEventData eventData)//按下按钮
    {
        isstart = true;
       
    }
    public void OnPointerExit(PointerEventData eventData)//鼠标移出按钮
    {
        isstart = false;
    }

    private void Update()
    {
        if (isstart)
        {
            tip.enabled =true ;
            if (Input.anyKeyDown)//检测到按键或者鼠标
            {
                foreach (KeyCode keyCode in Enum.GetValues(typeof(KeyCode)))
                {
                    if (Input.GetMouseButton(0)|| Input.GetMouseButton(1) || Input.GetMouseButton(2))
                    {
                        continue;//去除鼠标按键的影响
                    }
                    if (Input.GetKeyDown(keyCode))
                    {
                        curinput = keyCode;//按下的按钮
                    }
                }
            }
        }
        else
        {
            tip.enabled = false;
        }
        Desplaytext.text = curinput.ToString();
    }

   
}

```



### Shader 画网格

[(30条消息) Unity Shader 画网格_XinYueStudio的博客-CSDN博客_unity 网格shader](https://blog.csdn.net/qq_20589257/article/details/78688621)



### 血条渐变

[(30条消息) Unity3D 血条的渐变效果_m0_46208939的博客-CSDN博客](https://blog.csdn.net/m0_46208939/article/details/117985180)



## 其他

### 如何协作

[让Unity项目自带“版本控制”与“团队协作”能力 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/351409158)







# Unity API



## Physics

### OverlapCapsuleNonAlloc

public static int **OverlapCapsuleNonAlloc** ([Vector3](https://docs.unity.cn/cn/2019.4/ScriptReference/Vector3.html) **point0**, [Vector3](https://docs.unity.cn/cn/2019.4/ScriptReference/Vector3.html) **point1**, float **radius**, Collider[] **results**, int **layerMask**= AllLayers, [QueryTriggerInteraction](https://docs.unity.cn/cn/2019.4/ScriptReference/QueryTriggerInteraction.html)  **queryTriggerInteraction** = QueryTriggerInteraction.UseGlobal);

#### 参数

| | |
| -- | -- |
| point0                  | 胶囊体在 `start` 处的球体中心。                              |
| point1                  | 胶囊体在 `end` 处的球体中心。                                |
| radius                  | 胶囊体的半径。                                               |
| results                 | 用于存储结果的缓冲区。                                       |
| layerMask               | [层遮罩](https://docs.unity.cn/cn/2019.4/Manual/Layers.html)，用于在投射胶囊体时有选择地忽略碰撞体。 |
| queryTriggerInteraction | 指定该查询是否应该命中触发器。                               |

#### 返回

**int** 写入缓冲区的条目的数量。

#### 描述

在物理世界中检查给定胶囊体，并在用户提供的缓冲区中返回所有重叠的碰撞体。

与 [Physics.OverlapCapsule](https://docs.unity.cn/cn/2019.4/ScriptReference/Physics.OverlapCapsule.html) 相同，但不在**托管堆**上进行任何分配。
