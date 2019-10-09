Activity
 生命周期，启动模式，scheme

Broadcast
  本地广播是无法通过静态注册来实现的。因为静态注册是为了让程序未启动也能接收广播。本地广播是在本程序内进行传递，肯定是已经启动了，因此也完全不需要静态注册。
  sendBroadcast   sendBroadcastSync区别。sendBroadcastSync思考其他线程处理的场景？

Service
  bindService
  扩展Binder类  如果我们的服务只是自有应用的后台工作线程，则优先采用这种方法。 不采用该方式创建接口的唯一原因是，服务被其他应用或不同的进程调用
  使用Messenger

      * Create a new Messenger pointing to the given Handler.  Any Message
      * objects sent through this Messenger will appear in the Handler as if
      * {@link Handler#sendMessage(Message) Handler.sendMessage(Message)} had
      * been called directly.
      * target The Handler that will receive sent messages.


        public Messenger(Handler target) {
          mTarget = target.getIMessenger();
        }

    由于要测试不同进程的交互，则需要将Service放在单独的进程中，因此Service声明如下：
    <service android:name=".messenger.MessengerService"
           android:process=":remote"
          />

    因为Message和Messenger都实现了Parcelable接口，可以轻松跨进程传递数据（关于Parcelable接口可以看博主的另一篇文章：序列化与反序列化之Parcelable和Serializable浅析），而Message可以传递的信息载体有，what,arg1,arg2,Bundle以及replyTo，至于object字段，对于同一进程中的数据传递确实很实用，但对于进程间的通信，则显得相当尴尬，在android2.2前，object不支持跨进程传输，但即便是android2.2之后也只能传递android系统提供的实现了Parcelable接口的对象，也就是说我们通过自定义实现Parcelable接口的对象无法通过object字段来传递，因此object字段的实用性在跨进程中也变得相当低了。不过所幸我们还有Bundle对象，Bundle可以支持大量的数据类型

    通常情况下我们应该在客户端生命周期（如Activity的生命周期）的引入 (bring-up) 和退出 (tear-down) 时刻设置绑定和取消绑定操作，以便控制绑定状态下的Service，一般有以下两种情况：

    如果只需要在 Activity 可见时与服务交互，则应在 onStart() 期间绑定，在 onStop() 期间取消绑定。

    如果希望 Activity 在后台停止运行状态下仍可接收响应，则可在 onCreate() 期间绑定，在 onDestroy() 期间取消绑定。需要注意的是，这意味着 Activity 在其整个运行过程中（甚至包括后台运行期间）都需要使用服务，因此如果服务位于其他进程内，那么当提高该进程的权重时，系统很可能会终止该进程。


    我们应该始终捕获 DeadObjectException DeadObjectException 异常，该异常是在连接中断时引发的，表示调用的对象已死亡，也就是Service对象已销毁，这是远程方法引发的唯一异常，DeadObjectException继承自RemoteException，因此我们也可以捕获RemoteException异常。

    显式启动，隐式启动

    AIDL
      创建 AIDL
      创建要操作的实体类，实现 Parcelable 接口，以便序列化/反序列化
      新建 aidl 文件夹，在其中创建接口 aidl 文件以及实体类的映射 aidl 文件
      Make project ，生成 Binder 的 Java 文件
      服务端
      创建 Service，在其中创建上面生成的 Binder 对象实例，实现接口定义的方法
      在 onBind() 中返回
      客户端
      实现 ServiceConnection 接口，在其中拿到 AIDL 类
      bindService()
      调用 AIDL 类中定义好的操作请求


Fragment
    替代方案  Flow 为我们创造了一个简单，方便，以 View 为中心的应用架构

webView
    3.4 注意事项：如何避免WebView内存泄露？
    3.4.1 不在xml中定义 Webview ，而是在需要的时候在Activity中创建，并且Context使用 getApplicationgContext()

    LinearLayout.LayoutParams params = new LinearLayout.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT);
            mWebView = new WebView(getApplicationContext());
            mWebView.setLayoutParams(params);
            mLayout.addView(mWebView);
    3.4.2 在 Activity 销毁（ WebView ）的时候，先让 WebView 加载null内容，然后移除 WebView，再销毁 WebView，最后置空。
        protected void onDestroy() {
            if (mWebView != null) {
                mWebView.loadDataWithBaseURL(null, "", "text/html", "utf-8", null);
                mWebView.clearHistory();

                ((ViewGroup) mWebView.getParent()).removeView(mWebView);
                mWebView.destroy();
                mWebView = null;
            }
            super.onDestroy();
        }


        1.WebView常见的坑
        a.API 16之前版本存在远程代码执行漏洞，该漏洞源自于程序没有正确限制使用WebView.addJavascriptInterface方法，攻击者可以使用Java Reflection API利用该漏洞执行任意Java对象和方法。

        b.WebView的销毁和内存泄漏问题。WebView的完全销毁是件麻烦事，一旦销毁流程不正确，极易容易导致内存泄漏。http://blog.csdn.net/xuguobiao/article/details/51473016

        c.jsbridge
          通过javascript构建一个桥，桥的两端一个端是客户端，一端是服务端，它可以让本地端调用远端的web代码，也可以让远端调用本地的代码。

        d.WebViewClient.onPageFinished(不靠谱) –> WebChromeClient.onProgressChanged(靠谱)

        e.后台耗电(性能优化)
          WebView开启网页时会自己开启线程，如果没有合理的销毁，那么残余线程就会一直运行，so这会非常耗电的，解决方案：有一种暴力方式就是Activity的onDestroy中调用System.exit()方法把虚拟机关闭，也可以结合自己应用的WebView的情况设计出一个温柔的方案。

        f.WebView硬件加速导致的页面渲染问题
          WebView硬件加速偶尔导致页面白块，页面闪烁，但是加载速度比较快，解决方案：关闭硬件加速。

        2.WebView的内存泄漏问题
          原因：WebView会关联一个Activity,WebView执行的操作是在新线程当中回收，时间Activity没有办法确认，Activity的生命周期和WebView线程生命周期不一致导致WebView一直执行，因为WebView内部持有Activity的引用，导致Activity一直不能被回收，原理类似于匿名内部类持有外部类的引用一样。那么如何解决呢？解决方案如下：

        a.独立进程，简单暴力，涉及到进程间通信。(开发过程中常用)

        b.动态添加WebView,对传入WebView中使用Context弱引用，动态添加WebView意思在布局中创建一个ViewGroup用来放置WebView,Activity创建add进来，Activity停止时remove掉。


Handler

      在Java中，非静态的内部类和匿名内部类都会隐式地持有其外部类的引用。静态的内部类不会持有外部类的引用。
      确实上面的代码示例有点难以察觉内存泄露，那么下面的例子就非常明显了

      public class SampleActivity extends Activity {

      private final Handler mLeakyHandler = new Handler() {
        @Override
        public void handleMessage(Message msg) {
          // ...
        }
      }

      @Override
      protected void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);

      // Post a message and delay its execution for 10 minutes.
      mLeakyHandler.postDelayed(new Runnable() {
        @Override
        public void run() {
          ...
        }
      }, 1000 * 60 * 10);

      // Go back to the previous Activity.
      finish();
      }
    }
    分析一下上面的代码，当我们执行了Activity的finish方法，被延迟的消息会在被处理之前存在于主线程消息队列中10分钟，而这个消息中又包含了Handler的引用，而Handler是一个匿名内部类的实例，其持有外面的SampleActivity的引用，所以这导致了SampleActivity无法回收，进行导致SampleActivity持有的很多资源都无法回收，这就是我们常说的内存泄露。


      ****************************
      ****************************
      ****************************
    注意上面的new Runnable这里也是匿名内部类实现的，同样也会持有SampleActivity的引用，也会阻止SampleActivity被回收。

    要解决这种问题，思路就是不适用非静态内部类，继承Handler时，要么是放在单独的类文件中，要么就是使用静态内部类。因为静态的内部类不会持有外部类的引用，所以不会导致外部类实例的内存泄露。当你需要在静态内部类中调用外部的Activity时，我们可以使用弱引用来处理。另外关于同样也需要将Runnable设置为静态的成员属性。注意：一个静态的匿名内部类实例不会持有外部类的引用。 修改后不会导致内存泄露的代码如下

    public class SampleActivity extends Activity {

      /*** Instances of static inner classes do not hold an implicit reference to their outer class./
      private static class MyHandler extends Handler {
        private final WeakReference<sampleactivity> mActivity;

        public MyHandler(SampleActivity activity) {
          mActivity = new WeakReference<sampleactivity>(activity);
        }

        @Override
        public void handleMessage(Message msg) {
          SampleActivity activity = mActivity.get();
          if (activity != null) {
            // ...
          }
        }
      }

      private final MyHandler mHandler = new MyHandler(this);

      /**
       * Instances of anonymous classes do not hold an implicit
       * reference to their outer class when they are static.
      private static final Runnable sRunnable = new Runnable() {
          @Override
          public void run() { /* ... }
      };

      @Override
      protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // Post a message and delay its execution for 10 minutes.
        mHandler.postDelayed(sRunnable, 1000 * 60 * 10);

        // Go back to the previous Activity.
        finish();
      }

    其实在Android中很多的内存泄露都是由于在Activity中使用了非静态内部类导致的，就像本文提到的一样，所以当我们使用时要非静态内部类时要格外注意，如果其实例的持有对象的生命周期大于其外部类对象，那么就有可能导致内存泄露。个人倾向于使用文章的静态类和弱引用的方法解决这种问题。
