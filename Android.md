# Android

## XML 命名空间

*   XML 命名空间提供避免元素冲突的方法
*   常见命名空间

    *   android

        ```xml
        xmlns:android=”http://schemas.android.com/apk/res/android”
        在Android布局文件中我们都必须在根元素上定义这样一个命名空间,接下来对这行代码进行逐一讲解：
        xmlns:即xml namespace，声明我们要开始定义一个命名空间了
        android：称作namespace-prefix，它是命名空间的名字
        http://schemas.android.com/apk/res/android：这看起来是一个URL，但是这个地址是不可访问的。实际上这是一个URI(统一资源标识符),所以它的值是固定不变的,相当于一个常量)。
        ```

        *   使用上一行代码，就会提示我们输入什么，也可以理解为语法文件。我们就可以引用命名空间中的属性，例如：
            ```
            <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            android:layout_width="match_parent"
            android:layout_height="match_parent">
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="center"
                android:text="New Text"
                android:id="@+id/textView" />
            </LinearLayout>
            ```
            在这个布局中，只要以 android 开头的属性便是引用命名空间中的属性
            android 是赋予命名空间的一个名字，就跟平时定义变量一样，也可以把它取名 myns，则上面布局中的 android 都可以替换为 myns，例如 android:layput_width 可以写成 myns:layout_width

    *   tools
        *   tools 有三种使用方法，tools 只作用于开发阶段，当 app 被打包时，所有关于 tools 的属性都会被摒弃
        *   tools:context 开发中查看 activity 布局效果 context 后面跟一个 activity 的完整包名，作用：当我们设置一个 activity 主题时，是在 androidManifest.xml 设置中，但是主题效果只能在运行后在 activity 中现实，使用 context 属性可以在开发阶段看到设置在 activity 中的主题效果
        ```
        tools:context=”com.littlehan.myapplication.MainActivity
        // 在布局中加入这行代码,就可以在design视图中看到与MainActivity绑定主题的效果。
        ```
    *   app
