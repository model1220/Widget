## 导航栏

<b>`ThemeTablayoutWrapper`</b>导航栏组件简介:

进一步对[ThemeTablayout](https://gitlab.mangatoon.mobi/android/mangatoon-android-docs/blob/master/%E5%9F%BA%E7%A1%80%E6%8E%A7%E4%BB%B6%E6%96%87%E6%A1%A3/%E5%AF%BC%E8%88%AA%E6%A0%8F-ThemeTabLayout.md)进行封装,增加了右侧单/双图标支持、底部分割线等,,并进行了局部模糊处理.该控件用作最顶级导航栏.

## UI规范

### UI理念

### 应用场景

### 组成

- ① ThemeTabLayout
- ② ThemeView 过渡蒙层
- ③ NavTextView icon1 图标1
- ② NavTextView icon2 图标2
- ③ ThemeLineView 底部分割线

### UI设计

## 使用

### 开发理念

对ThemeTabLayout进行封装,增加UI装饰.

如果UI对底部分割线或图标有需求的话,可以使用该控件,否则建议使用[ThemeTabLayout](链接)

1. 如无特殊要求layout_width 使用<b>`match_parent`</b>, layout_height使用<b>`wrap_content`</b>
2. 根据需求来设置[属性](#### 属性)

### 摘要

#### 属性

| 属性           | 类型    | 作用                                   |
| -------------- | ------- | -------------------------------------- |
| tabLayoutStyle | enum    | 导航栏风格                             |
| withUnderLine  | boolean | 是否显示底部分割线                     |
| tabIconType    | enum    | ThemeTabLayout的Tab角标类型(红点/数字) |
| iconType1      | enum    | icon1角标类型(红点/数字)               |
| iconType2      | enum    | icon2 角标类型(红点/数字)              |

#### api

##### initIcon

初始化指定位置的icon内容及设置点击事件

```java
tabLayoutWrapper?.apply {
  initIcon(
    ThemeTabLayoutWrapper.IconPosition.ICON_POSITION_1,
    getString(R.string.icon_write),
    null)
  initIcon(
    ThemeTabLayoutWrapper.IconPosition.ICON_POSITION_2,
    getString(R.string.icon_channel),
    null)
}
```

##### getIconView

获取指定位置的icon

```java
val msgController = NewFunctionMsgController(
  NewFunctionMsgController.channel,
  getIconView(ThemeTabLayoutWrapper.IconPosition.ICON_POSITION_2),
  null)
```

##### getThemeTabLayout

获取ThemeTabLayout,以对Tablayout进行操作

##### getCustomView

获取指定tab的customView,可以进行更新角标状态等

```java
private fun createTabMediator(): TabLayoutMediator? {
  val tabLayout = tabLayout ?: return null
  val viewPager = viewPager ?: return null
  return TabLayoutMediator(tabLayout, viewPager) { tab, position ->
    adapter?.let {
      tabLayoutWrapper?.getCustomView(tab)?.updateDotView(it.isDotVisible(position))
      tab.text = it.getPageTitle(position)
      logTabShowEvent(position)
    }
  }
}
```

### 代码示例

+ 导航栏-发现页

 ``` xml
  <mobi.mangatoon.widget.tablayout.ThemeTabLayoutWrapper
    android:id="@+id/tabLayoutWrapper"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:tabLayoutStyle="title"
    app:iconType1="onlyIconText"
    app:iconType2="with_num"
    app:withUnderLine="true"
    app:tabIconType="with_dot"
    app:layout_constraintTop_toTopOf="parent"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent" />
 ```

<div align="center">
<img src="../images/导航栏-发现页.jpg" height="200px" alt="图片说明" style="zoom:100%;" >
</div>

+ 导航栏-书柜

```xml
<mobi.mangatoon.widget.tablayout.ThemeTabLayoutWrapper
  android:id="@+id/tabLayout"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  app:tabLayoutStyle="fixedSubtitle"
  app:withUnderLine="true"
  app:tabIconType="with_dot" />
```

<div align="center">
<img src="../images/导航栏-书柜.png" height="200px" alt="图片说明" style="zoom:100%;" >
</div>