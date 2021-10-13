## 导航栏
<b>`ThemeTabLayout`</b>导航栏组件简介:
封装了TabLayout,统一了Tablayout使用风格和避免自定义Tab等.

## 使用

### 开发理念

该控件主要解决以下问题:

> Tab的customView自定义风格不一致的问题
>
> tab的选择/未选中状态自行定义字体色值、大小等的问题
>
> 角标状态与Tab状态脱离的问题
>
> 代码中差异化处理镜像布局的问题
>
> 保证能够面向vp和vp2具有表现一致性.

开发者在使用过程中,不必再去关心customView、镜像等问题,只需关注业务内容即可.

### 摘要

#### 属性

| 属性              | 类型   | 作用                     |
| ----------------- | ------ | ------------------------ |
| themeTabStyle     | enum   | 导航栏风格               |
| dotViewType       | enum   | Tab弱提醒风格(红点/数字) |
| tabIndicatorColor | String | 指示器颜色               |

+ 导航栏风格

|        themeTabStyle         |  类型   | 特征                                                         |
| :--------------------------: | :-----: | :----------------------------------------------------------- |
|            title             | boolean | 可滚动,tab指示器为宽度20dp且高度3dp半径1.5dp的圆角矩形(主题色); 未选中时字体16;选中时,字体大小18,颜色#CCCCCC(#333333) |
| fixedSubtitle/scrollSubtitle | String  | 不可滚动/可滚动,,tab指示器为宽度与tab文本宽度一致且高度2dp半径4dp的圆角矩形(主题色); 未选中时字体14;选中时,字体大小14,颜色#CCCCCC(#333333) |

#### api

##### 自定义tab内部view(customView)

ThemeTabLayout内部实现了自定义的风格统一的Tab,因此无需再调用TabLayout的setCustomView()方法来实现自定义tab内部view.

customView是一个宽度为LayoutParams.WRAP_CONTENT,高度为30dp的[TabTextView](链接).

该customView支持的功能有:

> 支持不同导航栏主题下的不同的颜色和字体大小设置
>
> 支持角标设置.

##### addOnTabSelectedListener

  控件内部调用了addOnTabSelectedListener方法,实现了选中和未选中状态时不同导航栏风格的Tab属性设置,无特殊情况,开发者无需再对Tab进行属性修改.

  开发者依旧可以调用addOnTabSelectedListener来做事件埋点等其他工作.

```java
mTabLayout.addOnTabSelectedListener(new TabLayout.OnTabSelectedListener() {
  @Override
  public void onTabSelected(TabLayout.Tab tab) {
    logPageEnter(); // 事件埋点
  }

  @Override
  public void onTabUnselected(TabLayout.Tab tab) {

  }

  @Override
  public void onTabReselected(TabLayout.Tab tab) {

  }
});
```

##### updateDotView  更新角标状态

调用updateDotView方法用来更新弱提醒状态(红点或数字)

```
FavoriteDbModel.getUpdatedCount(getContext()).doOnSuccess(updatedCount -> {
      // 书柜页-收藏更新小红点状态
      mTabLayout.updateDotView(mTabLayout.getTabAt(1), updatedCount > 0);
    }).subscribe();
```

### 代码示例

+ 导航栏-用户关注

 ``` xml
   <mobi.mangatoon.widget.tablayout.ThemeTabLayout
     android:id="@+id/tabLayout"
     android:layout_width="match_parent"
     android:layout_height="wrap_content"
     app:tabIndicatorColor="@color/mt_primary"
     app:themeTabStyle="fixedSubtitle" />
 ```
<div align="center">
<img src="../images/导航栏-用户关注.jpg" height="200px" alt="图片说明" style="zoom:100%;" >
</div>

