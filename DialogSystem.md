[TOC]

# 简单对话系统

## part 1					（UI）

首先创建UI方面来设置对白框，通过Canvas（画布）里的Panel（面板）创建

<img src="C:\Users\Pluto\AppData\Roaming\Typora\typora-user-images\image-20230409164630759.png" alt="image-20230409164630759" style="zoom: 80%;" />

panel设置为世界空间（此UI存在于指定的位置）

![image-20230409164830699](C:\Users\Pluto\AppData\Roaming\Typora\typora-user-images\image-20230409164830699.png)

此时注意给事件摄像机赋值，若不赋值，UI事件可能无响应，例如点击UI上的按钮无反应

但是这里其实没有影响，原因如下

（如果没有设立，自动以带有MainCamera Tag标签的Camera来检测。）

### 注意事项

设置为世界空间之后，部分物体无法被UI遮挡，原理是世界空间UI可以设置遮挡图层

![image-20230409165642509](C:\Users\Pluto\AppData\Roaming\Typora\typora-user-images\image-20230409165642509.png)

就对话系统的UI而言，图层一般是最高的

调整canvas大小

设置文本框  **按下t可以快捷调整大小**

## part 2					（实现）

创建DialogSystem脚本，挂载在panel上

![image-20230409172038087](C:\Users\Pluto\AppData\Roaming\Typora\typora-user-images\image-20230409172038087.png)

UI必须要使用这段话

```c#
using UnityEngine.UI;
```

![image-20230409174111547](C:\Users\Pluto\AppData\Roaming\Typora\typora-user-images\image-20230409174111547.png)

这里使用了一个名为TextAsset的类，文本文件资源，这样通过该类申明变量可以访问textFile中的内容

[UnityEngine.TextAsset - Unity 脚本 API](https://docs.unity.cn/cn/2019.4/ScriptReference/TextAsset.html)

此类中Text（）方法将整个文本转化为一整个字符串，然后一行一行读取字符串内容就可以获取文本信息

申明一个string类型的列表用来存储文本信息 之后转化分割获取每行的信息

![image-20230409182025763](C:\Users\Pluto\AppData\Roaming\Typora\typora-user-images\image-20230409182025763.png)

这样这个函数就实现了读取文件的功能，但是文本中是有A和B用于区分人物的，之后需要处理、



（题外话，在学了python之后对于面向对象有了更深刻的认识，也加深了对于c#中的语法的理解）

![image-20230409182830450](C:\Users\Pluto\AppData\Roaming\Typora\typora-user-images\image-20230409182830450.png)

此函数内容直接放在start中也行，但是注意开始时将index归零，其实我认为如果是想跳过A和B只需要index+2，但是需要根据A B来改变头像，那么就需要读取但特判，并作出相应的变化

### 注意事项

**这里函数还不完善，当读取到文本末尾时，index越界，需要判断然后退出对话系统，并重置内容**

解决方法：判断index是否等于列表的长度，里面count函数返回列表长度

![image-20230409184044742](C:\Users\Pluto\AppData\Roaming\Typora\typora-user-images\image-20230409184044742.png)

==值得注意的是，start函数在setactive时不会重新调用，而onenable和awake则会，所以这里需要将index归零，方便下次active时重复文本内容==

这里还有一些问题，当第二次调用时，开始是结尾的内容，较为简单的办法是先将文本设置为初始内容再关闭

![image-20230409184842589](C:\Users\Pluto\AppData\Roaming\Typora\typora-user-images\image-20230409184842589.png)

也可以使用onenable和awake来设置，因为onenable在active的时候直接调用，这样也能避免一开始文本框为空的情况

![image-20230409185729006](C:\Users\Pluto\AppData\Roaming\Typora\typora-user-images\image-20230409185729006.png)

这里index++的目的是转到第二句话，文本显示的还是第一句，但是index指向第二句了

## part 3					（视觉效果）

通过协程来让文字逐个输出，输出时间可控，其实主要还是协程的写法，IEnumerator  返回值

调用使用StartCoroutine

![image-20230409192528964](C:\Users\Pluto\AppData\Roaming\Typora\typora-user-images\image-20230409192528964.png)

这里一直是+=所以一直没有清空文本框，方法为index++前设置为当前index指向的文本

```c#
IEnumerator SetTextUI()
    {
    	textLabel.text = null;
        for (int i = 0; i < textList[index].Length; i++)
        {
            textLabel.text += textList[index][i];
            yield return new WaitForSeconds(0.1f);
        }
        textLabel.text = textList[index];
        index++;
    }
```

上面的代码其实可以删去textLabel.text = textList[index]

**特殊字符与人物头像**

==申明两个sprite的变量，不是image而是sprite==

### 注意事项

**最严重的问题是连续按时产生的乱码问题**

只需要设置一个布尔值，在onenable内设置为true，在协程内设置为false，在输入完成后设置为true



还有一个想法，中途按下任意键直接输入完毕，再按下继续下一个文本，这个实现也很简单

## part 4					（快速显示）

这个功能在dome中实现了，使用方法为协程，协程优点明显，缺点也明显，优点在于控制方便，缺点在于如果使用过多执行顺序混乱
