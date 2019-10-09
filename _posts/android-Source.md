###### AsyncTask
1. AsyncTasks should ideally be used for short operations (a few seconds at the most.)
```
If you need to keep threads running for long periods of time, it is highly recommended you use the various APIs provided by the <code>java.util.concurrent</code> package such as {@link Executor}, {@link ThreadPoolExecutor} and {@link FutureTask}.
```

2. AsyncTask must be subclassed to be used. The subclass will override at least one method ({@link #doInBackground}), and most often will override a second one ({@link #onPostExecute}.)

3. When an asynchronous task is executed, the task goes through 4 steps
```
1.onPreExecute
2.doInBackground
3.onProgressUpdate（可在2.调用publishProgress  Each call to this method
will trigger the execution of {@link #onProgressUpdate} on the UI thread. 触发）
4.onPostExecute
```

4. ***There are a few threading rules that must be followed for this class to work properly***
```
1.The AsyncTask class must be loaded on the UI thread
2.The task instance must be created on the UI thread
execute} must be invoked on the UI thread
3.Do not call {@link #onPreExecute()}, {@link #onPostExecute},{@link #doInBackground}, {@link #onProgressUpdate} manually
4.The task can be executed only once (an exception will be thrown if a second execution is attempted.
```
5. Memory observability
```
Set member fields in the constructor or {@link #onPreExecute}, and refer to them in {@link #doInBackground}.
Set member fields in {@link #doInBackground}, and refer to them in{@link #onProgressUpdate} and {@link #onPostExecute}
```

6. Code
```
// We want at least 2 threads and at most 4 threads in the core pool,
   // preferring to have 1 less than the CPU count to avoid saturating
   // the CPU with background work
   private static final int CORE_POOL_SIZE = Math.max(2, Math.min(CPU_COUNT - 1, 4));
   private static final int MAXIMUM_POOL_SIZE = CPU_COUNT * 2 + 1;
   private static final int KEEP_ALIVE_SECONDS = 30;
```

7. If you write your own implementation, do not call <code>super.onCancelled(result)</code>.

8. Attempts to cancel execution of this task.  This attempt will fail if the task has already completed, already been cancelled, or could not be cancelled for some other reason.
*** public final boolean cancel(boolean mayInterruptIfRunning)*** After invoking this method, ***you should check the value returned by {@link #isCancelled()***} periodically from  {@link #doInBackground(Object[])} to finish the task as early as possible.</p>

9. ***TODO***
```
Future get
Atomic
ThreadFactory
BlockingQueue
ThreadPoolExecutor
WorkerRunnable
FutureTask
```

10.  java 内部类是默认持有一个外部类的引用，因为 jvm 在把.java 源文件编译成 .class 字节码的时候，会在默认的构造函数加入外部类的引用。所以我们在内部类中也能访问外部类的引用

11. ***AsyncTask注意事项：***

```
  a.内存泄漏:静态内部类持有外部类的匿名引用，导致外部对象无法得到释放，解决方法很简单，让内部持有外部的弱引用即可解决
  b.生命周期:在Activity的onDestory()中及时对AsyncTask进行回收，调用其cancel()方法来保证程序的稳定性。
  c.结果丢失:当内存不足时，当前的Activity被回收，由于AsyncTask持有的是回收之前Activity的引用，导致AsyncTask更新的结果对象为一个无效的Activity的引用，这就是结果丢失。
  d.并行或串行:在1.6(Donut)之前: 在第一版的AsyncTask，任务是串行调度。一个任务执行完成另一个才能执行。由于串行执行任务，使用多个AsyncTask可能会带来有些问题。所以这并不是一个很好的处理异步（尤其是需要将结果作用于UI试图）操作的方法。1.6-2.3： 所有的任务并发执行，这会导致一种情况，就是其中一条任务执行出问题了，会引起其他任务出现错误。3.0之后AsyncTask又修改为了顺序执行，并且新添加了一个函数 executeOnExecutor(Executor)，如果您需要并行执行，则只需要调用该函数，并把参数设置为并行执行即可。
```


###### HandlerThread
1. Any attempt to post messages to the queue after the looper is asked to quit will fail.
For example, the {@link Handler#sendMessage(Message)} method will return false.
</p><p class="note"> Using this method may be unsafe because some messages may not be delivered before the looper terminates.  ***Consider using {@link #quitSafely}*** instead to ensure that all pending work is completed in an orderly manner.</p>

2. 开启子线程进行耗时操作，多次创建和销毁子线程是很耗费资源的

3. 在Android开发中，不熟悉多线程开发的人一想到要使用线程，可能就用new Thread(){…}.start()这样的方式。实质上在只有单个耗时任务时用这种方式是可以的，但若是有多个耗时任务要串行执行呢？那不得要多次创建多次销毁线程，这样导致的代价是很耗系统资源，容易存在性能问题。那么，怎么解决呢？
我们可以只创建一个工作线程，然后在里面循环处理耗时任务，创建过程如下：
```
Handler mHandler;
private void createWorkerThread() {
  new Thread() {
      @Override
        public void run() {
            super.run();
            Looper.prepare();
             mHandler = new Handler(Looper.myLooper()) {
                @Override
                public void handleMessage(Message msg) {
                    ......
                }
            };
            Looper.loop();
        }
    }.start();
}
```

4. **start后，onLooperPrepared不是立刻执行。注意nullpointexception**

###### IntentService
1. IntentService is subject to all thebackground execution limits imposed with Android 8.0 (API level 26). **In most cases, you are better off using {@link android.support.v4.app.JobIntentService}, which uses jobs instead of services when running on Android 8.0 or higher.**

2. If enabled is true. **public void setIntentRedelivery(boolean enabled)**
 If multiple Intents have been sent, only the most recent one is guaranteed to be redelivered.

3. 使用场景 ***每个耗时任务都按顺序依次执行***

```
多个耗时任务需要顺序依次执行的问题

在之前做的app中，有一个需求是下载某段时间用户保存的图片，下载完成后显示在imageView中；
一般需要下载的图片有很多，每下一个图片就是一个线程，下载完后立即显示出来，所以肯定希望下载是按顺序依次下载，这样用户体验就比较好。
当时的处理方式欠妥，直接开多个线程去下载，在前一个图片下载完成之前，其他线程必须等待；这样虽然也可以实现功能，但效率上不高，甚至可能出现ANR（具体代码逻辑是，如果后面的线程获得了cpu,而前面的图片还没下完，则等待；假设前一张图片没下完，而后面的线程一直获得cpu，就有问题）。

```

4. IntentService Serivce+handler的结合产物，可以在onHandleIntent直接处理耗时操作。
而本地service和远程service不能在onStart方法中执行耗时操作，只能放在子线程中进行处理，当有新的intent请求过来都会调用onStartCommond先将其入队列。
当第一个耗时操作结束后，就会处理下一个耗时操作(此时调用onHandleIntent)，都执行完了自动执行onDestory销毁IntentService服务。


##### View
1.   If this view doesn't do any drawing on its own, set this flag to allow further optimizations. By default, ***this flag is not set on View, but could be set on some View subclasses such as ViewGroup***  Typically, if you override {@link #onDraw(android.graphics.Canvas)} you should clear this flag. @param willNotDraw whether or not this View draw on its own  **该标记位的作用是：当一个View不需要绘制内容时，系统进行相应优化  默认情况下：View 不启用该标记位（设置为false）；ViewGroup 默认启用（设置为true）**
```
    public void setWillNotDraw(boolean willNotDraw) {
        setFlags(willNotDraw ? WILL_NOT_DRAW : 0, DRAW_MASK);
    }
```

2. 为什么你的自定义View wrap_content不起作用？
```
如果没设置默认值，就继续往上层VIew充满大小，即从父View的大小等于顶层View的大小（），那么子View的大小 = 父View的大小
如果设置了默认值，就用默认值。
```

##### 事件分发

1. dispatchTouchEvent

```
public boolean dispatchTouchEvent(MotionEvent event) {
    if (mOnTouchListener != null && (mViewFlags & ENABLED_MASK) == ENABLED &&
            mOnTouchListener.onTouch(this, event)) {
        return true;
    }
    return onTouchEvent(event);
}

public boolean onTouchEvent(MotionEvent event) {
    final int viewFlags = mViewFlags;
    if ((viewFlags & ENABLED_MASK) == DISABLED) {
        // A disabled view that is clickable still consumes the touch
        // events, it just doesn't respond to them.
        return (((viewFlags & CLICKABLE) == CLICKABLE ||
                (viewFlags & LONG_CLICKABLE) == LONG_CLICKABLE));
    }
    if (mTouchDelegate != null) {
        if (mTouchDelegate.onTouchEvent(event)) {
            return true;
        }
    }
    if (((viewFlags & CLICKABLE) == CLICKABLE ||
            (viewFlags & LONG_CLICKABLE) == LONG_CLICKABLE)) {
        switch (event.getAction()) {
            case MotionEvent.ACTION_UP:
                                performClick();
            case
            .
            .
            .
        }
        return true;
    }
    return false;
}
```

##### extra
1. Button TextView区别

```
Button com.android.internal.R.attr.buttonStyle
TextView com.android.internal.R.attr.textViewStyle
themes.xml下
    <item name="buttonStyle">@style/Widget.Button</item>
    <item name="textViewStyle">@style/Widget.TextView</item>
styles.xml下   
    <style name="Widget.Button">
        <item name="background">@drawable/btn_default</item>
        <item name="focusable">true</item>
        <item name="clickable">true</item>
        <item name="textAppearance">?attr/textAppearanceSmallInverse</item>
        <item name="textColor">@color/primary_text_light</item>
        <item name="gravity">center_vertical|center_horizontal</item>
    </style>
    <style name="Widget.TextView">
    <item name="textAppearance">?attr/textAppearanceSmall</item>
    <item name="textSelectHandleLeft">?attr/textSelectHandleLeft</item>
    <item name="textSelectHandleRight">?attr/textSelectHandleRight</item>
    <item name="textSelectHandle">?attr/textSelectHandle</item>
    <item name="textEditPasteWindowLayout">?attr/textEditPasteWindowLayout</item>
    <item name="textEditNoPasteWindowLayout">?attr/textEditNoPasteWindowLayout</item>
    <item name="textEditSidePasteWindowLayout">?attr/textEditSidePasteWindowLayout</item>
    <item name="textEditSideNoPasteWindowLayout">?attr/textEditSideNoPasteWindowLayout</item>
    <item name="textEditSuggestionItemLayout">?attr/textEditSuggestionItemLayout</item>
    <item name="textEditSuggestionContainerLayout">?attr/textEditSuggestionContainerLayout</item>
    <item name="textEditSuggestionHighlightStyle">?attr/textEditSuggestionHighlightStyle</item>
    <item name="textCursorDrawable">?attr/textCursorDrawable</item>
    <item name="breakStrategy">high_quality</item>
    <item name="hyphenationFrequency">normal</item>
</style>
```




##### ListView
1. convertView重用机制
2. ViewHolder机制
3. 三级缓冲/滑动监听事件
```
优化一：在Adapter中的getView方法中使用ConvertView，即ConvertView的复用，不需要每次都inflate一个View出来，这样既浪费时间，又浪费内存。
优化二：使用ViewHolder，不要在getView方法中写findViewById方法，因为getView方法会执行很多遍，这样也可以节省时间，节约内存。
优化三：使用分页加载，讲真实际开发中，ListView的数据肯定不止几百条，成千上万条数据你不可能一次性加载出来，所以这里需要用到分页加载，一次加载几条或者十几条，但是如果数据量很大的话，像qq，微信这种，如果顺利加载到最后面的话，那么你的list中也会有几万甚至几十万的数据，这样可能也会导致OOM，所以你的数据集List中也不能有那么多数据，所以每加载一页的时候你可以覆盖前一页的数据。
优化四：如果数据当中有图片的话，使用第三方库来加载(也就是缓存)，如果你的能力强大到能自己维护的话，那也不是不可以。
优化五：当你手指在滑动列表的时候，尽可能的不加载图片，这样的话滑动就会更加流畅。
```
4.
Displays a vertically-scrollable collection of views, where each view is positioned
immediatelybelow the previous view in the list.  For a more modern, flexible, and performant
approach to displaying lists, use {@link android.support.v7.widget.RecyclerView}.

##### RecyclerView和ListView
```
1..Adapter的getView方法里面convertView没有使用setTag和getTag方式；
2.在getView方法里面ViewHolder初始化后的赋值或者是多个控件的显示状态和背景的显示没有优化好，抑或是里面含有复杂的计算和耗时操作；
3.在getView方法里面 inflate的row 嵌套太深（布局过于复杂）或者是布局里面有大图片或者背景所致；
4.Adapter多余或者不合理的notifySetDataChanged；
5.listview 被多层嵌套，多次的onMessure导致卡顿，如果多层嵌套无法避免，建议把listview的高和宽设置为fill_parent. 如果是代码继承的listview，那么也请你别忘记为你的继承类添加上LayoutPrams，注意高和宽都是fill_parent的;


本文讲解的主要是通过预处理布局来解决ListView和RecyclerView的卡顿问题。

ListView与RecyclerView的卡顿我将之分为两个阶段：

1）  绑定数据阶段  

这个阶段优化网上有很多说法，也已经被大家所熟知。 比如： 图片通过框架延迟加载，懒加载；  禁止在这里做耗时操作， 如： findViewById ,  创建对象， 处理业务逻辑， 处理复杂的计算逻辑等； 

2） 初始化layout阶段 

本文重点介绍的是这个阶段的优化， 有人会问， ListView通过viewHolder 通过判定convertView == null  这个不是固化的操作吗， 还有啥优化空间？ 还能避免这一步？？？ RecyclerView 也是一样。 

答案当然是否定的。 是的， 我们不能避免这些固化的写法， 不能避免不去创建inflate layout .  那这个阶段还有啥可以优化的呢？？？ 

首先我要提出问题所在： 在ListView和RecyclerView被加载出来显示之后， 再滑动会出现卡顿， 而后就顺畅多了， 问题是显示之后， 在滑动为啥会卡顿呢 ？？

RecyclerView 虽然比ListView灵活的多， 但是滚动时的卡顿确比ListView更明显， 有些人有疑问 ：这难道不是绑定数据造成的吗？  其实不然。 是在滚动时会额外通过getView或者onCreateViewHolder  里面继续inflate.layout造成的。 很多有又有疑问， 为啥我没有这个感觉呢？？ 那是因为很多layout不复杂的情况下效果很轻微， 但是如果你设置了header， 第一屏幕显示的都是header view ， 而list是从第2页屏幕开始显示的呢？？？ 你有试过吗， 如果试过或者知道它们的缓存原理就会发现， 再RecyclerView或者ListView被显示出来的时候， getView或者onCreateViewHolder 并没有被执行， 或者执行很少， 只有在滚动时才获取执行， 然后就造成了卡顿。 那是因为它们的缓存机制导致， 初始化只加载可视范围内的item （这里不细细谈到它们的缓存机制，如果需要验证， 那么请在getView 或者onCreateViewHolder 哪里添加打印） 。 而滚动时inflate.layout  如果不复杂可能10ms就解决了， 你还能流程的体验， 但是如果复杂， 那么就会体验到滚动开始有轻微的卡顿（丢帧） ， 知道它们的缓存机制完全足以支撑你的滚动， 不再去重新inflate.layout 。 



说了这么多， 没有解决办法， 然并卵 ？ 

ListView 的预处理加载item , 也就是让它多加载几个item , 使之滚动的时候， 不去创建layout ， 而是使用缓存好的item 。 

Method method = ShareReflectUtil.findMethod(this,"addViewBelow",View.class, int.class);

intposition = getLastVisiblePosition();

View view = getChildAt(position);

for(inti =0;i < size;i++) {

       method.invoke(this,view,i + position);

}

RecyclerView的处理办法： 

RecyclerView.RecycledViewPool pool = recyclerView.getRecycledViewPool();

pool.setMaxRecycledViews(viewType,count);

for(intindex =0;index < count;index++) {

      pool.putRecycledView(recyclerView.getAdapter().createViewHolder(recyclerView,0));

}

注意一定要在setAdapter之后调用， 否者无效， 因为setAdapter时会清除你的pool缓存。
```

##### RecyclerView

1. position
```
Beware that these methods may not be able to calculate adapter positions if {@link Adapter#notifyDataSetChanged()} has been called and new layout has not yet been calculated. For this reasons, you should carefully handle {@link #NO_POSITION} or <code>null</code> results from these methods.
```

##### ItemTouchHelper
```
This is a utility class to add swipe to dismiss and drag & drop support to RecyclerView
```

##### Animation

1. 监听
```
若采取上述方法监听动画，每次监听都必须重写4个方法
背景：有些时候我们并不需要监听动画的所有时刻
问题：但上述方式是必须需要重写4个时刻的方法，这显示太累赘
解决方案：采用动画适配器AnimatorListenerAdapter，解决
实现接口繁琐 的问题
具体如下：
anim.addListener(new AnimatorListenerAdapter() {  
// 向addListener()方法中传入适配器对象AnimatorListenerAdapter()
// 由于AnimatorListenerAdapter中已经实现好每个接口
// 所以这里不实现全部方法也不会报错
    @Override  
    public void onAnimationStart(Animator animation) {  
    // 如想只想监听动画开始时刻，就只需要单独重写该方法就可以
    }  
});  
```

2. 插值器Interpolator和估值器TypeEvaluator
```
插值器设置 属性值 从初始值过渡到结束值 的变化规律
估值器设置 属性值 从初始值过渡到结束值 的变化具体数值,e.g. 圆移动
```

3. 补间动画
```
activity 动画切换
fragment  动画切换

  视图组中子元素的出场效果
  步骤1：设置子元素的出场动画
  <?xml version="1.0" encoding="utf-8"?>
  // 此处采用了组合动画
  <set xmlns:android="http://schemas.android.com/apk/res/android" >
      android:duration="3000"
      <alpha
          android:duration="1500"
          android:fromAlpha="1.0"
          android:toAlpha="0.0" />
      <translate
          android:fromXDelta="500"
          android:toXDelta="0"
           />
  </set>
  步骤2：设置 视图组（ViewGroup）的动画文件
  <?xml version="1.0" encoding="utf-8"?>
  // 采用LayoutAnimation标签
<layoutAnimation xmlns:android="http://schemas.android.com/apk/res/android"
    android:delay="0.5"
    // 子元素开始动画的时间延迟
    // 如子元素入场动画的时间总长设置为300ms
    // 那么 delay = "0.5" 表示每个子元素都会延迟150ms才会播放动画效果
    // 第一个子元素延迟150ms播放入场效果；第二个延迟300ms，以此类推
    android:animationOrder="normal"
    // 表示子元素动画的顺序
    // 可设置属性为：
    // 1. normal ：顺序显示，即排在前面的子元素先播放入场动画
    // 2. reverse：倒序显示，即排在后面的子元素先播放入场动画
    // 3. random：随机播放入场动画
    android:animation="@anim/view_animation"
    // 设置入场的具体动画效果
    // 将步骤1的子元素出场动画设置到这里
    />
    步骤3：为视图组（ViewGroup）指定andorid:layoutAnimation属性
    <ListView
        android:id="@+id/listView1"
        android:layoutAnimation="@anim/anim_layout"
        // 指定layoutAnimation属性用以指定子元素的入场动画
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
```

4. 组合动画
```
AnimatorSet.play(Animator anim)   ：播放当前动画
AnimatorSet.after(long delay)   ：将现有动画延迟x毫秒后执行
AnimatorSet.with(Animator anim)   ：将现有动画和传入的动画同时执行
AnimatorSet.after(Animator anim)   ：将现有动画插入到传入的动画之后执行
AnimatorSet.before(Animator anim) ：  将现有动画插入到传入的动画之前执行
  // 步骤1：设置需要组合的动画效果
  ObjectAnimator translation = ObjectAnimator.ofFloat(mButton, "translationX", curTranslationX, 300,curTranslationX);  
  // 平移动画
  ObjectAnimator rotate = ObjectAnimator.ofFloat(mButton, "rotation", 0f, 360f);  
  // 旋转动画
  ObjectAnimator alpha = ObjectAnimator.ofFloat(mButton, "alpha", 1f, 0f, 1f);  
  // 透明度动画

  // 步骤2：创建组合动画的对象
  AnimatorSet animSet = new AnimatorSet();  

  // 步骤3：根据需求组合动画
  animSet.play(translation).with(rotate).before(alpha);  
  animSet.setDuration(5000);  

  // 步骤4：启动动画
  animSet.start();  
```
