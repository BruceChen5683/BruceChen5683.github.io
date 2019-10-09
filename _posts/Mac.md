command+i  默认文件用某软件打开


Startuml 注册http://www.cnblogs.com/Michael-Chen-ibm-com/p/6097495.html

Mac平台重新设置MySQL的root密码
```
您是否忘记了Mac OS 的MySQL的root密码? 通过以下4步就可重新设置新密码：


1.  停止 mysql server.  通常是在 '系统偏好设置' > MySQL > 'Stop MySQL Server'


2.  打开终端，输入：


     sudo /usr/local/mysql/bin/mysqld_safe --skip-grant-tables


3.  打开另一个新终端，输入:


     sudo /usr/local/mysql/bin/mysql -u root


     UPDATE mysql.user SET authentication_string=PASSWORD('新密码') WHERE User='root';


     FLUSH PRIVILEGES;


     \q


4.  重启MySQL.

*以上方法针对 MySQL V5.7.9, 旧版的mysql请使用：UPDATE mysql.user SET Password=PASSWORD('新密码') WHERE User='root';
```



###### handler

1. can potentially create leaks

```
/*
 * Set this flag to true to detect anonymous, local or member classes
 * that extend this Handler class and that are not static. These kind
 * of classes can potentially create leaks.
 */
private static final boolean FIND_POTENTIAL_LEAKS = false;


if (FIND_POTENTIAL_LEAKS) {
        final Class<? extends Handler> klass = getClass();
        if ((klass.isAnonymousClass() || klass.isMemberClass() || klass.isLocalClass()) &&
                (klass.getModifiers() & Modifier.STATIC) == 0) {
            Log.w(TAG, "The following Handler class should be static or leaks might occur: " +
                klass.getCanonicalName());
        }
    }

```
2. 默认同步synchronous,可设置异步asynchronous
```
/**
 * Use the {@link Looper} for the current thread
 * and set whether the handler should be asynchronous.
 *
 * Handlers are synchronous by default unless this constructor is used to make
 * one that is strictly asynchronous.
 *
 * Asynchronous messages represent interrupts or events that do not require global ordering
 * with respect to synchronous messages.  Asynchronous messages are not subject to
 * the synchronization barriers introduced by {@link MessageQueue#enqueueSyncBarrier(long)}.
 *
 * @param async If true, the handler calls {@link Message#setAsynchronous(boolean)} for
 * each {@link Message} that is sent to it or {@link Runnable} that is posted to it.
 *
 * @hide
 */
public Handler(boolean async) {
    this(null, async);
}
```

3. Looper.getMainLooper()  Looper.myLooper() ????
```
@hide getMain   mainIfNull   getTraceName ???
```
4. obtainMessage **More efficient than
 creating and allocating new instances.**
 ```
 /**
     * Returns a new {@link android.os.Message Message} from the global message pool. More efficient than
     * creating and allocating new instances. The retrieved message has its handler set to this instance (Message.target == this).
     *  If you don't want that facility, just call Message.obtain() instead.
     */
    public final Message obtainMessage()
    {
        return Message.obtain(this);
    }
 ```

5. sendMessageAtTime

**The time-base is {@link android.os.SystemClock#uptimeMillis}.</b>
Time spent in deep sleep will add an additional delay to execution.**

**Note that a result of true does not mean the Runnable will be processed -- if the looper is quit before the delivery time of the message occurs then the message will be dropped.**
```
/**
  * Causes the Runnable r to be added to the message queue, to be run
  * at a specific time given by <var>uptimeMillis</var>.
  * <b>The time-base is {@link android.os.SystemClock#uptimeMillis}.</b>
  * Time spent in deep sleep will add an additional delay to execution.
  * The runnable will be run on the thread to which this handler is attached.
  *
  * @param r The Runnable that will be executed.
  * @param uptimeMillis The absolute time at which the callback should run,
  *         using the {@link android.os.SystemClock#uptimeMillis} time-base.
  *  
  * @return Returns true if the Runnable was successfully placed in to the
  *         message queue.  Returns false on failure, usually because the
  *         looper processing the message queue is exiting.  Note that a
  *         result of true does not mean the Runnable will be processed -- if
  *         the looper is quit before the delivery time of the message
  *         occurs then the message will be dropped.
  */
 public final boolean postAtTime(Runnable r, long uptimeMillis)
 {
     return sendMessageAtTime(getPostMessage(r), uptimeMillis);
 }
```

6. sendMessageAtFrontOfQueue  ** This method is only for use in very special circumstances **
```
/**
 * Posts a message to an object that implements Runnable.
 * Causes the Runnable r to executed on the next iteration through the
 * message queue. The runnable will be run on the thread to which this
 * handler is attached.
 * <b>This method is only for use in very special circumstances -- it
 * can easily starve the message queue, cause ordering problems, or have
 * other unexpected side-effects.</b>
 *  
 * @param r The Runnable that will be executed.
 *
 * @return Returns true if the message was successfully placed in to the
 *         message queue.  Returns false on failure, usually because the
 *         looper processing the message queue is exiting.
 */
public final boolean postAtFrontOfQueue(Runnable r)
{
    return sendMessageAtFrontOfQueue(getPostMessage(r));
}
```

7. runWithScissors ** This method is dangerous!  Improper use can result in deadlocks.  Never call this method while any locks are held or use it in a
possibly re-entrant manner.    One example of where you might want to use this method is when you just set up a Handler thread and need to perform some initialization steps on it before continuing execution. **
```
/**
 * Runs the specified task synchronously.
 * <p>
 * If the current thread is the same as the handler thread, then the runnable
 * runs immediately without being enqueued.  Otherwise, posts the runnable
 * to the handler and waits for it to complete before returning.
 * </p><p>
 * This method is dangerous!  Improper use can result in deadlocks.
 * Never call this method while any locks are held or use it in a
 * possibly re-entrant manner.
 * </p><p>
 * This method is occasionally useful in situations where a background thread
 * must synchronously await completion of a task that must run on the
 * handler's thread.  However, this problem is often a symptom of bad design.
 * Consider improving the design (if possible) before resorting to this method.
 * </p><p>
 * One example of where you might want to use this method is when you just
 * set up a Handler thread and need to perform some initialization steps on
 * it before continuing execution.
 * </p><p>
 * If timeout occurs then this method returns <code>false</code> but the runnable
 * will remain posted on the handler and may already be in progress or
 * complete at a later time.
 * </p><p>
 * When using this method, be sure to use {@link Looper#quitSafely} when
 * quitting the looper.  Otherwise {@link #runWithScissors} may hang indefinitely.
 * (TODO: We should fix this by making MessageQueue aware of blocking runnables.)
 * </p>
 *
 * @param r The Runnable that will be executed synchronously.
 * @param timeout The timeout in milliseconds, or 0 to wait indefinitely.
 *
 * @return Returns true if the Runnable was successfully executed.
 *         Returns false on failure, usually because the
 *         looper processing the message queue is exiting.
 *
 * @hide This method is prone to abuse and should probably not be in the API.
 * If we ever do make it part of the API, we might want to rename it to something
 * less funny like runUnsafe().
 */
public final boolean runWithScissors(final Runnable r, long timeout) {
    if (r == null) {
        throw new IllegalArgumentException("runnable must not be null");
    }
    if (timeout < 0) {
        throw new IllegalArgumentException("timeout must be non-negative");
    }
    if (Looper.myLooper() == mLooper) {
        r.run();
        return true;
    }
    BlockingRunnable br = new BlockingRunnable(r);
    return br.postAndWait(this, timeout);
}
```

8. removeCallbacks  **token???  
If <var>token</var> is null,all callbacks will be removed.**
```
/**
     * Remove any pending posts of Runnable <var>r</var> with Object
     * <var>token</var> that are in the message queue.  If <var>token</var> is null,
     * all callbacks will be removed.
     */
    public final void removeCallbacks(Runnable r, Object token)
    {
        mQueue.removeMessages(this, r, token);
    }
```

9. hasMessages  hasMessagesOrCallbacks
**Check if there are any pending posts of messages with code 'what' in the message queue. Return whether there are any messages or callbacks currently scheduled on this handler.**
```
   public final boolean hasMessages(int what) {
       return mQueue.hasMessages(this, what, null);
   }
```

10. getLooper??? get rid of this method???
```
    // if we can get rid of this method, the handler need not remember its loop
    // we could instead export a getMessageQueue() method...
    public final Looper getLooper() {
        return mLooper;
    }
```

11. 调用者？
```
final IMessenger getIMessenger() {
    synchronized (mQueue) {
        if (mMessenger != null) {
            return mMessenger;
        }
        mMessenger = new MessengerImpl();
        return mMessenger;
    }
}
```

12.
```
final IMessenger getIMessenger() {
       synchronized (mQueue) {
           if (mMessenger != null) {
               return mMessenger;
           }
           mMessenger = new MessengerImpl();
           return mMessenger;
       }
   }
   private final class MessengerImpl extends IMessenger.Stub {
       public void send(Message msg) {
           msg.sendingUid = Binder.getCallingUid();
           Handler.this.sendMessage(msg);
       }
   }
   private static Message getPostMessage(Runnable r) {
       Message m = Message.obtain();
       m.callback = r;
       return m;
   }
   private static Message getPostMessage(Runnable r, Object token) {
       Message m = Message.obtain();
       m.obj = token;
       m.callback = r;
       return m;
   }
   private static void handleCallback(Message message) {
       message.callback.run();
   }
   final Looper mLooper;
   final MessageQueue mQueue;
   final Callback mCallback;
   final boolean mAsynchronous;
   IMessenger mMessenger;
   private static final class BlockingRunnable implements Runnable {
       private final Runnable mTask;
       private boolean mDone;
       public BlockingRunnable(Runnable task) {
           mTask = task;
       }
       @Override
       public void run() {
           try {
               mTask.run();
           } finally {
               synchronized (this) {
                   mDone = true;
                   notifyAll();
               }
           }
       }
       public boolean postAndWait(Handler handler, long timeout) {
           if (!handler.post(this)) {
               return false;
           }
           synchronized (this) {
               if (timeout > 0) {
                   final long expirationTime = SystemClock.uptimeMillis() + timeout;
                   while (!mDone) {
                       long delay = expirationTime - SystemClock.uptimeMillis();
                       if (delay <= 0) {
                           return false; // timeout
                       }
                       try {
                           wait(delay);
                       } catch (InterruptedException ex) {
                       }
                   }
               } else {
                   while (!mDone) {
                       try {
                           wait();
                       } catch (InterruptedException ex) {
                       }
                   }
               }
           }
           return true;
       }
   }
}
```
