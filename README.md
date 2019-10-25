## NeImmersive  Android沉浸式和CardView用法
### 1. 沉浸式
* 在style.xml文件中设置
```android
    <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
         <!--5.0及其以上可以设置状态栏颜色-->
         <!--白色-->
         <!--<item name="android:statusBarColor">@android:color/transparent</item>-->
    
         <!--沉浸式-->
         <!--状态栏透明 4.0-->
         <item name="android:windowTranslucentStatus">true</item>
         <!--底部导航栏透明 4.0-->
         <item name="android:windowTranslucentNavigation">true</item>
    </style>
```
* 在Java代码中设置
```android
    private void immersive() {
        //4.0 以下不支持沉浸式
        if(Build.VERSION.SDK_INT < Build.VERSION_CODES.KITKAT) {
            return;
        }

        if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
            Window window = getWindow();
            window.clearFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS);
            window.addFlags(WindowManager.LayoutParams.FLAG_DRAWS_SYSTEM_BAR_BACKGROUNDS);
            //设置状态栏颜色透明
            window.setStatusBarColor(Color.TRANSPARENT);

            int visibility = window.getDecorView().getSystemUiVisibility();
            //布局内容全屏显示
            visibility |= View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN;
            //隐藏底部虚拟导航栏
            visibility |= View.SYSTEM_UI_FLAG_HIDE_NAVIGATION;
            //防止内容区域大小发生变化
            visibility |= View.SYSTEM_UI_FLAG_LAYOUT_STABLE;

            window.getDecorView().setSystemUiVisibility(visibility);
        }else {
            getWindow().addFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS);
        }
    }

    public int getStatusBarHeight(Context context) {
        int resId = context.getResources().getIdentifier("status_bar_height", "dimen", "android");
        if(resId > 0) {
            return context.getResources().getDimensionPixelSize(resId);
        }
        return 0;
    }

    public void setHeightAndPadding(Context context, View view) {
        ViewGroup.LayoutParams layoutParams = view.getLayoutParams();
        layoutParams.height += getStatusBarHeight(context);
        view.setPadding(view.getPaddingLeft(), view.getPaddingTop() + getStatusBarHeight(context),
                view.getPaddingRight(), view.getPaddingBottom());
    }
```
### 2. CardView
```xml
    <!--app:cardBackgroundColor="@color/colorPrimary"--> <!-- 设置CardView背景色-->
    <!--app:cardPreventCornerOverlap="false"--> <!-- 取消Lollipop 5.0以下版本的padding-->
    <!--app:cardUseCompatPadding="true"--> <!-- 为Lollipop 5.0及其以上版本增加一个阴影padding内边距-->
    <!--app:cardCornerRadius="8dp"--> <!-- 设置CardView圆角效果-->
    <!--app:cardElevation="10dp"--> <!-- 设置CardView Z轴阴影大小-->
    <!--app:cardMaxElevation="6dp"--> <!-- 设置CardView Z轴最大阴影-->
    <!--app:contentPaddingBottom="12dp"--> <!-- 设置内容的底部内边距-->
    <!--app:contentPaddingLeft="12dp"--> <!-- 设置内容的左边内边距-->
    <!--app:contentPaddingRight="12dp"--> <!-- 设置内容的右边内边距-->
    <!--app:contentPaddingTop="12dp"--> <!-- 设置内容的底顶内边距-->
    <android.support.v7.widget.CardView
        android:layout_width="200dp"
        android:layout_height="200dp"
        app:cardBackgroundColor="@color/colorPrimary"
        app:cardPreventCornerOverlap="false"
        app:cardUseCompatPadding="true"
        app:cardCornerRadius="8dp"
        app:cardElevation="10dp"
        app:cardMaxElevation="6dp"
        app:contentPaddingBottom="12dp"
        app:contentPaddingLeft="12dp"
        app:contentPaddingRight="12dp"
        app:contentPaddingTop="12dp"

        android:clickable="true"
        android:foreground="?attr/selectableItemBackground"
        android:layout_centerInParent="true">
        <TextView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:background="#FFFF00"
            android:text="cardView"
            android:gravity="center"/>
    </android.support.v7.widget.CardView>
```
### 3. 示例
![image](https://github.com/tianyalu/NeImmersive/blob/master/show/show.png)  

