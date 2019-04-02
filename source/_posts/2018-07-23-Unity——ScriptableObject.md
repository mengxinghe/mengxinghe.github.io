---
title: Unity——ScriptableObject
tags:
  - Unity
  - ScriptableObject
categories:
  - Unity
abbrlink: 6ec434c5
date: 2018-07-23 14:08:09
comments:
---
`ScriptableObject ` 是Unity3D整个引擎的设计中，最为出彩的地方。通过他我们将数据保存，数据和编辑器的交互以及数据在runtime的使用三部分很方便的联系在一起。这是一个容易被Unity3D的初学者们容易忽略的领域。 

<!-- more -->

# ScriptableObject 简介

## 1.ScriptableObject是什么

`ScriptableObject`是一个用于生成单独Asset的结构。同时，它也能被称为是Unity中用于处理序列化的结构。 
在Unity里面有单独的序列化结构，所有的Object(`UnityEngine.Object`)都能够通过这个方法进行数据的序列化与反序列化。文件和Unity编辑器都能够方便的获取其中的数据。
Unity内部的Asset(Material或者AnimationClip等)都是从`UnityEngine.Object`衍生出来的。为了制作单独的Asset，需要制作`UnityEngine.Object`的子类。不过对于用户而言是不允许制作`UnityEngine.Object`的子类。所以用户如果要利用Unity中的序列化结构、生成单独的Asset，就必须借助`ScriptableObject`。

## 2.制作ScriptableObject

为了制作`ScriptableObject`，继承`ScriptableObject`基类是首先要做的事情。这个时候，类名与Asset名必须要一样。另外，`ScriptableObject`的限制和`MonoBehaviour`是一样的。

```
using UnityEngine;
public class ExampleAsset : ScriptableObject
{
}
```

## 3.实例化

通过`ScriptableObject.CreateInstance`来生成`ScriptableObject`。使用`new`操作符来生成是不行的。理由和`MonoBehaviour`是一样的，Unity生成Object的时候必须经过序列化。

```
using UnityEngine;
using UnityEditor;
public class ExampleAsset : ScriptableObject
{
    [MenuItem ("Example/Create ExampleAsset Instance")]
    static void CreateExampleAssetInstance ()
    {
        var exampleAsset = CreateInstance<ExampleAsset> ();
    }
}
```

## 4.作为Asset进行保存

实例化完成后是将Object作为Asset进行保存。通过`AssetDatabase.CreateAsset`就能够生成Asset。 
Asset的后缀名必须是.asset。如果是其他后缀名的话，Unity会无法识别。

```
[MenuItem ("Example/Create ExampleAsset")]
static void CreateExampleAsset ()
{
    var exampleAsset = CreateInstance<ExampleAsset> ();

    AssetDatabase.CreateAsset (exampleAsset, "Assets/Editor/ExampleAsset.asset");
    AssetDatabase.Refresh ();
}
```

![](http://anchan828.github.io/editor-manual/web/images/scriptableobject/ss01.png)

另外，使用`CreateAssetMenu`属性的话会使得Asset的制作更加简单。

```
using UnityEngine;
using UnityEditor;

[CreateAssetMenu(menuName = "Example/Create ExampleAsset Instance")]
public class ExampleAsset : ScriptableObject
{
}1234567
```

使用`CreateAssetMenu`的情况下，会在[Assets/Create]下面生成菜单。 

![](http://anchan828.github.io/editor-manual/web/images/scriptableobject/ss07.png)

## 5.从脚本中读取ScriptableObject

读取的方法很简单，只要使用`AssetDatabase.LoadAssetAtPath`就可以了。

```
[MenuItem ("Example/Load ExampleAsset")]
static void LoadExampleAsset ()
{
    var exampleAsset =
    　　　　AssetDatabase.LoadAssetAtPath<ExampleAsset>
                               ("Assets/Editor/ExampleAsset.asset");
}1234567
```

## 6.在Inspector上显示属性

和`MonoBehaviour`一样，在字段上附加`SerializeField`属性就能够使字段显示出来。另外，`PropertyDrawer`在这里也同样适用。 
![image](http://anchan828.github.io/editor-manual/web/images/scriptableobject/ss03.png)

```
using UnityEngine;
using UnityEditor;

public class ExampleAsset : ScriptableObject
{
    [SerializeField]
    string str;

    [SerializeField, Range (0, 10)]
    int number;

    [MenuItem ("Example/Create ExampleAsset Instance")]
    static void CreateExampleAssetInstance ()
    {
        var exampleAsset = CreateInstance<ExampleAsset> ();

        AssetDatabase.CreateAsset (exampleAsset, "Assets/Editor/ExampleAsset.asset");
        AssetDatabase.Refresh ();
    }
}1234567891011121314151617181920
```

## 7. ScriptableObject的继承关系

首先，请想象一下父类以及持有父类变量的子类。 
下面则是具体的实现代码。 
父类

```
using UnityEngine;

public class ParentScriptableObject : ScriptableObject
{
    [SerializeField]
    ChildScriptableObject child;
}1234567
```

子类

```
using UnityEngine;

public class ChildScriptableObject : ScriptableObject
{
  //什么都没有的话Inpector很孤单的，所以添加一个
  [SerializeField]
  string str;

  public ChildScriptableObject ()
  {
    //一开始Asset的名字
    name = "New ChildScriptableObject";
  }
}1234567891011121314
```

**注：这里在构造函数里面设置name有可能报错，建议放在Awake或者OnEnbale中** 
接着，`ParentScriptableObject`作为asset被保存起来。含有自变量的子类也试着实例化吧。 
父类

```
using UnityEngine;
using UnityEditor;

public class ParentScriptableObject : ScriptableObject
{
  const string PATH = "Assets/Editor/New ParentScriptableObject.asset";

  [SerializeField]
  ChildScriptableObject child;

  [MenuItem ("Assets/Create ScriptableObject")]
  static void CreateScriptableObject ()
  {
    //父类实例化
    var parent = ScriptableObject.CreateInstance<ParentScriptableObject> ();

    //子类实例化
    parent.child = ScriptableObject.CreateInstance<ChildScriptableObject> ();

    //把父类作为asset保存起来
    AssetDatabase.CreateAsset (parent, PATH);

    //使用import刷新状态
    AssetDatabase.ImportAsset (PATH);
  }
}1234567891011121314151617181920212223242526
```

`ParentScriptableObject`保存成Asset后，可以看一下它的Inspector。如图所示，字段child变成了**Type mismatch**。 
![image](http://anchan828.github.io/editor-manual/web/images/scriptableobject/ss08.png) 
试着双击一下显示着**Type mismatch**的地方，`ChildScriptableObject`的信息会显示在Inspector上面。看上去并没有什么问题，很正常的切换过去了。 
![image](http://anchan828.github.io/editor-manual/web/images/scriptableobject/ss09.png)

想要把UnityEngine.Object当做Asset处理就必须保存在硬盘上

持有**Type mismatch**子类的父类生成完成后，就这样重新启动Unity。第二次查看`ParentScriptableObject`的Inspector会发现child部分显示为None(null)。 
![image](http://anchan828.github.io/editor-manual/web/images/scriptableobject/ss10.png) 
这个是因为作为`ScriptableObject`父类的`UnityEngine.Object`被作为序列化数据处理的时候，必须要以Asset的形式保存到硬盘上。**Type mismatch**的状态下，虽然在Inspector上看着没问题，不过代表着硬盘上并不存在对应的Asset文件。也就是说，这个实例一旦消失(例如重启Unity等情况)，那么就无法方位这个数据了。

## 8. ScriptableObject作为Asset进行保存

想要避免**Type mismatch**很简单。只要把`ScriptableObject`全部作为Asset保存下来就可以了。 
![image](http://anchan828.github.io/editor-manual/web/images/scriptableobject/ss11.png)
不过，像现在这样存在着父类与子类关系的情况下，制作各种单独的Asset从管理角度来看并不是一个上策。如果子类的数量增加，一方面要用一个列表来进行管理，另一方面届时生成Asset后的文件管理也非常的麻烦。 
Unity提供了SubAsset这样一种功能来用于整理具有继承关系的Asset。

### 8.1 SubAsset

在作为MainAsset的Asset添加`UnityEngine.Object`就能作为SubAsset来处理。SubAsset中最容易理解的例子要数Model Asset。 
在Model Asset里面，会含有Mesh或者Animation之类的Asset。通常来说，这些必须要单独存在。不过对于SubAsset来说，Mesh或者AniamtionClip这些Asset的信息都会被包含在Main Asset里面。 
![image](http://anchan828.github.io/editor-manual/web/images/scriptableobject/ss12.png) 
`ScriptableObject`也能够使用SubAsset的功能，因而在硬盘上不容易增添的具有继承关系的`ScirptableObject`也能够构建起来。 。

### 8.2 AssetDatabase.AddObjectToAsset

想要在`UnityEngine.Object`上追加SubAsset，只需要在想要成为MainAsset的Asset上添加Object就可以了。 
父类`ScriptableObject`

```
using UnityEngine;
using UnityEditor;

public class ParentScriptableObject : ScriptableObject
{
  const string PATH = "Assets/Editor/New ParentScriptableObject.asset";

  [SerializeField]
  ChildScriptableObject child;

  [MenuItem ("Assets/Create ScriptableObject")]
  static void CreateScriptableObject ()
  {
    //父类实例化
    var parent = ScriptableObject.CreateInstance<ParentScriptableObject> ();

    //子类实例化
    parent.child = ScriptableObject.CreateInstance<ChildScriptableObject> ();

    //在父类上添加子类
    AssetDatabase.AddObjectToAsset (parent.child, PATH);

    //将父类作为Asset保存
    AssetDatabase.CreateAsset (parent, PATH);

    //调用Import更新状态
    AssetDatabase.ImportAsset (PATH);
  }
}1234567891011121314151617181920212223242526272829
```

从下图中可以发现 ，有两个父类的`ParentScriptableObject`。不过持有实际数据的则是在SubAsset中的`ParentScriptableObject`。 
![image](http://anchan828.github.io/editor-manual/web/images/scriptableobject/ss13.png) 
这种状态，是Unity判断用户生成了特殊的Asset(例如生成SubAsset)，因此MainAsset使用DefaultAsset进行表示。 
**译注：这里的信息已然过时，使用Unity5.6时并未出现该现象，而是只有一个父和一个子**

### 8.3 使用HideFlags.HideInHierarchy隐藏SubAssets

还可以做到隐藏SubAsset，只有MainAsset显示出来。

```
[MenuItem ("Assets/Create ScriptableObject")]
static void CreateScriptableObject ()
{
  var parent = ScriptableObject.CreateInstance<ParentScriptableObject> ();
  parent.child = ScriptableObject.CreateInstance<ChildScriptableObject> ();

　//隐藏SubAsset中的子物体
  parent.child.hideFlags = HideFlags.HideInHierarchy;

  AssetDatabase.AddObjectToAsset (parent.child, PATH);

  AssetDatabase.CreateAsset (parent, PATH);
  AssetDatabase.ImportAsset (PATH);
}1234567891011121314
```

通过这种方式，Asset就不再以多层的表示，而是整理到了一个进行表示。 
![image](http://anchan828.github.io/editor-manual/web/images/scriptableobject/ss14.png)
这种隐藏SubAsset的方法也能够用在`AnimatorController`上面。让我们确认一下。

```
[MenuItem ("Assets/Set to HideFlags.None")]
static void SetHideFlags ()
{
  //选中AnimatorController的状态下弹出菜单
  var path = AssetDatabase.GetAssetPath (Selection.activeObject);

  //获取SubAsset里面的所有东西
  foreach (var item in AssetDatabase.LoadAllAssetsAtPath(path)) {
    //把全部的标志位设置为None，解除隐藏状态
    item.hideFlags = HideFlags.None;
  }
  //用Import进行刷新
  AssetDatabase.ImportAsset (path);
}1234567891011121314
```

运行上面的代码，会解除所有的`HideFlags`，然后所有的SubAsset都会表示出来。 
![image](http://anchan828.github.io/editor-manual/web/images/scriptableobject/ss15.png)

### 8.4 从MainAsset里面删除SubAsset

SubAsset的删除含简单。只要使用`Object.DestoryImmediate`就能够删除SubAsset。

```
[MenuItem ("Assets/Remove ChildScriptableObject")]
static void Remove ()
{
  var parent = AssetDatabase.LoadAssetAtPath<ParentScriptableObject> (PATH);

  //删除子物体
  Object.DestroyImmediate (parent.child, true);

  //删除后会成为Missing状态，用Null取代
  parent.child = null;

  //用Import更新状态
  AssetDatabase.ImportAsset (PATH);
}1234567891011121314
```

## 9. Icon的修改

桌面图标类似于![image](http://anchan828.github.io/editor-manual/web/images/scriptableobject/ScriptableObject.png)。虽然不是特别重要，但也有修改的方法。

### 9.1 在脚本中设置Icon

选中Script Asset后，再选中Icon部分，会弹出来修改Icon的面板。这里有一个[Other]按钮可以点击，然后选择想要变更的纹理。 
![image](http://anchan828.github.io/editor-manual/web/images/scriptableobject/ss04.png)
![image](http://anchan828.github.io/editor-manual/web/images/scriptableobject/ss05.png)

### 9.2 把Icon放在Gizmos里面

还有一种修改Icon的方法就是在Gizmos文件夹里面放置图片文件，并取名为[ScriptableObject类名 Icon]。Gizmos文件夹是存在于assets文件夹下面，使用起来可能稍微有些麻烦。不过不会出现有三个一模一样的Icon并列的情况，从这一点来说很方便。 
![image](http://anchan828.github.io/editor-manual/web/images/scriptableobject/ss06.png)