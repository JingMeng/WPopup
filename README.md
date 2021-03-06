# WPopup - 一个简单使用并且高度定制的Popupwindow

<p >
	<a><img src="https://img.shields.io/github/release/wanglu1209/WPopup.svg"/></a>
  	<a><img src="https://img.shields.io/github/last-commit/wanglu1209/WPopup.svg"/></a>
	<a><img src="https://img.shields.io/github/issues/wanglu1209/WPopup.svg"/></a>
	<a><img src="https://img.shields.io/github/issues-closed/wanglu1209/WPopup.svg"/></a>
	<a><img src="https://img.shields.io/github/issues-pr/wanglu1209/WPopup.svg"/></a>
	<a><img src="https://img.shields.io/github/issues-pr-closed/wanglu1209/WPopup.svg"/></a>
	<a><img src="https://img.shields.io/github/forks/wanglu1209/WPopup.svg"/></a>
	<a><img src="https://img.shields.io/github/stars/wanglu1209/WPopup.svg"/></a>
</p>

<div>
<img src="https://raw.githubusercontent.com/wanglu1209/WPopup/master/img/gif.gif" width="200" height="350" />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

<img src="https://raw.githubusercontent.com/wanglu1209/WPopup/master/img/undo.gif" width="200" height="350" />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://raw.githubusercontent.com/wanglu1209/WPopup/master/img/friend_circle.gif" width="200" height="350" />

</div>

<br/>
<br/>
<br/>

- 自动设置show的位置
- 自动设置倒三角的位置
- 跟随手指点按位置弹出
- 几行代码设置朋友圈点击弹出点赞和回复弹出框（全部使用一个WPopup，完全不用担心复用问题！）
- 高度定制

## 依赖

```Gradle
allprojects {
	repositories {
		...
		maven { url 'https://jitpack.io' }
	}
}

dependencies {
        implementation 'com.github.wanglu1209:WPopup:lastRelease'
}
	
```


## 使用

### 普通操作

```Kotlin
// 创建WPopup
val pop = WPopup.Builder(this)
      .setData(data)    // 设置数据，数据类为WPopupModel
      .setCancelable(false) // 设置是否能点击外面dismiss
      .setPopupOrientation(WPopup.Builder.HORIZONTAL)   // 设置item排列方向 默认为竖向排列
      .setOnItemClickListener(object : WPopup.Builder.OnItemClickListener {
          override fun onItemClick(view: View, position: Int) {
              // 设置item点击事件
              Toast.makeText(view.context, data[position].text, Toast.LENGTH_LONG).show()
          }
      })
      .create()

```

#### 根据View自动设置显示位置
```Kotlin      
pop.showAtView(view)
```

#### 根据view手动设置显示位置
```Kotlin      
pop.showAtDirectionByView(view, WPopupDirection.LEFT)
```

**注意：如果使用类似朋友圈的效果，使用同一个WPopup在一个ListView中展现，则需要调用：**

```Kotlin
pop.showAtDirectionByListView(view, WPopupDirection.LEFT, position)

其中，position为本次点击的position
```


#### 自己选择弹出位置  上下左右
```Kotlin
pop.showAtDirection(WPopupDirection.BOTTOM)
```

<br/>
<br/>

### 骚操作 根据手指点击位置来显示

```Kotlin
val longClickPop = WPopup.Builder(this)
      .setData(longData)
      .setPopupOrientation(WPopup.Builder.VERTICAL)
      .setClickView(longClickView) // 点击的View，如果是RV/LV，则只需要传入RV/LV
      .setOnItemClickListener(object : WPopup.Builder.OnItemClickListener {
          override fun onItemClick(view: View, position: Int) {
              Toast.makeText(view.context, "$position", Toast.LENGTH_LONG).show()
          }
      })
      .create()
```

#### 自动挑选位置 默认为右下方

```Kotlin
longClickPop.showAtFingerLocation()
```

#### 手动挑选位置

```Kotlin
longClickPop.showAtFingerLocation(WPopupDirection.TOP)
```



<br/><br/><br/>

### 方法及参数

```Kotlin
data class WPopupModel(var text: String, var imgRes: Int = -1, var switchText: String = "", var switchImgRes: Int = -1)

text：必填参数，显示的文字
imgRes：非必填参数，显示的图片
switchText：非必填参数，点击之后切换的文字
switchImgRes：非必填参数，点击之后切换的图片
```

```Kotlin
object WPopupDirection{
    // 此为控制WPopup显示方向的类
    const val LEFT = -0x001
    const val RIGHT = -0x002
    const val BOTTOM = -0x003
    const val TOP = -0x004
    // 下面这些只适用于showAtFingerLocation() 也就是根据手指手动挑选位置
    const val LEFT_TOP = -0x005     // 左上
    const val LEFT_BOTTOM = -0x006  // 左下
    const val RIGHT_TOP = -0x007    // 右上
    const val RIGHT_BOTTOM = -0x008 // 右下
}
```

**WPopup已经为您设置好了通用的UI，您只需要自己配置参数即可**


方法名|参数类型|默认值|备注
:---|:--|:---|:--
setData|List`<WPopupModel>`|null|设置数据 参数为一个字符串，一个图片的resId
setCancelable|Boolean|false|设置点击外面是否能dismiss
setOnItemClickListener|OnItemClickListener|null|设置item点击事件
setPopupOrientation|String|WPopupWindow.Builder.VERTICAL|设置item的排列方向
setDividerColor|Int|Color.WHITE|设置分割线的颜色 
setDividerSize|Int|1|设置分割线的粗细
setDividerMargin|Int|10|设置分割线边距
setIsDim|Boolean|false|设置弹出时背景是否半透明
setDimValue|Float|0.4f|设置背景半透明的值  0.1f - 1f 值越大，越接近透明
setPopupBgColor|Int|Color.parseColor("#CC000000")|设置弹出背景颜色
setPopupMargin|Int|1|设置弹出时和view的距离
setClickView|View|null|设置长按事件的view
setAnim|Int|WPopupAnim.ANIM_ALPHA|设置动画
setIconDirection|Int|WPopupDirection.LEFT|设置icon的方向 如果data中设置了图片的话
setDrawablePadding|Int|5|设置drawablePadding
setTextColor|Int|Color.WHITE|设置item中text的颜色
setTextSize|Int|14|设置item中text的大小


**当然，如果默认的UI不适合您，您也可以自定义UI，只需传入一些参数即可**

```Kotlin
val customPopup = BasePopup(
      WPopParams(
              R.layout.view_custom, // layoutRes，必须参数
              this, // activity，必须参数
              true, // 背景是否变暗，非必须参数
              cancelable = false,
              width = ViewGroup.LayoutParams.MATCH_PARENT, // popup的宽 只支持LayoutParams里的
              height = ViewGroup.LayoutParams.WRAP_CONTENT
      )
)

```



