## 格式

>导航:
>
><font size="3">UI规范</font> 
>
>​	<font size="2">[控件简介](#UI理念)</font> 

<b>`TabTextView`</b>继承了ThemeConstraintLayout,用作显示带角标的文本容器.

## UI规范

### UI理念

### 应用场景

带有角标提醒的文本，例如ThemeTabLayout.Tab（书柜页的作品更新）、消息提醒（发现页的关注、我的页面消息提醒等）等

### 组成

- ① ThemeConstraintLayout
- ② ThemeTextView(文本填充,默认字体大小24dp)
- ③ 角标(可选, 参[属性摘要](####属性摘要)`dotType` )

### UI设计

### 行为效果

按需选择角标类型及控制角标显示与否.

通常角标作为弱提醒功能,点击或查看后应有以下响应:

> 如果为红点角标,点击相应Tab后,应该消失
>
> 如果未数字角标,查看相应消息后,数字角标的数字应该随着发生变化（参类`UnreadMsgController`）

## 使用说明

### 摘要

#### 属性摘要

| 属性    | 类型   | 作用                                               | 默认值 |
| ------- | ------ | -------------------------------------------------- | ------ |
| dotType | enum   | 角标类型(with_dot: 红点   with_num:数字  none: 无) | none   |
| textMt  | string | tab文本内容                                        | 无     |

#### api摘要

##### getTextView

获取TextView

##### setDotViewType

设置角标类型

##### upDateDotView

更新角标状态

### 示例

+ 标题栏

```xml
<mobi.mangatoon.widget.textview.TabTextView
  android:id="@id/navIcon1"
  android:layout_width="40dp"
  android:layout_height="44dp"
  app:layout_constraintEnd_toStartOf="@+id/navIcon2"
  app:layout_constraintTop_toTopOf="parent"
  android:background="@drawable/ripple_mask_oval_30dp" />
```

+ 导航栏

  ```java
    override fun newTab(): Tab {
      val tab = super.newTab()
      val root = TabTextView(context)
      root.setDotViewType(DotView.TYPE_DOT)
      root.layoutParams = FrameLayout.LayoutParams(FrameLayout.LayoutParams.WRAP_CONTENT, MTDeviceUtil.dip2px(30))
      tab.customView = root
      (root.textView as? ThemeTextView)?.run {
        setTextColorStyle(0)
        isTitleStyle().yes { textSize = 16f }.otherwise { textSize = 14f }
        setTextColor(getSelectedSelectableColor())
        typeface = TypefaceUtil.getAppTypefaceRegular(this.context)
      }
      root.setDotViewType(dotViewType)
      tab.view.run {
        minimumWidth = 0
        minimumHeight = MTDeviceUtil.dip2px(36)
        ViewCompat.setPaddingRelative(
          this, padding10, 0, padding10, padding2)
      }
      return tab
    }
  ```

  
