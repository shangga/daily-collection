# Android

## XML 命名空间
### 定义
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

        *   如果使用 databinding 会在 xml 用到 app 属性，其实这是个自定义命名空间。xmlns:app=”http://schemas.android.com/apk/res-auto”，实际上也可以这么写：
            xmlns:app=”http://schemas.android.com/apk/res/完整的包名” 在 res/后面填写包名即可。但是，在 Android Studio2.0 上，是不推荐这么写的，所以建议大家还是用第一种的命名方法。

    *   继承 view 类

        *   View 是所有视图的父类，并实现它三个构造方法

        ```java
            public class CustomTextView extends View {
                private Paint mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);//画笔
                public CustomTextView(Context context) {
                    super(context);
                }
                public CustomTextView(Context context, AttributeSet attrs){
                    this(context, attrs, 0);//注意不是super(context,attrs,0);
                }
                public CustomTextView(Context context, AttributeSet attrs, int defStyleAttr){
                    super(context,attrs,defStyleAttr);
                }

                @Override
                protected void onDraw(Canvas canvas) {
                    super.onDraw(canvas);
                    canvas.drawText("I am a CustomTextView",100, 100, mPaint);
                }
            }
        ```

    *   使用自定义布局

    ```
        <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:background="#ffffff"
                >
            <com.littlehan.customtextview.CustomTextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content" />
            </RelativeLayout>
    ```

    *   但是这个自定义控件，并不能在 xml 中去改变字体颜色，字体大小、自定义文本等。这个功能的实现，需要 XML 创建自定义属性和在自定义 View 中解析属性
    *   自定义属性
        *   在 values 根目录下新建一个名为 attrs 的 xml 文件来自定义属性（自定义的属性便是自定义命名空间里面的属性）


        ```
           <?xml version="1.0" encoding="utf-8"?>
           <resources>
               <declare-styleable name="CustomTextView">
               <attr name="customColor" format="color"/>
               <attr name="customText" format="string"/>
               </declare-styleable>
           </resources>
        ```
        *   name 定义的是属性的名字
        *   format 定义的是属性的类型

    - 解析属性
    ```java
    public class CustomTextView extends View {
        private int mColor = Color.RED;//默认为红色
        private String mText="I am a Custom TextView";//默认显示该文本
        private Paint mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);//画笔
        public CustomTextView(Context context) {
            super(context);
            //        init();
        }
        public CustomTextView(Context context, AttributeSet attrs){
            this(context, attrs, 0);//注意不是super(context,attrs,0);
            init();
        }
        public CustomTextView(Context context, AttributeSet attrs, int defStyleAttr){//解析自定义属性
            super(context,attrs,defStyleAttr);
            TypedArray typedArray = context.obtainStyledAttributes(attrs,R.styleable.CustomTextView);
            mColor = typedArray.getColor(R.styleable.CustomTextView_customColor, Color.RED);
            //        如果没有判断，当没有指定该属性而去加载该属性app便会崩溃掉
            if(typedArray.getText(R.styleable.CustomTextView_customText) != null ){
                mText = typedArray.getText(R.styleable.CustomTextView_customText).toString();
            }
            typedArray.recycle();//释放资源
            init();
        }
        private void init(){
            mPaint.setColor(mColor);// 为画笔添加颜色
        }

        @Override
        protected void onDraw(Canvas canvas) {
            super.onDraw(canvas);
            canvas.drawText(mText, 100, 100, mPaint);
        }
    }
    ```
    - 使用自定义属性
        - 要使用自定义属性，就需要自定义属性命名空间，在布局文件的根元素下插入这样一行代码： xmlns:app=”http://schemas.android.com/apk/res-auto”
        ```
            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:app="http://schemas.android.com/apk/res-auto"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:background="#ffffff"
                >
            <com.littlehan.customtextview.CustomTextView
                app:customColor="@color/colorAccent"
                app:customText="Test Message"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content" />
            </RelativeLayout>
        ```

### 总结
在Android中，命名空间可分为3种:
- xmlns:android=”http://schemas.android.com/apk/res/android”
- xmlns:tools=”http://schemas.android.com/tools”
- xmlns:app=”http://schemas.android.com/apk/res-auto”
其中，1和2命名空间里的属性是系统封装好的，第3种命名空间里的属性是用户自定义的
